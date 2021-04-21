---
title:  "Reactive"
excerpt: "Reactive"

categories:
  - paradigm
tags:
  - paradigm 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

# 웹 개발 패러다임의 거대한 변화 "Reactive"

***

 막힘 없이 흘러다니는 데이터(이벤트)를 통해 사용자에게 자연스러운 응답을 주고, 규모 탄력적으로 리소스를 사용하며 실패에 있어서 유용하게 대처한다.

# Reactive 패러다임 

***

"막힘없이 흘러다니는 데이터(이벤트)를 통해 사용자에게 자연스러운 응답을 주고, 규모 탄력적으로 리소스를 사용하며 실패에 있어서 유연하게 대처한다."  
"모든 지점에서 블럭되지 않게 하자."  

# Reactive 

***

- "외부의 어떤 이벤트나 데이터가 발생하면 거기에 대응하면 방식으로 동작하는 프로그램을 만드는 것"  
- "데이터 플로우와 상태 변경을 전파한다는 생각에 근간을 둔 프로그래밍 패러다임"  
- "막힘 없이 흘러다니는 데이터(이벤트)를 통해 사용자에게 자연스러운 응답을 주고, 규모 탄력적으로 리소스를 사용하며 실패에 있어서 유연하게 대처한다."   

![](https://keepinmindsh.github.io/lines/assets/img/Reactive.png){: .align-center}

#### Responsive : 사용자에 대한 반응  

  시스템이 적시에 응답합니다. 응답성은 사용성과 기능성의 기반인데, 그것보다 더 응답성은 문제에 대해서 빠르게 감지하는 것과 효율성을 다루는 것에 초점을 둡니다. 반응성(응답성)이 좋은 시스템은 속도와 일정한 응답성을 제공하고 , 신뢰할 수 있는 상향 경계를 수립하므로써, 일정한 품질의 서비스를 제공하는 것에 있습니다. 이 일관성있는 행동은 에러 처리를 간편화 하고, 사용자 신뢰를 구축하고, 앞선 상호작용을 장려합니다.

#### Scalable(Elastic) : 부하에 대한 반응

  시스템은 다양한 작업하에 응답성을 유지합니다. Reactive System은 서비스에 제공되기 위한 입력을 할당한 자원을 증가시키거나 감소시키는 것으로써 입력 량의 변화에 응답할 수 있다. 이것은 경합 포인트나 병목현상을 가지지 않게 설계되었으며, 공유하고, 컴포넌트를 복제하고, 입력을 분산할 수 있도록 하는 결과를 의도합니다. Reactive System은 응답성 뿐만 아니라 예측 가능성을 지원하는데 이는 실시간 성능 측정을 제공하여 알고리즘을 조정할 수 있습니다. 상용 하드웨어 및 소프트웨어 플랫폼에서 비용 효율적인 방식으로 탄력성을 얻을 수 있습니다.

#### Resillent : 실패 상황에 대한 반응

  시스템은 장애가 발생하더라도 응답성을 유지합니다. 이는 고 가용성, Mission Critical 시스템에 적용됩니다. 탄력성은 복제, 유지, 격리 및 위임에 의해 획득할 수 있습니다. 장애는 각각의 컴포넌트에 포함되어 있기 때문에 각각으로 부터 컴포넌트가 고립하되는 것으로 시스템 전체에 영향일 미치지 않고, 시스템의 일부가 장애 및 복구되는 것을 보장할 수 있습니다. 각각의 컴포넌트의 복수는 새로운 컴포넌트에게 위임되고, 고가용성은 필요에 따라 복제 따라서 보장됩니다. 고객의 기능은 장애를 처리함으로써 부담을 받지 않아도 됩니다.

#### Event-Driven( Message-Driven ) : 이벤트에 대한 반응

  Reactive 시스템은 Location Transparency, Isolation, Loose Coupling을 보장하는 컴포넌트들 사이의 경계를 관리하기 위해서 비동기적인 Message 전달 (Asynchronouse message-passing)에 의존합니다. 이 경계는 메세지로서 장애를 위임하기 위한 의도를 제공합니다. 명시적인 Message 전달을 이용하면 부하관리, 탄력성, 흐름제어 및 시스템에서의 메세지 큐 모니터링, 필요에 따라 Back Pressure를 적용하는 것을 가능하게 합니다. 통신수단으로의 Location Transparent Message는 동일한 구조와 의미의 단일 호스트 또는 클러스터와 동작하기 위한 장애의 관리를 가능하게 합니다. Non-Blocking Communication은 수신자로 하여금 활성 상태에서만 자원을 소모할 수 있게 하여 시스템의 오버헤드를 줄일 수 있습니다.


# Functional Reactive Programming 

***
  
  Functional Reactive Programming 은 위에서 살펴본 Reactive Programming을 Functional Programming의 원리를 통해 구현하는 것을 말합니다. 쉽게 말하자면, 비동기적인 데이터 처리를 간단한 함수를 통해 수행하는 프로그래밍을 말합니다. RP에 FP에서 제공하는 함수를 활용하는 것입니다.   

  여기에서 Functional Programming에 대해서 먼저 알아보고 가야하는데, Functional Programming이란 어떤 문제를 해결하는데 있어서 그 과정이나 공식에 매몰되기보다는 그냥 이미 만들어진 함수를 활용하는 방식을 말합니다. 다만, 무조건 활용하는 것이 아니라 함수 자체가 '숨겨진 input과 output'이 없도록 설계되어야 하는 것이 전제 조건입니다.

```java


// side-cause와 side-effect가 존재하는 함수 
// 함수를 콜할 때 implementation detail을 살펴보지 못한다면 무엇이 어떻게 변할지 알 수 없음                                     
func add() {
  number = 5
  letter = "S"
  title = title + " \ (number) " + letter 
}                                         

// 숨겨진 input/ouput이 없는 함수
func add(numberOnt: Int, numberTwo: Int) -> Int {
  return numberOne + numberTwo
}

```

 결국 Functional Programming이라는 것은, 결과에 집중하는 실용적인 함수를 정의하고 활용하되, 이러한 함수안에 숨겨진 input과 output이 최대한 없을 수 있도록 선언하는 프로그래밍 패러다임입니다.  

# Reactive Streams 

***

- Reactive Stream : <https://www.reactive-streams.org/>  
- Reactive Menifesto : <https://www.reactivemanifesto.org>  

 우리가 앞으로 Reactive Programming을 다루기 위해 사용할 Project Reactor 에 대해서 이해하기 전에 반드시 알아야할 내용이 있다.
그것은 현재 Rective Programming을 지원하고 있는 Reactor, RxJava, RxSwift, RxJS, ... 등등의 근간이 되는 *Reactive Stream* 이라는 것이다. 

Reactive Stream은 위의 링크(Reactive Stream 의 링크)에 영어로 설명되어 있는 것을 간단하게 정의해보면, 

 **비동기적인 stream 프로세싱을 논-블록킹 방식의 배압(Back Pressure)를 이용해서 표준을 제공한다.**

  - 위의 굵은 글씨의 내용이 Reactive Stream에서 가장 중요한 내용이며 이를 구현하기 위한 기본적인 개념을 하나씩 알아가보고자 한다. 

Reactive Stream의 GitHub : <https://github.com/reactive-streams/reactive-streams-jvm/tree/v1.0.3>\\