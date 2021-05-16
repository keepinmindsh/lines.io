---
title:  "큐"
excerpt: "큐에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-08T14:15:00-05:00
---

> 훌륭한 개발자가 되는 방법,   
> 내가 훌률한 개발자가 될 수 있을 거라고 스스로를 믿을 것 

***

스택의 경우, 나중에 들어온 데이터가 먼저 나가는 구조인데 반하여 큐는 먼저 들어온 데이터가 먼저 나가는 구조로 이러한 특성을 선입선출(First In First Out)이라고 한다.

###### 큐의 구조 : 선입선출 ( FIFO : First In First Out )

 - 함수에서 실행이 끝나면 자신을 호출한 함수로 되돌아가야한다. 이때 스택이 사용되는데, 즉 복귀할 주소를 기억하는데 사용된다.

### 추상 자료형 큐

 ```c

객체 : 0개 이상의 요소들로 구성된 선형 리스트 

연산 : 
    create(max_size) ::=
        최대 크기가 max_size 인 공백큐를 생성한다. 
    is_empty(q) ::= 
        if(size == max_size) return TRUE;
        else return FALSE;
    is_full(q) ::=
        if(size == max_size ) return TRUE;
        else return FALSE;
    enqueue(q, e) ::=
        if( is_full(q) ) queue_full 오류;
        else q의 끝에 e를 추가한다. 
    dequeue(q) ::=
        if( is_empty(q) ) queue_empty 오류:
        else q의 맨 앞에 있는 e를 제거하여 반환한다. 
    peek(q) ::=
        if( is_empty(q) ) queue_empty 오류;
        else q의 맨 앞에 있는 e를 읽어서 반환한다.                      

 ```

많이 이용되는 분야는 컴퓨터를 이용하여 현실 세계의 실제 상황을 시뮬레이션 하는 것이다, 예를 들면 은행에서 기다리는 사람들의 대기열, 공항에서 이륙하는 비행기들, 인터넷에서 전송되는 데이터 패킷들을 모델링하는 데 큐가 이용된다. 큐는 운영체제에서도 중요하게 사용된다. 예를 들면 운영체제에는 인쇄 작업큐가 존재한다. 프린터는 속도가 늦고 상대적으로 컴퓨터의 CPU는 속도가 빠르기 때문에 CPU는 빠른 속도로 인쇄 데이터를 만든 다음, 인쇄 작업 큐에 저장하고 다른 작업으로 넘어간다.

### 선형 큐 

```c 
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#define MAX_QUEUE_SIZE 5

typedef int element;
typedef struct {
  int front;
  int rear;
  element data[MAX_QUEUE_SIZE];
} QueueType;

void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

void init_queue(QueueType *q){
  q -> rear = -1;
  q -> front = -1;
}

void queue_print(QueueType *q){
  for ( int i = 0; i < MAX_QUEUE_SIZE; i ++ ) {
    if ( i <= q -> front || i > q -> rear ){
      printf(" | ");
    }else{
      printf("%d | ", q -> data[i]);
    }
  }
  printf("\n");
}

int is_full(QueueType *q){
  if( q -> rear == MAX_QUEUE_SIZE - 1 )
    return 1;
  else 
    return 0;
}

int is_empty(QueueType *q){
  if( q -> front ==  q -> rear )
    return 1;
  else 
    return 0;
}

void enqueue(QueueType *q, int item){
  if(is_full(q)) {
    error("큐가 포화상태입니다. ")
    return;
  }
  q -> data[++(q->rear)] = item;
}

int dequeue(QueueType *q){
  if(is_empty(q)){
    error("큐가 공백상태입니다. ");
    return 1;
  }
  int item = q -> data[++(q->front)];
  return item;
}

int main(void){
  int item = 0;
  QueueType q;

  init_queue(&q);

  enqueue(&q, 10); queue_print(&q);
  enqueue(&q, 20); queue_print(&q);
  enqueue(&q, 30); queue_print(&q);

  item = dequeue(&q); queue_print(&q);
  item = dequeue(&q); queue_print(&q);
  item = dequeue(&q); queue_print(&q);
  return 0;
}
{% endraw %}       
```

### 원형큐  

```c
{% raw %} 
* 원형큐에서의 삽입 알고리즘 
enqueue(Q, x):
    rear <- ( rear + 1) % MAX_QUEUE_SIZE;
    Q[rear] <- x;                          

* 원형큐에서의 삭제 알고리즘
    front <- ( front + 1) % MAX_QUEUE_SIZE;
    return Q[front];                          
{% endraw %}
```

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

#define MAX_QUEUE_SIZE 5
typedef int element;
typedef struct {
  element data[MAX_QUEUE_SIZE];
  int front;
} QueueType;

void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

void init_queue(QueueType *q)
{
  q -> front = q -> rear = 0;
}

int is_empty(QueueType *q)
{
  return ( q -> front == q -> rear);
}

int is_full(QueueType *q){
  return ((q -> rear + 1)% MAX_QUEUE_SIZE == q -> front);
}

void queue_print(QueueType *q){
  printf("QUEUE(front=%d rear=%d) = " , q -> front, q -> rear );
  if(!is_empty(q)) {
    int i = q -> front;
    do {
      i = ( i + 1) % ( MAX_QUEUE_SIZE );
      printf("%d | " , q -> data[i]);
      if( i == q -> rear ){
        break;
      }
    } while ( i != q -> front );
  }

  printf("\n");
}

void enqueue(QueueType *q, element item){
  if(is_full(q))
    error("큐가 포화상태입니다. ");
  q -> rear = ( q -> rear + 1) % MAX_QUEUE_SIZE;
  q -> data[q->rear] = item;
}

element dequeue(QueueType *q){
  if(is_empty(q)){
    error("큐가 공백상태입니다.");
  }
  q -> front = ( q -> front + 1) % MAX_QUEUE_SIZE;
}

element peek(QueueType *q){
  if(is_empty(q))
    error("큐가 공백상태입니다.");
  return q -> data[(q->front+1 ) % MAX_QUEUE_SIZE;
}

int main(void){
  QueueType queue;
  int element;

  init_queue(&queue);
  printf("--데이터 추가 단계--\n");
  while(!is_full(&queue)){
    printf("정수를 입력하시오 : ");
    scanf(%d, &element);
    enqueue(&queue, element);
    queue_print(&queue);
  }
  printf("큐는 포화상태입니다. \n\n");

  printf("--데이터 삭제 단계--\n");
  while(!is_empty(&queue)){
    element = dequeue(&queue);
    printf("꺼내진 정수 : %d \n", element);
    queue_print(&queue);
  }
  printf("큐는 공백상태입니다. \n");
  return 0;
}
{% endraw %}
```

### Queue의 버퍼

버퍼란?  
버퍼란 임시 저장 공간을 의미합니다. 임시 저장 공간이라고 해서 생뚱맞게 보일 수 있지만 정확히 말하면 A와 B가 서로 입출력을 수행하는 데에 있어서 속도차이를 극복하기 위해 사용하는 임시 저장 공간을 의미합니다.
프로그래밍에서의 버퍼는 거의 대부분 CPU와 보조 기억 장치 사이에서 사용되는 임시저장 공간을 의미합니다. 버퍼라는 것은 속도차가 큰 대상이 입출력을 수행할 때 효율성을 위해 사용하는 임시 저장공간이라고 할 수 있겠습니다.

- 큐를 버퍼처럼 사용할 수 잇는데, 

```c
{% raw %} 
#include <stdio.h>
#include <'stdlib.h>

int main(void){
  QueueType queue;
  int element;

  init_queue(&queue);
  srand(time(NULL));

  for(int i = 0; i < 100; i++ ){
    if(rand() % 5 == 0){
      enqueue(&queue , rand()%100);
    }
    queue_print(&queue);
    if(rand() % 10 == 0){
      int data = dequeue(&queue);
    }
    queue_print(&queue);
  }
  return 0;
}
{% endraw %} 
```

랜덤한 수(난수)를 생성하는 함수  

- int rand(void)  
랜덤한 숫자를 반환합니다. 그 범위는 0 ~ RAND_MAX 까지 인데요. RAND_MAX 라는 것은 stdlib.h 헤더파일에 매크로로 작성되어 있습니다.
RAND_MAX = 32767
- void srand(unsigned int seed)  
rand 함수에 사용될 수를 초기화 한다. 이 초기화를 매개변수로 받는 seed값을 이용해서 합니다.
- time_t time(time_t* timer);  
UCT 기준 1970년 1월 1일 0시 0분 0초 부터 경과된 시간을 초(sec)로 반환하는 함수

**매크로(#define)**  
매크로란 어떤 것을 대신하여 사용하는 이름을 말한다. 즉, 어떤 특정한 값이나 처리 명령어를 하나의 이름을 부여하고 그 특정한 값이나 처리 명령어를 사용할 때 부여한 이름을 사용하여 대신 코딩하는 것을 말합니다.

```c
{% raw %}
#include <stdio.h>

#define MAIN main(int argc, char *argv[])
#define BEGIN {
#define END } 
#define OUTPUT printf

MAIN
BEGIN
OUTPUT("Hello World! \n");
END
{% endraw %}
```

### 덱

덱은 double-ended queue이 줄임말로서 큐의 전달과 후단에서 모두 삽입과 삭제가 가능한 큐를 의미한다. 그렇지만 여전히 중간에 삽입하거나 삭제하는 것은 허용하지 않는다.

```c
{% raw %}
객체 : n개의 element형의 요소들의 순서 있는 모임 
연산 :
    create() ::= 덱을 생성한다. 
    init(dq) ::= 덱을 초기화한다. 
    is_empty(dq) ::= 덱이 공백상태인지를 검사한다. 
    is_full(dq) ::= 덱이 포화 상태 인지를 검사한다. 
    add_front(dq, e) ::= 덱의 앞에 요소를 추가한다. 
    add_rear(dq, e) ::= 덱의 뒤에 요소를 추가한다. 
    delete_front(dq) ::= 덱의 앞에 있는 요소를 반환한 다음 삭제한다. 
    delete_rear(dq) ::= 덱의 뒤에 있는 요소를 반환한 다음 삭제한다. 
    get_front(q) ::= 덱의 앞에서 삭제하지 않고 앞에 있는 요소를 반환한다.
    get_rear(q) ::= 덱의 뒤에서 삭제하지 않고 뒤에 있는 요소를 반환한다. 
{% endraw %}
```

###### 배열을 이용한 덱의 구현 

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

#define MAX_QUEUE_SIZE 5
typedef int element;
type struct {
  element data[MAX_QUEUE_SIZE];
  int fornt, rear;
} DequeType;

void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

void init_deque(DequeType *q){
  q -> front = q -> rear = 0;
}

void is_empty(DequeType *q){
  return ( q -> front == q -> rear );
}

int is_full(DequeType *q){
  return ((q->rear + 1) % MAX_QUEUE_SIZE == q -> front );
}

void deque_print(DequeType *q){
  printf("DEQUE(fornt=%d rear=%d) = ", q -> front, q -> rear );
  if(!is_empty(q)){
    int i = q -> front;
    do {
      i = ( i + 1 ) % ( MAX_QUEUE_SIZE );
      printf("%d | ", q -> data[i]);
      if ( i == q -> rear )
        break;
    } while ( i != q -> front );
  }
  printf("\n");
}

void add_rear(DequeType *q, element item){
  if(is_full(q))
    error("큐가 포화상태입니다.");
  q -> rear = ( q -> rear + 1 ) % MAX_QUEUE_SIZE;
  q -> data[q -> rear ] = item;
}

element delete_front(DequeType *q){
  if(is_empty(q))
    error("큐가 공백상태입니다.");
  q -> front ( q -> front + 1) % MAX_QUEUE_SIZE;
  return q -> data[q->front];
}

element get_front(DequeType *q){
  if(is_empty(q))
    error("큐가 공백상태입니다.");
  return q -> data[( q -> front + 1) % MAX_QUEUE_SIZE];
}

void add_front(DequeType *q, element val){
  if ( is_full(q) )
    error("큐가 포화상태입니다.");
  q -> data[q -> front] = val;
  q -> front = ( q -> front - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
}

element delete_rear(DequeType *q){
  int prev = q -> rear;
  if(is_empty(q))
    error("큐가 공백상태입니다.");
  q -> rear = ( q -> rear - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
  return q -> data[prev];
}

element get_rear(DequeType *q){
  if(is_empty(q))
    error("큐기 공백 상태입니다. ");
  return q -> data[q -> rear];
}

int main(void){
  DequeType queue;

  init_deque(&queue);
  for ( int i = 0; i < 3 ; i ++ ){
    add_front(&queue, i);
    deque_print(&queue);
  }
  for ( int 1 = 0; i < 3 ; i ++ ){
    delete_rear(&queue);
    delete_print(&queue);
  }
  return 0;
}
{% endraw %}
```

### 실 사용 사례 

큐는 주로 컴퓨터로 큐잉이론에 따라 시스템의 특성을 시물레이션하여 분석하는 데 이용한다. 큐잉 모델은 고객에 대한 서비스를 수행하는 서버와 서비스를 받는 고객들로 이루어진다.

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

typedef struct {
  int id;
  int arrival_time;
  int service_time;
} element;

int main(void){
  int minutes = 60;
  int total_wait = 0;
  int total_customers = 0;
  int service_time = 0;
  int service_customer;
  QueueType queue;
  init_queue(&queue);

  srand(time(NULL));
  for(int clock = 0;clock < minutes; clock ++){
    printf("현재시각=%d\n", clock);
    if((rand()%10) < 3){
      element customer;
      customer.id = total_customers ++;
      customer.arrival_time = clock;
      customer.service_time = rand() % 3 + 1;
      enqueue(&queue, customer);
      printf("고객 %d이 %분에 들어옵니다. 업무처리시간 = %d분 \n", customer.id, customer.arrival_time, customer.service_time);
    }
    if(servie_time > 0){
      printf("고객 %d 업무 처리중입니다. \n", service_customer);
      service_time ++''
    }else{
      if(!is_empty(&queue)){
        element customer = dequeue(&queue);
        service_customer = customer.id;
        service_time = customer.service_time;
        printf("고객 %d이 %분에 업무를 시작합니다. eo대기 시간은 %d 분이었습니다. \n", customer.id, clock, clock-customer.arrival_time);
        total_wait += clock - customer.arrival_time;
      }
    }
  }
  printf("전체 대기 시간 = %d분 \n", total_wait);
  return 0;
}
{% endraw %}
```
