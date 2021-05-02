---
title:  "Docker 란?"
excerpt: "Docker for Container"

categories:
  - Virtualization
tags:
  - Virtualization 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

# Docker 

***

![](https://keepinmindsh.github.io/lines/assets/img/docker_logo.png){: .align-center}

### Docker 의 역사 

***

 2013년 3월 산타클라라에서의 Pycon 행사에서 Solomon Hykes 에 의해서 The Future of Linux Container 라는 세션을 발표하면서 세상에 알려졌다.  

 지금이야 아래의 문장에 대해서 쉽게 이해하고 받아들일 수 있었지만, 그 시기에는 획기적인 방식이었다고 이야기할 수 있을 것 같다. 

```shell
 
docker run -a busybox echo hello world

```

Docker가 현재는 컨테이너를 대표하는 기술이지만, 사실 그 이전부터 이와 같은 방식이 나왔고 과거에서부터 어떻게 변해왔는지를 이해하는 것도 중요하다고 생각한다. 

#### 컨테이너 연대기  

***

###### 1979년 - Unix V7 <https://en.wikipedia.org/wiki/Version_7_Unix>

  chroot 시스템 콜이 처음으로 소개되었다. chroot은 root의 폴더 경로를 변경하고, children이 새 지점에 설정되어 운영된다. 해당 장점은 프로세스 격리(독립)을 위한 시작이었다. 

```shell

 chroot - run command or interactive shell with special root directory

```


###### 2000년 - FreeBSD Jail <https://en.wikipedia.org/wiki/FreeBSD_jail>

```shell

  jail -- manage system jails 

```

###### 2001년 - Linux VServer  <https://en.wikipedia.org/wiki/Linux-VServer>

 Linux-VServer는 GNU/Linux 시스템을 위한 가상화를 제공한다. 이것은 커널 레벨 격리(Isolation)의 의해서 구성되어 있으며, 다중의 가상환경을 구성할 수 있다. 
 각각의 가상환경은 필요한 보안을 보장하기 위해서 충분히 격리도어 있으면서, 활용 가능한 자원을 충분히 제공한다. 이들은 동일한 커널 내에서 동작한다. 

###### 2004년 - Solaris Containers 

 Solaris 10 build 51 beta 버전이 2004에 공식적으로 출시되었다. Solaris Container는 자원 제어와 Zone에 의해 제공되는 경계분리 시스템의 조합이다. Zones은 하나의 단일 운영 시스템내에서 완벽히 격리된 가상된 서버로 동작한다. 

###### 2005년 - Open VZ ( Open Virtuzzo )

 가상화, 격리, 자원 관리 를 위한 리눅스 커널에 적용되어 사용되는 리눅스를 위한 운영 시스템 수준의 가상화 기술이다. 

###### 2006년 - Process Containers 

 Google에 의해서 런칭된 제한적이고, 관리 가능한, 격리 자원(CPU, memory, disk I/O, network) 사용의 processes 집합이다. 

###### 2008년 - LXC ( Linux Container ) <https://en.wikipedia.org/wiki/LXC>

 아래는 리눅스 컨테이너에서 사용하는 명령어를 정리해봤다. 참고 - <https://lzone.de/cheat-sheet/LXC>

```shell

lxc-ls           # List existing containers

# Note: all commands take -n  as parameter to specify the container 
lxc-start        # Start and attach
lxc-start -d     # Start in background
lxc-console      # Attach to running container
lxc-stop

lxc-clone <source> <target>
lxc-create -t <template> -f <config file>
lxc-destroy

lxc-execute -n <name> -- <command>  # Run command in new container
lxc-attach  -n <name> -- <command>  # Run command in running container

lxc-monitor    # Monitor containers for state changes
lxc-wait       # Wait for a state change
lxc-info       # Give details on a container

lxc-freeze
lxc-unfreeze

lxc-netstat
lxc-ps


```

###### 2011년 - Warden

 CloudFoundry는 2011년에 Warden을 시작했다. 이전에는 LXC를 사용하는 것을 후에 직접 구현하여 대체했다. Warden은 어떤 운영시스템에 대한 환경도 격리할 수 있다. 하나의 데몬으로 동작하면서, 컨테이너 관리를 위한 API를 제공한다. 

###### 2013년 - LMCTFY ( Let Me Contain That For You )

Google 컨테이터 스택의 오픈 소스 버전으로써 2013년에 시작되었다. 

- 관련 링크 : <https://github.com/google/lmctfy>

###### 2013년 - Docker 

2013년도에 Docker가 나왔을때, 컨테이너들이 대중적인 기술로 인지되고 있었다. 컨테이너를 활발하게 사용하던 시기에 독커의 성장은 어찌보면 당연한 것이었습니다. 

독커 또한 초기에는 LXC를 사용하다가 후에 자신의 내부 라이브러리로 대체되었으며, 이는 컨테이너 관리르 위한 에코시스템을 제공하며, 독립적으로 운영되지 시작한 것에는 의심의 여지가 없습니다. 

###### 2017년 - Container Tools Become Mature 

수백개의 툴이 컨테이너 관리를 더욱 쉽게하기 위해서 개발되었습니다. 2017년도는 실질적으로 많은 사람들이 컨테이너 기반의 관리 및 운영에 대한 경험을 쌓은 
해라고 할 수 있습니다. Kubernetes를 보면, 2016년에 CNCF(Cloud Native Computing Foundation)에 채택된 이후 VMWare, Azure, AWS, 심지어 Docker도 컨테이너를 관리하기 위한 인프라에 대한 지원을 발표했습니다. 

그 속에서 Kubernetes는 복잡한 계층의 어플리케이션을 지원했습니다. Docker는 Kubernetes의 컨테이너 오케스트레이터, Azure, AWS를 지원할것이라고 밣표했습니다. 

###### 2018년 - 황금기

2018년도는 현대적인 소프트웨어 인프라스트럭처를 위한 기반이 된 컨테이너화였다. 

###### 2019년 

- https://containerd.io/
- https://cri-o.io/
