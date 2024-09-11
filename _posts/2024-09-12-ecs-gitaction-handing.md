---
title: "ECS와 GitAction을 이용한 서비스 배포"
excerpt: "cicd"

categories:
  - cicd
tags:
  - cicd
classes: wide
last_modified_at: 2023-09-12T07:45:00-11:00
sidebar:
  nav: "cicd"
---

- [GitHub Action 작업 명세](#github-action-작업-명세)
  - [ECR 내의 설정](#ecr-내의-설정)
  - [AWS 접근을 위한 Secret / Access Key 생성](#aws-접근을-위한-secret--access-key-생성)
  - [ECS 배포는 Task를 기반으로 Git Action에서 프로세스가 동작한다.](#ecs-배포는-task를-기반으로-git-action에서-프로세스가-동작한다)

ECS와 GitAction을 이용해서 기초적인 CI/CD를 구현하면서, 아래의 사례를 이용하여 AWS 내의 ECS와 GitAction을 이용한 배포 방식을 이해보는 시간이 될 수 있었다. 

# GitHub Action 작업 명세 

```yaml
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.

# GitHub recommends pinning actions to a commit SHA.
# To get a newer version, you will need to update the SHA.
# You can also reference a tag or branch, but the action may change without warning.
name: Deploy to Amazon ECS

on:
  push:
    branches:
      - master

env:
  AWS_REGION: {REGION}                   # set this to your preferred AWS region, e.g. us-west-1
  ECR_REPOSITORY: {Amazone Elastic Container Registry의 경로}           # set this to your Amazon ECR repository name
  ECS_SERVICE:  {Elastic Container Service Name}                # set this to your Amazon ECS service name
  ECS_CLUSTER: {Elastic Container Cluster}                 # set this to your Amazon ECS cluster name
  ECS_TASK_DEFINITION: .aws/task-definition.json # set this to the path to your Amazon ECS task definition
  CONTAINER_NAME: repair-backend-container           # set this to the name of the container in the

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up JDK 16
        uses: actions/setup-java@v2
        with:
          java-version: '16'
          distribution: 'temurin'

      - name: Build with Gradle
        run: ./gradlew clean build

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@0e613a0980cbf65ed5b322eb7a1e075d28913a83
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@62f4f872db3836360b72999f4b87f1ff13310f3a

      - name: Build, tag, and push image to Amazon ECR
        id: build-image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        run: |
          # Build a docker container and
          # push it to ECR so that it can
          # be deployed to ECS.
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
          echo "image=$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG" >> $GITHUB_OUTPUT

      - name: Fill in the new image ID in the Amazon ECS task definition
        id: task-def
        uses: aws-actions/amazon-ecs-render-task-definition@c804dfbdd57f713b6c079302a4c01db7017a36fc
        with:
          task-definition: ${{ env.ECS_TASK_DEFINITION }}
          container-name: ${{ env.CONTAINER_NAME }}
          image: ${{ steps.build-image.outputs.image }}

      - name: Deploy Amazon ECS task definition
        uses: aws-actions/amazon-ecs-deploy-task-definition@df9643053eda01f169e64a0e60233aacca83799a
        with:
          task-definition: ${{ steps.task-def.outputs.task-definition }}
          service: ${{ env.ECS_SERVICE }}
          cluster: ${{ env.ECS_CLUSTER }}
          wait-for-service-stability: true
```

## ECR 내의 설정 

- ECR 내에서 Repository를 구성해야 한다. 
  - 초기 세팅을 위해서 구성한 사항이었고, 
    - Image tag를 지정하고, 
    - Lifecycle Policy도 설정하는게 좋다. 
  - 아래의 이미지 경로가 위의 세팅 파일에서 {Amazone Elastic Container Registry의 경로}의 경로가 되어야 한다. 

![](https://keepinmindsh.github.io/lines/assets/img/ecr.png)

## AWS 접근을 위한 Secret / Access Key 생성 

아래의 이미지에서 보여지는 것처럼, 시크릿 및 액세스 키를 Repo > Settings > Secrets and variables > Actions 에서 등록 가능합니다. 

![](https://keepinmindsh.github.io/lines/assets/img/secret_gitactions.png)


## ECS 배포는 Task를 기반으로 Git Action에서 프로세스가 동작한다. 

- Reposistory 내에 지정된 브랜치로 소스가 머지가 되면, 
- GitAction이 동작하면서 
  - 프로젝트를 빌드하고, 
  - Docker 이미지를 생성하고, 
  - 프로젝트 내에 있는 task-definition을 이용해서, 
    - .aws/task-definition.json
  - Task 버전이 업데이트가 되고, 
  - 업데이트 된 버전을 기준으로 Cluster의 Service가 Task 버전을 교체한다. 
    - 교체 방법은 기본적으로는 Rolling Update이며, 
    - 만약 Blue/Green 배포 방식을 적용하고 싶을 경우 추가적인 설정이 필요하다.  

![](https://keepinmindsh.github.io/lines/assets/img/ecs_task.png)

