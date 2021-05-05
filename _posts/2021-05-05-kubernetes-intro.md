---
title:  "Kubernetes Intro"
excerpt: "Service Ochestration"

categories:
  - Container Ochestration
tags:
  - Container Ochestration
classes: wide
last_modified_at: 2019-05-05T14:06:00-05:00
---

Minikube를 이용한 쿠버네티스 맛보기 

***

MacOS 기준 설명 

### MiniKube 설치 

```shell

brew install minikube

```

### 클러스터 시작하기 

```shell

minikube start 

```

### 클러스터와 상호작용하기 

```shell

kubectl get po -A

minikube kubectl -- get po -A

minikube dashboard 

```

### 어플리케이션 배포 

```shell

# 샘플 배포 및 서비스 8080 포트 오픈 
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080

# 서비스 동작여부 확인
kubectl get services hello-minikube

# web browser 상의 서비스 확인 
minikube service hello-minikube

# kubectl을 통한 서비스 포트 포워딩 
kubectl port-forward service/hello-minikube 7080:8080

```

### Load Balancer 배포 

```shell

# LoadBalancer 배포 및 balancer 접근을 위한 minkube tunnel 실행 
kubectl create deployment balanced --image=k8s.gcr.io/echoserver:1.4  
kubectl expose deployment balanced --type=LoadBalancer --port=8080

minikube tunnel

kubectl get services balanced

```

### Cluster 관리 

```shell

minikube pause

minikube stop

minikube config set memory 16384

minikube addons list

minikube start -p aged --kubernetes-version=v1.16.1

minikube delete --all

```