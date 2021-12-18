---
title: "Git Command"
excerpt: "Git"

sidebar:
    nav: "Tools"
categories:
  - Tools
tags:
  - Tools 
classes: wide
last_modified_at: 2021-12-16T08:57:00-21:55:00
---

> 직원의 성장 방향과 회사의 목표에 부합해야 시너지가 발생한다. 

***

##### git Lab에서 origin의 feature branch를 삭제할  경우 

![](https://keepinmindsh.github.io/lines/assets/img/git_0001.png){: .align-center} 

```

git push origin --delete feature/hotelinfo_setting

```

##### local의 feature branch를 삭제할 경우 

##### local의 feature branch를 origin으로 push 할 경우 

![](https://keepinmindsh.github.io/lines/assets/img/git_0002.png){: .align-center} 

```

git push origin feature/oma_api_definition

```

##### git 의 develop branch 변경후 git pull을 하고 git 을 master로 merge 처리 하는 경우 

![](https://keepinmindsh.github.io/lines/assets/img/git_0003.png){: .align-center} 

```

git switch master 

```


![](https://keepinmindsh.github.io/lines/assets/img/git_0004.png){: .align-center} 

```

git merge --no-ff develop

```


![](https://keepinmindsh.github.io/lines/assets/img/git_0005.png){: .align-center} 

```

git push origin master 

```

![](https://keepinmindsh.github.io/lines/assets/img/git_0006.png){: .align-center} 

```

git checkout -b {branch명}  -> Branch 가져와서 세팅하기

```


