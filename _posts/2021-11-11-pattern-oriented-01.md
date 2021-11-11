---
title: "패턴지향 아키텍처 개발"
excerpt: "Pattern Oriented Development"

sidebar:
    nav: "pattern"
categories:
  - Pattern
tags:
  - Pattern 
classes: wide
last_modified_at: 2021-10-22T17:31:00-21:55:00
---

> 모든 결정은 나에 의해서. 

***

### 패턴은 특정 설계 상황에서 반복적으로 발생하는 설계 문제를 제기하며 그 문제에 대한 해법을 제시한다. 

- 혼돈에서 질서로 

고객사의 요구사항 수집 -> 명세서 작성 -> 시스템의 아키텍처 정의   

여기에서 정의하게될 시스템 아키텍처에 대해서 각 패턴별로 정의가 필요하다. 


> Layers 패턴  

애플리케이션을 구조화 하기 위해서 서브 태스크들을 그룹으로 묶어 분해한다. 이때 특정 추상 레벨에 있는 서브 태스크들끼리 묶어서 서브 태스크들을 각각의 그룹으로 분류한다.   

예) 레이어 형태로 이루어진 아키텍처 중 가장 잘 알려진 예제는 네트워크 프로토콜임.   

- OSI 7 계층 모델



> Pipe And Filter 패턴  

데이터 스트림을 처리하는 시스템 구조를 제공한다. 각 프로세싱 단계는 필터 컴포넌트로 캡술화한다. 데이터는 파이프를 통해서 인접한 필터들 간에 전달된다. 


> BlackBoard 패턴  

명확히 정의된 해법 전략이 아직 존재하지 않는 도메인에서 문제를 해결하는데 유용하다. 이 패턴은 부분적인 해법 혹은 대략적인 해법을 수립하기 위해 몇가지 특수한 서브 시스템들의 지식을 조합한다. 


***

- 분산 시스템 
	- Broker 패턴
	- Microkernel 패턴 

***

- 상호작용 시스템 
	- Model View Controller 패턴 
	- Presentation Abstraction Control 패턴

***

- 적응 시스템 
	- Reflection 패턴 
	- Mircokernel 패턴 

***