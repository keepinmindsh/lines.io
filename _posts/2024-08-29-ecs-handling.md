---
title: "Elastic Container Service 구성 후기"
excerpt: "aws"

categories:
  - aws
tags:
  - aws
classes: wide
last_modified_at: 2023-08-04T14:21:00-14:51
sidebar:
  nav: "aws"
---

최근에 ECS를 이용한 CI/CD 프로세스 구성에 대한 인프라 구성 요청이 있어 구성해보게 되었다.    

해당 서비스 관련해서 다양한 이슈가 존재했지만 중요한 부분인 클라우드도 각 클라우드의 핵심개념은 유사하기 때문에 개념을 잘 익히고 있다면 쉽게 처리할 수 있다. 

예를 들면, 

- VPC
- Security Group 
- Certification 
- Container 
- LoadBalancer 

등은 어느하나에 국한된 개념이 아니기에 하나씩 차근차근 본인의 스타일로 정리하는게 중요하다고 생각한다. 

# GitAction ECS 연동하기 

## 진행절차 

### 절차 정리 

- Image 빌드 및 업로드 
  - 이미지의 경우에는 Docker Build시 플랫폼을 정확히 확인해야 한다. 
  - 만약 초기 테스트시 MacOS를 사용하는 경우 arm64로 빌드가 되기 때문에 amd64 계열에서는 정상적으로 동작하지 않기 때문에 platform 설정을 해서 필드한다.  
- ECS 구성 
  - Cluster 구성  
  - Task 정의 
  - Cluster 정의 
  - Service 정의 
    - Task 연결 
    - LoadBalancer 연결 
      - https://repost.aws/ko/knowledge-center/create-alb-auto-register
      - https://aws.amazon.com/ko/blogs/korea/using-static-ip-addresses-for-application-load-balancers/
        - Network LoadBalancer 
        - Application LoadBalancer 
    - Service 연결
- LoadBalancer 구성 
  - Application Load Balancer를 이용해서 ECS로의 서비스를 구성할 수 있다. 
  - Route 53에서 세팅한 도메인/서브 도메인에 대해서 Certification Manager에서 세팅한뒤 해당 인증서를 Application Load Balancer의 443 리스너 포트에 대해서 https 에 대한 인증서를 등록할 수 있다.  


### Amazone ECS 구성 

#### AWS EC2 instance 기반 클러스터 구성 

##### Cluter 구성 

클러스터 구성은 EC2 또는 Fargate로 사용이 가능하다. Cluster 설정시의 스펙은 Task Definition에 정의한 vCPU와 Memory를 확인하여 구성해야한다. 
왜냐하면 Rolling Update의 경우 순간적으로 Task가 2개가 떠야하는 경우에 스펙이 부족하면 정상적인 교체가 이뤄지지 않고 대기 상태에서 배포가 종료되지 않는다.    

ECS는 EKS로 가기전에 간단하게 서비스를 구성해서 사용할 수 있는 방식으로 Task 의 스펙이 하나하나가 너무 클 경우에는 문제가 있을 수 있다.      
그런 방식은 컨테이너 환경 내에서의 인프라 구성에서 추천하는 방식은 아니기 때문에 서비스의 적절한 스펙 정의가 중요하다.   

- 클러스터명 : sample-cluter
  - ec2-instances로 기본 구성 

- 클러스터 구성시 Key Pair 생성 
  - sample_ecs_cluster.pem

클러스터 내에는 다양한 옵션들이 존재하여 필요에 따라 확인해서 추가 설정이 가능하다. 

#### ECR 이미지 업로드 

아래의 작업전에 profile의 설정을 맞춰놓고 진행해야함. 

```shell 
vi ~/.aws/config 
```

```shell

ACCOUNT_ID=$(aws sts get-caller-identity | jq -r .Account)

aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.${region}.amazonaws.com

```

```shell 
docker tag project-name:latest ${ACCOUNT_ID}.dkr.ecr.${region}.amazonaws.com/project-name:latest
```

```shell 
docker push ${ACCOUNT_ID}.dkr.ecr.${region}.amazonaws.com/project-name:latest
```


#### Task Definition 정의 

- 사용할 Task를 정의할 수 있다. 정의시에 위에서 업로드 한 이미지 경로를 기반으로 하여 리소스의 스펙을 정의 할 수 있다. 
  - 추후 설명할 Github Action을 연동할 때 Task의 버전을 증가시키는 방식으로 ECS Cluster 내에서 배포가 이루어진다. 

#### 서비스 생성 

- 실제 서비스될 Task를 정의하고, Blue/Green, Rolling Update 및 오토스케일링에 대한 설정이 가능하다. 

### ECS 외부 접속 허용 

#### Application Load Balancer 세팅 

ECS를 외부에서 접속되도록 하기 위해서 ALB를 구성한다. ALB의 경우에는 Elastic IP에 대한 할당도 가능하며, Route 53에 Record CNAME을 기준으로 도메인 이름도 설정이 가능하다. 

- http / 80 리스너 포트 구성 
- https / 443 리스터 포트 구성 
  - https의 경우 인증서를 사전에 준비하여 생성한다. AWS내의 Certification Manager에서 인증서 발급 또는 외부 인증서를 설정해서 이를 리스너 포트 구성시 사용 가능하다. 

# 참고 사항 / Task Definition 의 정의 

추후에 Task Definition의 경우 Git Action을 통한 이미지 빌드 및 Task 버전 생성을 위해서 반드시 필요하다. Task 생성시 Json 파일을 이용해서 생성도 가능하다. 

```json 
{
    "taskDefinitionArn": "arn:aws:ecs:{region}:{account_id}:task-definition/{task_name}:1",
    "containerDefinitions": [
        {
            "name": "~~~",
            "image": "~~~",
            "cpu": 6144,
            "memory": 16384,
            "memoryReservation": 16384,
            "portMappings": [
                {
                    "name": "{project_name}-container-80-tcp",
                    "containerPort": 80,
                    "hostPort": 80,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ],
            "essential": true,
            "environment": [],
            "environmentFiles": [],
            "mountPoints": [],
            "volumesFrom": [],
            "ulimits": [],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "/ecs/{project_name}",
                    "awslogs-create-group": "true",
                    "awslogs-region": "{region}",
                    "awslogs-stream-prefix": "ecs"
                },
                "secretOptions": []
            },
            "healthCheck": {
                "command": [
                    "CMD-SHELL",
                    "curl -f http://localhost/actuator/health || exit 1"
                ],
                "interval": 30,
                "timeout": 5,
                "retries": 3
            },
            "systemControls": []
        }
    ],
    "family": "{task_name}",
    "taskRoleArn": "arn:aws:iam::{account_id}:role/ecsTaskExecutionRole",
    "executionRoleArn": "arn:aws:iam::{account_id}:role/ecsTaskExecutionRole",
    "networkMode": "awsvpc",
    "revision": 1,
    "volumes": [],
    "status": "ACTIVE",
    "requiresAttributes": [
        {
            "name": "ecs.capability.execution-role-awslogs"
        },
        {
            "name": "com.amazonaws.ecs.capability.ecr-auth"
        },
        {
            "name": "com.amazonaws.ecs.capability.docker-remote-api.1.21"
        },
        {
            "name": "com.amazonaws.ecs.capability.task-iam-role"
        },
        {
            "name": "ecs.capability.container-health-check"
        },
        {
            "name": "ecs.capability.execution-role-ecr-pull"
        },
        {
            "name": "com.amazonaws.ecs.capability.docker-remote-api.1.18"
        },
        {
            "name": "ecs.capability.task-eni"
        },
        {
            "name": "com.amazonaws.ecs.capability.docker-remote-api.1.29"
        },
        {
            "name": "com.amazonaws.ecs.capability.logging-driver.awslogs"
        },
        {
            "name": "com.amazonaws.ecs.capability.docker-remote-api.1.24"
        },
        {
            "name": "com.amazonaws.ecs.capability.docker-remote-api.1.19"
        }
    ],
    "placementConstraints": [],
    "cpu": "6144",
    "memory": "16384",
    "runtimePlatform": {
        "cpuArchitecture": "X86_64",
        "operatingSystemFamily": "LINUX"
    },
    "tags": []
}
```

# References

- https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth 
- https://sh-t.tistory.com/62
- https://chinggin.tistory.com/875
- https://junuuu.tistory.com/803
- https://chinggin.tistory.com/875
- https://a3magic3pocket.github.io/posts/aws-rds-allow-ec2/
- https://velog.io/@gun_123/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4-ECS%EB%A1%9C-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0
- https://rok93.tistory.com/entry/SpringBoot-Application-ECS-%EB%B0%B0%ED%8F%AC

### Elastic IP 고정해서 사용하기 

- https://docs.aws.amazon.com/ko_kr/AmazonECS/latest/developerguide/alb.html
- https://goodahn.tistory.com/55#google_vignette
- https://aws.amazon.com/ko/blogs/korea/using-static-ip-addresses-for-application-load-balancers/