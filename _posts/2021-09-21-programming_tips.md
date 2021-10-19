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

### Joel On Software 

##### 장인정신, 99%의 완성 상태에서 1%의 완성도를 위해서 들어가는 비싸지만 다른 기업과의 경쟁에서 경쟁우위를 점할 수 있는 핵심 요인 

##### 검색, 검색에서의 진짜 문제는 검색 결과를 정렬하는 방법 

> [Page Rank Algorithim]  <https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EC%A7%80%EB%9E%AD%ED%81%AC>

- Page Rank에 대한 논문을 확인할 수 있는 링크 ( 아래 링크 참조 )

> 문서출처: 이명헌 경영스쿨 http://www.emh.co.kr/xhtml/google_pagerank_citation_ranking.html

##### Anti-Aliasing 문자 

- 컬러 디스플레이의 해상도가 낮을 때, 회색 그림자를 사용해서 해상도 '착각'을 불러 일으키는 편이 보기에 좋다는 생각해서 출발하였음. 

> https://blog.naver.com/marasyl/222494098324

##### 네트워크 투명성 

- 유명한 예제 : RPC (원격 프로시져 호출)

### UNC - Universal Naming Convention

UNC[유엔씨]는 컴퓨터 내의 공유 파일이 저장되어 있는 장치를 명시하지 않고서도, 그 파일을 확인하기 위한 방법이다. 윈도우 운영체계, 노벨 네트웨어, 그리고 어쩌면 많은 다른 운영체계들에서도, 자체적인 명명 시스템 대신 UNC가 사용될 수 있다.

### CopyFile 메소드 

> https://docs.microsoft.com/ko-kr/dotnet/api/microsoft.visualbasic.fileio.filesystem.copyfile?view=net-5.0

### FtpOpenFile

> https://docs.microsoft.com/en-us/windows/win32/api/wininet/nf-wininet-ftpopenfilea
> https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tipsware&logNo=100200000637