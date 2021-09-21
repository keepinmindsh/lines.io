---
title: "Programming Tips"
excerpt: "프로그래밍시 알게 되는 용어들"

categories:
  - Basic
tags:
  - Basic
classes: wide
last_modified_at: 2021-09-20T17:31:00-21:55:00
---

> 집중하고 싶다면 우선 주위에 있는 것들을 버려라. 그래야 현재 내가 가지고 있는 하나에만 집중할 수 있게 된다. 

***

### CRC-32
 
CRC-32은 Cyclic Redundancy Check (CRC) 방식중의 하나이며 CRC 알고리즘 계산을 추진할 때 CRC32 다항식 (Polynomial) 을 사용한다. CRC는 네트워크 데이터블록 또는 파일 등 데이터에 근거하여 짧고 간단하며 자리수가 고정 된 검증코드를 발생시키는 채널코딩기술 (Channel Coding Techniques) 이며 주로 데이터 전송 또는 저장 후에 발생할 수 있는 오류 등을 감지 또는 검증하는데 사용된다. CRC는 1961년에 미국의 정보와 컴퓨터과학 전문가 W. Wesley Peterson 이 발명하였다. 

### Modulo 연산 

나머지(영어: remainder)는 산술에서 두 정수의 나눗셈 이후, 온전한 정수 몫으로 표현할 수 없이 남은 양을 가리킨다. 잉여(剩餘)라고도 한다.

```

A/B=Q remainder R 

이때 A는 피제수(dividend)라고 부른다.

B는 제수(divisor) 라고 부른다.

Q는 몫(quotient)라고 부른다.

R는 나머지(remainder)라고 부른다.

```

나머지 연산은 modulo라고 하며 mod라고 나타낸다.

7 mod 2 = 17 mod 2=1 이다. 왜냐면 7을 2로 나누면 몫이 3이고 나머지가 1이므로, 이 나머지 값이 모듈러 연산의 결과값이 된다.


> 출처: <https://eine.tistory.com/entry/수학-나머지Modulo-연산-1> [아인스트라세의 SW 블로그]