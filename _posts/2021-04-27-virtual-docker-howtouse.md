---
title:  "Docker 의 기본 명령어를 알아보자."
excerpt: "Docker for Container"

sidebar:
    nav: "virtualization"
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

# Docker File 명령어

***

### .dockerignore 

 Docker file과 같은 디렉터리에 들어 있는 모든 파일을 컨텍스트 (Context)라고 합니다. 특히 이미지를 생성할 때 컨텍스트를 모두 Docker 데몬에 전송하므로 필요 없는 파일이 포함되지 않도록 주의 합니다. 컨텍스트에서 파일이나 디렉터리를 제외하고 싶을 때는 .dockerimage 파일을 사용하면 됩니다. 

- .dockerignore 파일안에 아래와 같이 정의할 수 있습니다.  

```shell

 example/hello.txt 
 example/*.cpp 
 wo* 
 *.cpp 
 .git 
 .svn  

```

### FROM

```shell

FROM &lt;image>:&lt;tag>
FROM ubuntu:16.04

```

베이스 이미지를 지정합니다. 반드시 지정해야 하며 어떤 이미지도 베이스 이미지가 될 수 있습니다. tag는 될 수 있으면 latest(기본값)보다 구체적인 버(16.04등)을 지정하는 것이 좋습니다. 이미 만들어진 다양한 베이스 이미지는 Docker hub에서 확인할 수 있습니다.

FROM은 어떤 이미지를 기반으로 이미지를 생성할지 설정합니다. Dockerfile로 이미지를 생성할 때는 항상 기존에 있는 이미지를 기반으로 생성하기 때문에 FROM은 반드시 설정해야합니다.  

FROM <이미지>:<태그> 
앞에서 설명한 것 처럼 FROM 은 항상 설정해야 하고 맨 처음에 와야 합니다. 


### MAINTAINER

```shell

MAINTAINER <name>
MAINTAINER subicura@subicura.com

```

Dockerfile을 관리하는 사람의 이름 또는 이메일 정보를 적습니다. 빌드에 딱히 영향을 주지는 않습니다. 
MAINTAINER <작성자 정보> 형식으로 생략할 수 있습니다. 

### RUN

RUN 은 FROM 에서 설정한 이미지 위에서 스크립트 혹은 명령을 실행합니다. 여기서 RUN 으로 실행한 결과가 새 이미지로 생성되고, 실행 내역은 이미지의 히스토리에 기록됩니다. 

```shell

RUN <command>
RUN ["executable", "param1", "param2"]
RUN bundle install

```

가장 많이 사용하는 구문입니다. 명령어를 그대로 실행합니다. 내부적으로 /bin/sh -c 뒤에 명령어를 실행하는 방식입니다.

```shell

# bin/sh 로 명령 실행하기 
> Dockerfile 
RUN apt-get install -y nginx 
RUN echo "Hello Docker " > /tmp/hello 
RUN curl -sSL https://golang.org/dl/go1.3.1.src.tar.gz | tar -v -C /usr/local -xz 
RUN git clon https://github.com/docker/docker.git 


```

 위와 샘플과 같이 셸 스크립트 구문을 사용할 수 있습니다. FROM으로 설정한 이미지에 포함된 /bin/sh 실행 파일을 사용하게 되며, /bin/sh 실행 파일이 없으면 사용할 수 없습니다.   

 셀 없이 바로 실행하기   

 ```shell

RUN ["apt-get", "install", "-y", "nginx"]
RUN ["/usr/local/bin/hello", "--help"]

 ```

RUN [ "<실행파일>", "<매개변수1>", "<매개변수2>" ] 형식이다.    
FROM 으로 설정한 이미지의 bin/bash 실행 파일을 사용하지 않는 방식이다.   

### CMD

```shell

CMD ["executable","param1","param2"]
CMD command param1 param2
CMD bundle exec ruby app.rb

```

도커 컨테이너가 실행되었을 때 실행되는 명령어를 정의합니다. 빌드할 때는 실행되지 않으며 여러 개의 CMD가 존재할 경우 가장 마지막 CMD만 실행됩니다. 
한꺼번에 여러 개의 프로그램을 실행하고 싶은 경우에는 run.sh파일을 작성하여 데몬으로 실행하거나 supervisord나 forego와 같은 여러 개의 프로그램을 실행하는 프로그램을 사용합니다.

```shell

CMD touch /home/hello/hello.txt 

```

CMD <명령> 형식이며 셸 스크립트 구분을 사용할 수 있습니다.   
FROM 으로 설정한 이미지에 포함된 bin/sh 실행파일을 사용하게 되며 /bin/sh 실행 파일이 없으면 사용할 수 없습니다.   

셸 없이 바로 실행하기 

```shell

CMD ["redis-server"]

```


셸 없이 실행할 때 매개 변수 설정하기 