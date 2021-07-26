---
title: "Performance Engineering"
excerpt: "퍼포먼스 향상을 위한 기본적인 용어의 이해"

categories:
  - profiling
tags:
  - profiling 
classes: wide
last_modified_at: 2021-07-27T22:49:00-05:00
---

> 늘 고민할 것. 

***

# Performance 

 기업의 성장 단계에 따라서 성능에 대한 중요도가 달라진다. 초기 단계의 기업에서 성능은 중요하지 않을 수 있고, 기업이 성장하고 시스템의 사용자가 증가함에 따라 성능에 대한 요구사항이 커지는데, 이때 우리는 성능을 향상 시킬 수 있는 방법등을 구글링을 통해서 알아보거나 웹사이트/책/유투브 등을 통해서 공부하고 이를 단계적으로 적용 시킨다고 볼 수 있다.  

필자가 있는 회사에서도 초기 회사의 구조상 성능에 대한 요소보다는 사용자의 유입과 빠른 시스템 확장 및 기능 제공이 중요해졌고, 이에 대해서 초점을 맞춰 업무를 수행하였지만, 
시간이 지남에 따라 점차 솔수션의 사용자 증가로 인한 성능 이슈, 자원 증가 등의 문제를 해결해야만 했다.  

하지만 이를 위해서 성능을 측정하고 관리하는 것에 대한 경험이 전무했기에 기본적인 용어에서부터 성능을 측정하는 방법들에 대해서 고민하기 시작했다.  

아래의 내용들을 그 과장 중에 공부한 내용을 바탕으로 정리한 내용이다. 

# TPS ( Transaction Per Second )

- wikipedia 

초당 트랜잭션 수(transactions per second, TPS)는 일반적인 관점에서 초당 특정 엔티티가 수행한 원자 동작의 수를 가리킨다. 더 제한된 관점에서 이 용어는 DBMS 벤더와 사용자 공동체가 초당 데이터베이스 트랜잭션의 수를 가리키기 위해 사용되는 것이 보통이다.  

T / S = TPS  

- T : Transaction 수 
- S : 초
- TPS : 초당 Transaction 수 

# TPS ( Throughput per Second )

- wikipedia 

스루풋(throughput) 또는 처리율(處理率)은 통신에서 네트워크 상의 어떤 노드나 터미널로부터 또 다른 터미널로 전달되는 단위 시간당 디지털 데이터 전송으로 처리하는 양을 말한다.   



# PeakTime

 서버가 순간적으로 가장 부하를 많이 받는 시점으로 서버의 용량 산정이나 성능 설계는 이 시간의 부하량을 기준으로 한다. 

# HPS ( Hit Per Second )

  초당 사용자에 의해서 웹서버로 전달되는 HTTP Request의 수를 정의한다. 

# APM ( Application Performance Management )

  Application 의 성능을 관리하는 서비스  

  - traceview <https://www.solarwinds.com/ko/traceview>
  - pinpoint <https://github.com/pinpoint-apm/pinpoint>
  - datadog <https://www.datadoghq.com/>
  - Sematest API <https://sematext.com/apm/>
  - manageengine <https://www.manageengine.com/>
  - Site24x7 <https://www.site24x7.com/application-performance-monitoring.html>
  - NewRelic <https://newrelic.com/>
  - AppDynamics <https://www.appdynamics.com/>
  - opsview <https://www.opsview.com/>
  - stackify <https://stackify.com/retrace-application-performance-management/>

등 의 다양한 APM 툴이 제공되고 있음. 각 서비스 별로 오픈 소스로 운영되는 곳도 있으며 대부분은 월단위 구독형 서비스를 제공한다. 


# 관련 용어 

- Saturation Point ( 임계점 )
- Response Time ( 응답시간 )
- Active User ( 활성 사용자 )
- Concurrent User ( 동지 접속자 )
- Peak Time ( 최대 성능 )
- Proof of Concept 


# 참조 링크 

- https://bcho.tistory.com/787
- https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A3%A8%ED%92%8B
- 