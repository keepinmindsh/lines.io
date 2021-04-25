---
title:  "Slack - Git 연동하기"
excerpt: "Slack - Git 연동하기"

categories:
  - slack
tags:
  - slack 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

# Slack과 Git을 연동하기 

***

Slack에 Git을 연동하여 issues, pulls, commits, releases 시 메세지 처리를 받을 수 있는 방법은 다양합니다. 그 중 github app을 추가하여 slack에서 git 메세지를 받을 수 있는 방법에 대해서 알아보겠습니다. 

현재의 포스팅은 2021년 4월 26일 작성된 것으로 이후 Slack UI 및 프로세스는 변경될 수 있습니다. 
{: .notice--info}

####  사전 준비사항 

- Slack App 설치 
- 가이드 링크 : <https://github.com/integrations/slack>

####  Slack에 Github App 추가 

- Slack의 Apps로 들어가기

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-001.png){: .align-center}

- Slack의 github 찾기 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-002.png){: .align-center}

- github 선택해서 Add to Slack 버튼 클릭하기 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-003.png){: .align-center}

- Allow 선택 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-004.png){: .align-center}

- 만약 이미 슬랙이 App으로 설치되어 있는 경우 Slack App 실행 여부가 표시된다. 선택하고 다음 단계 진행 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-005.png){: .align-center}

- App 설치 확인

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-006.png){: .align-center}

####  Github App에 GitHub Account 연결 

Slack에서 추가된 GitHub App을 통해서 메세지를 받기 위해서는 GitHub Account는 연결해야 합니다. 

사전에 본인 계정의 GitHub 계정이나 회사의 GitHub 계정이 있다는 전제하에 진행됩니다. 
{: .notice--info}

github 연동 명령어 

'''

/github signin 

'''

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-007.png){: .align-center}

- Connect Github account 클락합니다. 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-008.png){: .align-center}

- 이후 Slack의 github app에서 본인 계정의 인증을 위해서 인증 코드를 입력해줘야 합니다. 인증 코드는 진행 과정상에 제공되니 해당 인증코드를 복사하여 입력합니다. 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-009.png){: .align-center}

#### 원하는 채널에서 메세지 받기 

- Github App에서 설정을 통해 "Add this app to a channel"을 선택합니다.

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-0101.png){: .align-center}

- 내가 사용하고 싶은 Channel을 선택합니다. 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-010.png){: .align-center}

#### Channel에서 Github Repository 구독하기 

우리가 원하는 channel에서 이제 Git Action Message를 받으려고 합니다. 

```

/github subscribe owner/repo [feature]
/github unsubscribe owner/repo [feature]

```

- subscribe를 통해 해당 github 계정의 repository를 구독합니다. 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-011.png){: .align-center}

- 구독 처리 완료후 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-012.png){: .align-center}


#### git을 통해서 push 후에 Slack 알림 확인


- git commit/push 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-014.png){: .align-center}

- Slack 알림 

![](https://keepinmindsh.github.io/lines/assets/img/slack-github-013.png){: .align-center}