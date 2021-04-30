---
title:  "Docker 기본 "
excerpt: "Docker for Container"

categories:
  - Virtualization
tags:
  - Virtualization 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

# Docker 기본

***

#### Docker 버전 체크 

 Docker의 명령은 docker {명령} 형식이며, Root 권한으로 이용해야 한다. 

```shell

$ docker --version

```

Docker는 Docker Hub를 통해 이미지를 공유하는 생태계가 구축되어 았다. 리눅스 배포 판 부터 오픈 소스 기반의 환경이 구축되어 있는 Image를 제공하고 있다. docker search를 이용한 검색

#### Pull 명령으로 이미지 받기

```shell

$ docker search nodejs
$ docker pull ubuntu:latest

```
pull 명령으로 이미지 받기    
docker pull {이미지 이름}:{태그}  

#### Images 명령으로 이미지 목록 출력하기

```shell

$ docker images / docker image ls

```