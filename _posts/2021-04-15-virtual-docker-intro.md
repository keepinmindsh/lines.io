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

##### 컨테이너 연대기  

1979년 - Unix V7 <https://en.wikipedia.org/wiki/Version_7_Unix>

  chroot 시스템 콜이 처음으로 소개되었다. chroot은 root의 폴더 경로를 변경하고, children이 새 지점에 설정되어 운영된다. 해당 장점은 프로세스 고립(독립)을 위한 시작이었다. 

```shell

 chroot - run command or interactive shell with special root directory

```


2000년 - FreeBSD Jail <https://en.wikipedia.org/wiki/FreeBSD_jail>

```shell

  jail -- manage system jails 

```

2008년 - LXC ( Linux Container ) <https://en.wikipedia.org/wiki/LXC>

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