---
title:  "Terminal을 Custom하게 꾸미기"
excerpt: "Terminal"

categories:
  - Mac
tags:
  - Mac
classes: wide
last_modified_at: 2019-05-06T14:15:00-05:00
layout: single
---

맥북에서 가장 많이 사용하게 되는 Terminal을 사용하게 좋게 꾸미는 방법에 대해서 정리해봤다. 

***

![](https://keepinmindsh.github.io/lines/assets/img/iterm2.png){: .align-center} 

iterm2는 기본적으로 설치되어 있어야 한다. 

### zsh 설치 

```shell

# zsh install
brew install zsh

```

### oh-my-zsh 설치

```shell

# oh-my-zsh install
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

```

### theme 적용 

```shell

# curl이 설치되어 있지 않은 경우
brew install curl

# font를 다운 받을 경로 생성 - 주로 home 폴더 내에서 세팅하면 된다. 
mkdir theme && cd theme

# snazzy color theme를 download
# 만약 다른 color 테마를 다운로드 할 경우 curl -LO 이후에 해당 URL을 넣으면 됨
curl -LO https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Snazzy.itermcolors


```

iterm에서 
- preferences 클릭
- profiles 클릭
- colors 클릭 
- 창의 우측 하단에 color preset에서 자신이 다운받은 경로에 있는 theme을 import 한다.
- import 설정 

### theme 변경 

agonster 적용

- vim 편집기를 이용해서 아래의 명령어 실행

```shell

vi ~/.zshrc 

```

- 해당 파일내에 ZSH_THEME을 찾아 "agnoster"로 적용

```shell

ZSH_THEME="agnoster" 로 변경 

```

### 폰트 적용

기본 폰트를 사용하게 되면 터미널을 열었을때 폰트 깨짐현상을 확인할 수 있다. 

Meslo LGL Nerd Font Mono 설치 

설치 방법은 brew & cask를 이용하면 쉽게 설치가 가능하다. 

```shell

# cask 가 설치 안되었을 경우 
brew install cask

# font 설치 
brew install --cask font-meslo-lg-nerd-font

```

폰트 설치 후 

- preference 열기
- Profiles 열기 
- Text 열기 
  Font / Non-ASCII Font를 모두 Meslo LGL Nerd Font Mono로 변경 할 것 

### 터미널 상의 사용자 이름 삭제 

- zshrc 오픈 

```shell

vi /.zshrc

```
- 파일의 맨 아래에 아래의 코드를 복사하여 추가 

```shell

prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
  fi
}

```
### New Line 적용하기 

- agnoster 테마를 설치했으므로 해당 theme의 파일 수정 필요 

```shell

vi ~/.oh-my-zsh/themes/agnoster.zsh-theme

```

- 파일내에서 build_promt()를 찾고 그위에 함수 및 라인 추가 

```shell

prompt_newline() {
  if [[ -n $CURRENT_BG ]]; then
    echo -n "%{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR
%{%k%F{blue}%}$SEGMENT_SEPARATOR"
  else
    echo -n "%{%k%}"
  fi

  echo -n "%{%f%}"
  CURRENT_BG=''
}

build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_virtualenv
  prompt_context
  prompt_dir
  prompt_git
  prompt_bzr
  prompt_hg
  prompt_newline # 반드시 추가 필요 ! 순서도 반드시 지켜야함! 
  prompt_end
}

```

이정도의 설정이면 기본적으로 터미널 사용시에 좀더 편하게 사용가능 하다. 사실 개인의 취향에 따라 설정은 원하는대로 변경해서 사용하는 것이 최고!

> 참고링크 : [윤자이 블로그]https://ooeunz.tistory.com/21
