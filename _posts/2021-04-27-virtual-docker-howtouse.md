---
title:  "Docker 의 기본 명령어를 알아보자."
excerpt: "Docker for Container"

categories:
  - Virtualization
tags:
  - Virtualization 
classes: wide
last_modified_at: 2019-05-04T08:06:00-05:00
---

# Docker 명령어

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

#### run 명령어로 컨테이너 생성하기

이미지를 컨테이너로 생성한 후에 Bash 셀로 ubuntu에 접근한다. 

```shell

$ docker run -i -t --name hello ubuntu bin/bash

```

docker run {옵션} {이미지 이름} {실행할 파일}  
-i(interactive)  -t(Pseudo-try) : Bash 셀에 대한 입력 및 출력을 할 수 있다.  
--name : 컨테이너의 이름을 지정한다.  
여기에서 docker run을 통해 접속할 때 bin/bash를 바로 실행하여 들어갔기 때문에 exit 를 통해서 빠져나오면 컨테이너가 정지됩니다.  

```shell

# docker 에서 외부로 연결할 포트를 지정할 경우 
# docker container의 6379포트외 외부 80포트를 연결해준다.
$ docker run -d -p 6379:80 --name lines-redis redis-6379:0.1

$ docker run -it -p 5005:5005 ubuntu

# run : 기동  , --name : 이름 지정 , -d : 백그라운드 기동 , -p : 외부/내부 포트 연결 , redis : 이미지 명 
$ docker run --name line-redis -d -p 6379:6379 redis

```

#### PS 명령으로 컨테이너 목록 확인하기 

ps 명령으로 컨테이너 목록 확인하기. docker ps 형식으로 -a 옵션을 사용하면 정지된 컨테이너까지 모두 출력하고, 옵션을 사용하지 않으면 실행되고 있는 컨테이너만 출력합니다

```shell 

$ docker ps -a / docker container ls

```

#### start 명령으로 컨테이너 시작하기 

start 명령으로 컨테이너 시작하기, docker start를 통해 container를 기동 시키고, docker ps 로 해당 실행된 컨테이너 상태를 확인한다. 

```shell

$ docker start hello 

```

#### restart 명령으로 컨테이너 시작하기

restart 명령으로 컨테이너 시작하기. docker restart를 통해 container를 기동 시키고, docker ps 로 해당 실행된 컨테이너 상태를 확인한다. 

```shell

$ docker restart hello    

```

#### attach 명령으로 컨테이너 시작하기

attach 명령으로 컨테이너 시작하기   
bin/bash를 통해 실행되기 때문에 명령을 자유롭게 입력할 수 있지만, DB나 서버 애플리케이션을 실행하면 입력은 할 수 없고, 출력만 보게된다.  
exit 또는 ctrl + D 를 통해 종료 가능   

```shell

$ docker attach hello 

```

#### exec 명령으로 외부에서 컨테이너 안의 명령 실행하기

exec 명령으로 외부에서 컨테이너 안의 명령 실행하기   
docker exec {컨테이너 이름} {명령} {매개 변수}  
컨테이너가 실행되고 있는 상태에서만 사용할 수 있으며 정지된 상태에서는 사용할 수 없습니다   

```shell

$ docker exec hello echo "Hello World"            

```

#### Stop 명령으로 컨테이너 정지하기

Stop 명령으로 컨테이너 정지하기   
docker stop {컨테이너 이름}   

```shell

$ docker stop hello   

```

#### rm 명령으로 컨테이너 삭제하기

rm 명령으로 컨테이너 삭제하기   
docker rm {컨테이너 이름}   

```shell

$ docker rm hello     

```

#### rmi 명령으로 이미지 삭제하기

rmi 명령으로 이미지 삭제하기   
docker rmi {이미지 이름}:{태그}    

```shell

$ docker rmi hello        

```

### 컨테이너 로그 보기 ( logs )

```shell

docker logs [OPTIONS] CONTAINER

```