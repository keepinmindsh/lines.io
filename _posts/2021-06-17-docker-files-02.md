---
title: "DockerFile"
excerpt: "Docker File의 기본"

categories:
  - Docker
tags:
  - Docker 
classes: wide
last_modified_at: 2021-06-07T22:49:00-05:00
---

> 무항산 무항심, 유항산 유항심 

***

# DockerFile

### Docker의 기본 

Docker file과 같은 디렉터리에 들어 있는 모든 파일을 컨텍스트 (Context)라고 합니다. 특히 이미지를 생성할 때 컨텍스트를 모두 Docker 데몬에 전송하므로 필요 없는 파일이 포함되지 않도록 주의 합니다.
컨텍스트에서 파일이나 디렉터리를 제외하고 싶을 때는 .dockerimage 파일을 사용하면 됩니다.   

.dockerignore  

```shell

.dockerignore 
 example/hello.txt 
 example/*.cpp 
 wo* 
 *.cpp 
 .git 
 .svn 

```

### FROM

```shell

FROM <image>:<tag>
FROM ubuntu:16.04       

```

베이스 이미지를 지정합니다. 반드시 지정해야 하며 어떤 이미지도 베이스 이미지가 될 수 있습니다. tag는 될 수 있으면 latest(기본값)보다 구체적인 버전(16.04등)을 지정하는 것이 좋습니다. 이미 만들어진 다양한 베이스 이미지는 Docker hub에서 확인할 수 있습니다.   
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

RUN 은 FROM 에서 설정한 이미지 위에서 스크립트 혹은 명령을 실행합니다. 여기서 RUN 으로 실행한 결과가 새 이미지로 생성되고,
실행 내역은 이미지의 히스토리에 기록됩니다.

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
한꺼번에 여러 개의 프로그램을 실행하고 싶은 경우에는 run.sh파일을 작성하여 데몬으로 실행하거나 supervisord나 forego와 같은 여러 개의 프로그램을
실행하는 프로그램을 사용합니다.

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

```shell

CMD ["mysql", "--datadir=/var/lib/mysql" , "--user=mysql"]

```

위의 방식은 FROM 이미지로 부터 bin/sh를 사용하지 않는 방식입니다.  

ENTRYPOINT를 사용하였을 때  

```shell

ENTRYPOINT ["echo"]
CMD ["hello"]

```
ENTRYPOINT 에 설정한 명령에 매개변수를 전달하여 실행합니다. DockerFile에 ENTRYPOINT가 있으면 CMD는 ENTRYPOINT에 매개 변수만 전달하는 역할을
합니다. 그래서 CMD 독자적으로 파일을 실행할 수 없게 됩니다.

### ENTRYPOINT

ENTRYPOINT는 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행합니다.  
즉 docker run 명령으로 컨테이너를 생성하거나, docker start 명령으로 정지된 컨테이너를 시작할 때 실행됩니다.  
ENTRYPOINT는 Dockerfile에서 단 한번만 사용할 수 있습니다.  

- 셸(/bin/sh)로 명령 실행하기

```shell

ENTRYPOINT touch /home/hello/hello.txt 

```
               
ENTRYPOINT <명령> 형식이며 셸 스크립트 구문을 사용할 수 있습니다.  
FROM으로 설정한 이미지에 포함된 /bin/sh 실행 파일을 사용하게 되며 /bin/sh 파일이 없으면 사용할 수 없습니다.  

- 셸(/bin/sh) 없이 바로 명령 실행하기

```shell

ENTRYPOINT [ "/home/hello/hello.sh"]

ENTRYPOINT [ "/home/hello/hello.sh", "--hello=1". "--world=2"]

```

- ENTRYPOINT ["/home/hello/hello.sh", "--hello=1", "--world=2"] 형식

```shell

FROM ubuntu:latest 
CMD ["echo", "hello"]

# 컨테이너를 생성할 때 docker run <이미지> <실행할 파일> 형식인데 이미지 다음에 실행할 파일을 설정할 수 있습니다. 
# docker run 명령에서 실행할 파일을 설정하면 CMD는 무시됩니다. 

$ docker build --tag example . 
$ docker run example echo world 

# CMD ["echo", "hello"] 는 무시되고 docker run 명령에서 설정한 echo world가 실행되어 world 가 출력되었습니다. 
# docker run 명령에서 설정한 <실행한 파일>과 Dockerfile의 CMD는 같은 기능입니다. 

# ENTRY POINT의 경우 
FROM ubuntu:latest 
ENTRYPOINT [ "echo", "hello" ]

# ENTRYPOINT [ "echo", "hello" ] 에서 echo hello가 실행되어 hello가 출력되고, docker run 명령에서 설정한 내용이 
# ENTRYPOINT [ "echo", "hello" ] 의 매개 변수로 처리되어 echo world 도 함께 출력됩니다. 

$ echo hello echo world 

$ docker run example 1 2 3 4

# ENTRYPOINT 는 docker run 명령에서 --entrypoint 옵션으로 설정할 수 있습니다. 

# --entrypoint 옵션을 설정하면 Dockerfile에 설정한 ENTRYPOINT 무시 
$ docker run --entrypoint="cat" example /etc/hostname 

```

### Dockerfile 작성하기 / Build 명령으로 이미지 생성하기 

rmi 명령으로 이미지 삭제하기  
docker stop {이미지 이름}:{태그}  

```shell

$ mkdir dockerFile

# 아래와 같은 Docker file을 생성  
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
                       
                  
# requirements.txt
Flask
Redis
                  
                 
# app.py 
rom flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
            "<b>Hostname:</b> {hostname}<br/>" \
            "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
                  
          

# Dockerfile이 저장된 디렉터레에서 다음의 명령을 실행합니다. 
# docker build <옵션> <Dockerfile 경로> 형식 
$ docker build --tag hello-python:0.1 .

```

### hello-python:0.1 이라는 이미지가 생성되었고 이를 실행 시켜 본다.

![](https://keepinmindsh.github.io/lines/assets/img/docker_images.png){: .align-center}

```shell

$ docker run --name hello-flask -d -p 4000:80 hello-python:0.1  

```

기동을 하였을 때, 아래와 같은 이미지가 표시된다.
- -d detached mode 흔히 말하는 백그라운드 모드
- -p 호스트와 컨테이너의 포트를 연결 (포워딩)
- -v 호스트와 컨테이너의 디렉토리를 연결 (마운트)
- -e 컨테이너 내에서 사용할 환경변수 설정
- –name 컨테이너 이름 설정
- –rm 프로세스 종료시 컨테이너 자동 제거
- -it -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션
- –link 컨테이너 연결 [컨테이너명:별칭]

![](https://keepinmindsh.github.io/lines/assets/img/dockerfile_run.png){: .align-center}

### History 명령으로 이미지의 히스토리를 살펴보기 

History 명령으로 이미지의 히스토리를 살펴보기  
docker hsitory <이미지 이름>:<태그>  

```shell

$ docker history hello:0.1

```

### 내가 생성한 이미지의 컨테이너로 부터 파일을 꺼내고 싶을 때 

내가 생성한 이미지의 컨테이너로 부터 파일을 꺼내고 싶을 때  
docker cp <컨테이너이름>:<경로> <호스트경로>  

```shell

$ docker cp hello-nginx:/etc/nginx/nginx.conf ./

```

![](https://keepinmindsh.github.io/lines/assets/img/docker_007.png){: .align-center}

### Commit 명령으로 컨테이너의 변경사항을 이미지로 생성하기 

Commit 명령으로 컨테이너의 변경사항을 이미지로 생성하기  
docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그>  
-a "Foo Bar <foo@bar.com>"  
-m "add hello.txt"  

![](https://keepinmindsh.github.io/lines/assets/img/docker_008.png){: .align-center}

### diff명령으로 컨테이너에서 변경된 파일 확인하기 

diff 명령으로 컨테이너에서 변경된 파일 확인하기  
docker diff <컨테이너 이름>  
A : 추가한 파일  
B : 변경된 파일  
C : 삭제된 파일  

```shell

$ docker diff hello

```

![](https://keepinmindsh.github.io/lines/assets/img/docker_009.png){: .align-center}

### inspect 명령으로 세부 정보 확인하기 

inspect 명령으로 세부 정보 확인하기  
docker inspect <이미지 또는 컨테이너 이름>  

```shell

$ docker inspect hello

```

![](https://keepinmindsh.github.io/lines/assets/img/docker_010.png){: .align-center}

### Docker Logs - 컨네이너에서 발생하는 다양한 로그 확인 

컨테이너의 로그를 확인하는 명령은 docker logs를 사용한다. docker logs 명령은 컨테이너의 표준 출력을 표시함으로써 애플리케이션 상태를 알 수 있다.

```shell

$ docker logs mariadb

# 컨테이너의 로그가 너무 많아 읽기 힘들면 --tail 옵션을 사용하여 마지막 로그부터 지정한 라인까지 출력할 수 있다. 
$ docker logs --tail 10 mariadb 


# --since 옵션은 입력받은 유닉스 시간 이후의 로그를 확인할 수 있으며 -t 옵션으로 타임 스탬프를 표시할 수도 있다. 
$ docker logs --since 15491503000 -t mariadb 

# -f 옵션을 이용하여 실시간 로그 확인 
$ docker logs --tail 10 -f mariadb                       

```

**로깅 드라이버**  
로깅 드라이버는 기본적으로 json-file 로 설정되지만 도커 데몬 시작 옵션에서 --log-drive  

```shell 

$ docker run -it --log-driver none alpine ash

# 실행중인 컨테이너의 로깅 드라이버를 확인하려면 docker inspect 명령을 실행한다. 
$ docker inspect -f '{{.HostConfig.LogConfig.Type}}' <CONTAINER>

$ docker inspect -f '{{.HostConfig.LogConfig.Type}}' mariadb

# 컨테이너 로그는 json 형태로 docker 내부에 저장된다. 
$ /var/lib/docker/cotainers/${CONTAINER_ID}/${CONTAINER_ID}-json.log

# docker 명령을 통해 직접 이용할 경우 
$ docker run --log-opt max-size=100m --log-opt max-file=5 my-lines:lastest

```