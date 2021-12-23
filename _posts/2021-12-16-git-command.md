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

##### git merge 하기 

![](https://keepinmindsh.github.io/lines/assets/img/git_0007.png){: .align-center} 

```shell

git petch origin 

git switch main

git merge --no-ff develop

git push origin main 

```

##### git에서 origin의 feature branch를 삭제할  경우 

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

git merge 샘플 코드 

```shell

D:\01_Sources\02_GIT\sample (develop -> origin)
λ git fetch origin
remote: warning: unable to find all commit-graph files
remote: Enumerating objects: 71, done.
remote: Counting objects: 100% (61/61), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 38 (delta 18), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (38/38), 2.92 KiB | 2.00 KiB/s, done.
From http://sample.gitlabs.com/sample/sample
   3e42447..f4433d3  develop                   -> origin/develop
 * [new branch]      sample-master-patch-28595 -> origin/sample-master-patch-28595
   ddba7c4..64a5a00  master                    -> origin/master

D:\01_Sources\02_GIT\sample (develop -> origin)
λ git checkout -b "sample-master-patch-28595" "origin/sample-master-patch-28595"
Switched to a new branch 'sample-master-patch-28595'
Branch 'sample-master-patch-28595' set up to track remote branch 'sample-master-patch-28595' from 'origin'.

D:\01_Sources\02_GIT\sample (sample-master-patch-28595 -> origin)
λ git fetch origin

D:\01_Sources\02_GIT\sample (sample-master-patch-28595 -> origin)
λ git switch "master"
Switched to branch 'master'
Your branch is behind 'origin/master' by 27 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

D:\01_Sources\02_GIT\sample (master -> origin)
λ git merge --no-ff "sample-master-patch-28595"
Merge made by the 'recursive' strategy.
 Jenkinsfile                                                                    | 179 ++++++++---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 src/main/**************************.java                                       |  64 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++-
 src/main/**************************.java                                       |  49 +++++++++++++++++++++++++++++++++++++++++++++++++
 src/main/**************************.java                                       |  56 +++++++++++++++++++++++++++++++++-----------------------
 src/main/**************************.java                                       |  11 ++++++++++-
 src/main/**************************.properties                                 |   2 +-
 src/main/**************************.properties                                 |   2 +-
 src/main/**************************.xml                                        |   8 +++++---
 src/main/**************************/NotificationScreen.xml                     |   6 ++++--
 src/main/**************************.xml                                        |  16 +++++++++++++---
 10 files changed, 187 insertions(+), 206 deletions(-)
 create mode 100644 src/main/**************************.java

D:\01_Sources\02_GIT\sample (master -> origin)
λ git push origin master
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 245 bytes | 245.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
remote: warning: unable to find all commit-graph files
remote: warning: unable to find all commit-graph files
To http://sample.gitlabs.com/sample/sample.git
   64a5a00..3a31cec  master -> master

D:\01_Sources\02_GIT\sample (master -> origin)


```