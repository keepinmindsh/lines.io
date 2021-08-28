---
title: "Nexus Repository"
excerpt: "넥서스 리포지토리 활용 및 구성"

categories:
  - Dependancy Management 
tags:
  - Dependancy Management 
classes: wide
last_modified_at: 2021-07-29T07:31:00-05:00
---

> 늘 행복한 하루가 되기를.

***

Maven에서 사용가능한 Repository 저장소입니다. 사설 Repository로 사용 할 수 있으며 코드 공유 등에 사용 할 수 있다. 


### Repository Type

모든 Format의 저장소에는 3가지 Type 이 있습니다.

- proxy
말그대로 외부의 다른 경로를 proxy하는 저장소입니다. 이 proxy를 통해 외부(공식 라이브러리 저장소 등)의 데이터를 캐시할 수 있습니다.

- hosted
자체 모듈 저장소입니다.

- group
proxy 와 hosted 들을 묶을 수 있는 단위 집단입니다. 그룹에 저장소를 나열하는 순서가 그룹의 라이브러리를 탐색 우선순위가 됩니다
