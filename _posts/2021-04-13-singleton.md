---
title:  "Pattern - Singleton"
excerpt: "Gof Design Pattern"

categories:
  - Pattern
tags:
  - Pattern 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

> 실패는 옵션이다. 실패하지 않는다면 당신은 충분한 혁신을 이룰 수 없다. 
> - 앨런머스크 ( 스페이스 X, 테슬라 창업자 )

# 의도 

***

오직 한 개의 클래스 인스턴스 만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공합니다.


# 활용성

***

- 기본 상황
  - 어떤 클래스는 정확히 하나의 인스턴스 만을 갖도록 하는 것이 좋습니다.
  - 시스템에 많은 프린터가 있다 하더라도, 프린터 스풀은 오직 하나여야 합니다.
  - 파일 시스템도, 윈도우 관리자도 오직 하나여야 합니다.
  - 한 회사에서는 하나의 회계 시스템만 운영될 것입니다.

- 필요 사항 
  - 클래스의 인스턴스가 오직 하나여야 함을 보장하고, 잘 정의된 접근 점으로 모든 사용자가 접근할 수 있도록 할 때,
  - 유일한 인스턴스가 서브클래싱으로 확장되어야 하며, 사용자는 코드의 수정없이 확장된 서브 클래스의 인스턴스를 사용할 수 있어야 할 때,

![](https://keepinmindsh.github.io/lines/assets/img/singleton.png){: .align-center}

  - 기본적인 Singleton

  ```java
    package com.moong.realiticdesignpattern;
    
    public class Singleton {
          private static Singleton instance = null;
          public static Singleton instance(){
              if( instance == null ){
                  instance = new Singleton();
              }
              return instance;
          }
          //... 
    }     
  ```

  - 위의 Singleton 예제의 문제점
    1. 스레드 1이 instance()를 호출하고 4번째 줄을 검사하고 있다. 그런데 다음 줄로 넘어가기 전에 클록 틱에 의해 선점되었다.
    2. 스레드 2가 instance()를 호출하고 메소드 전체를 실행한다. 인스턴스가 생성되었다.
    3. 스레드 1이 잠에서 깨어나 인스턴스가 아직 존재하지 않는다고 생각하고(이 스레드는 전에 중지되기 전에 null 테스트를 마쳤다
    4. Singleton의 두 번째 인스턴스를 생성한다
  
  ```java
      public class Singleton2 {
        private volatile static Singleton2 single;
        public static Singleton2 getInstance(){
            if (single == null) {
                synchronized(Singleton2.class) {
                    if (single == null) {
                        single = new Singleton2();
                    }
                }
            }
            return single;
        }
        private Singleton2(){
        }
    }   
  ```

  - static 초기화를 사용하면 오버헤드를 격지 않으면서도 동기화 문제를 해결할 수 있다.

  ```java
      class Singleton3 {
        private static Singleton3 instance = new Singleton3();
        public instance() { return instance; }
        //…
      } 
  ```

  - static 초기화 방식을 사용하지 못하는 경우는 초기화에 필요한 런타임에 인자를 통해 받아야 할 때이다. 예를 들어 Class 로딩 타임에 URL을 받아야 할 경우, 클래스 로딩 타임에 어떤 URL을 사용할 지 모르기 때문이다

  ```java
    class Connection {
      private static URL server;
      public static void pointAt( URL serverUrl){
        server = serverUrl;
      }
    
      private Connection(){
          // ...
          
          // ...
      }
    
      private static Connection instance;
      public synchronized static Connection getInstance(){
          if(instance == null) {
              instance = new Connection();
          }
          return instance;
      }
    }   
  ```

## Deep Thinking for Singleton 

  - Double-Checked Locking(사용하지 말라)
  - Memory Visibility ( 메모리 가시성 ) 와 Memory Barrier ( 메모리 장벽 )
  - Singleton 죽이기
  - static에 대하여 
  - 동시성 제어에 대하여 
  - 함수형 프로그래밍의 이점