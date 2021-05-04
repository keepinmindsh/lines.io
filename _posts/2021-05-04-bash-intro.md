---
title:  "Bash에 대해서"
excerpt: "Docker for Container"

categories:
  - Linux
tags:
  - Linux
classes: wide
last_modified_at: 2019-05-04T22:06:00-05:00
---

# Bash에 대해서 

***

Bash ( Bourne-again shell ) - 본 셸을 대체히는 자유 소프트웨어로서 GNU 프로젝트를 위해 브라이언 폭스가 작성한 유닉스 셸이다. 
1989년에 발표되어 GNU 운영 체제와 리눅스, 맥 OS X 그리고 다윈 등 운영 체제의 기본 셸로 탑재되어 광범위하게 배포되었다. 

***

#### Hello World 

```shell

#!/bin/bash

# This is my first script. 

echo 'Hello World!'

```

- #! : 이 두개의 문자열은 사실 shebang이라고 하는 특별한 조합이다. shebang은 뒤따라오는 스트립트를 실행하기 위한 인터프리터의 이름을 시스템에 알려준다. 모든 쉘 스크립트의 첫 줄에는 shebang이 반드시 포함되어야 한다. 

#### VIM 설정하기 

아래의 옵션들을 적용하면 Shell Script 작성시 도움이 된다. 

~/.vimrc 파일에 아래의 옵션을 추가한다. 

```shell

syntax on # 구문 강조 기능 
set hlsearch # 검색결과를 강조 
set tapstop=4 # 탭 간격을 지정
set autoindent # 자동 들여쓰기 기능 

```

- vim을 통해서 shell 파일을 열었을 때 ( 이미지 참고 )

![](https://keepinmindsh.github.io/lines/assets/img/vimrc.png){: .align-right}