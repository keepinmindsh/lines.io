---
title:  "스택"
excerpt: "스택에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-08T14:15:00-05:00
layout: single
---

> 자세히 보아야 예쁘다  
> 오래 보아야    
> 사랑스럽다.   
> 너도 그렇다.  
>   풀꽃, 시인 나태주 

***

스택에서의 입출력은 맨 위에서만 일어나고 스택의 중간에서는 데이터를 삭제할 수 없다. 스택에서 입출력이 이루어지는 부분을 스택 상단이라고 하고 반대쪽인 바닥부분을 스택 하단이라고 한다. 스택에 저장되는 것을 요소라 부른다.

###### 스택의 구조 : 후입선출 ( LIFO : Last In First Out )

 - 함수에서 실행이 끝나면 자신을 호출한 함수로 되돌아가야한다. 이때 스택이 사용되는데, 즉 복귀할 주소를 기억하는데 사용된다.

### 추상 자료형 스택 

 ```c

- 객체 : 0개 이상의 원소를 가지는 유한 선형 리스트 
- 연산 : 
  create(size)  ::= 최대크키가 size인 공백 스택을 생성한다. 
  is_full(s) ::= 
    if(스택의 원소수 == false) return TRUE;
    else return FALSE;
  is_empty(s)   ::= 
    if(스택의 원소수 == 0) return TRUE;
    else return FALSE;
  push(s, item) ::=
    if( is_full(s) ) retirm ERROR_STACKFULL;
    else 스택의 맨 위에 item을 추가한다. 
  pop(s) ::=
    if ( is_empty(s) ) return ERROR_STACKEMPTY;
    else 스택의 맨 위의 원소를 제거해서 반환한다.
  peeck(s) ::=
    if( is_empty(s) ) return ERROR_STACKEMPTY;
    else 스택의 맨 위의 원소를 제거하지 않고 반환한다.                          


 ```

 Stack은 기본적으로 Push와 Pop의 두가지 연산으로 이루어져있고, 실제 Stack에서 허용하는 Item 갯수가 가득찼는지 pop을 했을 때 더 빼낼 아이템이 없는지를 체크하기 위한 Validation이 존재한다.

### 스택의 구현  

 ```c 

is_empty(S):
  if top == -1
    then return TRUE
    then return FALSE 

is_full(S):
  if top >= ( MAX_STACK_SIZE )
    then return TRUE 
    then return FALSE 

// push에서는 먼저 top의 값을 증가하는 것에 유의하라. top이 가리키는 위치는 마지막으로 삽입되었던 요소이므로 top을 증가시키지 않고 삽입하면 마지막 요소가 지워지게 된다. 
push(S,x):
  if is_full(S)
    then error "overflow"
    else top <- top + 1
         stack[top] <- x

// pop 연산은 스택에서 하나의 요소를 제거하는 여산으로 top이 가리키는 요소를 스택에서 꺼내어 외부로 건네주는 연산이다. 
// 먼저 요소를 제거하기 전에 스택이 비어있는지 검사행한다. 
pop(S, x) :
  if is_empty(S)
    then error "underflow"
    else e <- stack[top]
        top <- top-1
        return e
        
 ```

### 전역변수를 이용한 스택의 구현 

 ```c

{% raw %} 

#include <stdio.h>
#include <stdlib.h>

// 스택이 전역 변수로 구현된다. 
#define MAX_STACK_SIZE 100
typedef int element;
element stack[MAX_STACK_SIZE];
int top = -1;

// 공백 상태 검출 함수 
int is_empty()
{
  return ( top == -1 );
}

// 포화 상태 검출 함수 
int is_full()
{
  return ( top == ( MAX_STACK_SIZE - 1));
}

// 삽입 함수 
void push(element item){
  if(is_full()){
    fprintf(stderr, "스택 포화 에러\n");
    return;
  }
  else stack[++top] = item;
}

// 삭제 함수 
element pop(){
  if(is_empty()){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return stack[top--];
}

// 피크함수 
element peek(){
  if(is_empty()){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return statck[top];
}

int main(void){
  push(1);
  push(2);
  push(3);
  printf("%d\n", pop());
  printf("%d\n", pop());
  printf("%d\n", pop());
  return 0;
}
  
{% endraw %}

 ```

### 스택의 요소를 구조체로 하기 

 ```c

{% raw %} 

#include <stdio.h>
#include <stdlib.h>

#define MAX_STACK_SIZE 100
#defind MAX_STRINC 100

typedef stuct {
  int student_no;
  char name[MAX_STRING];
  char address[MAX_STRING];
} element;

element stack[MAX_STACK_SIZE];
int top = -1;

// 공백 상태 검출 
int is_empty(){
  return ( top == -1);
}

// 포화 상태 검출 함수 
int is_full(){
  return ( top == (MAX_STACK_SIZE - 1));
}

// 삽입 함수 
void push(element itme){
  if(is_full()){
    fprintf(stderr, "스택 포화 에러\n");
    exit(1);
  }
  else stack[++top] = item;
}

// 삭제 함수 
element pop(){
  if(is_empty()){
    fprintf(stderr, "스택 공백 에러 \n");
    exit(1);
  }
  else return stack[top--];
}

// 피크 함수 
element peek(){
  if(is_empty()){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return stack[top];
}

int main(void){
  element ie = {
    20190001,
    "Hong",
    "Seoul"
  };

  element oe;

  push(ie);
  oe = pop();

  printf("학번 : %d\n", oe.student_no);
  printf("이름 : %d\n", oe.name);
  printf("주소 : %d\n", oe.address);

  return 0;
}

{% endraw %} 

 ```

### 함수의 매개변수로 전달하는 방법

```c

{% raw %}

#include <stdio.h>
#include <stdlib.h>

// 차후에 스택이 필요하면 여기만 복사하여 붙인다. 
// ===== 스택 코드의 시작 =====
#define MAX_STACK_SIZE 100

typedef int element;
typedef struct {
  element data[MAX_STACK_SIZE];
  int top;
} StackType;


// 스택 초기화 함수 
void init_stack(StackType *s){
  return ( s -> top == -1);
}

// 공백상태 검출 함수
int is_empty(StackType *s){
  return ( s -> top == -1);
}

// 포화 상태 검출 함수 
int is_full(StackType *s){
  return ( s -> top == ( MAX_STACK_SIZE - 1));
}

// 삽입함수 
void push(StackType *s, element item){
  if(is_full(s)){
    fprinf(stderr, "스택 포화 에러\n");
    return;
  }
  else{
    s -> data[++(s-> top)] = item;
  }
}

// 삭제함수 
element pop(StackType *s){
  if(is_empty(s)){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return s->data[(s->top)X-UA-Compatible]
}

// 피크함수 
element peek(StackType *s){
  if(is_empty(s)){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return s ->data[s->top];
}
// ===== 스택 코드의 끝 =====

// C언어에서의 함수 매개변수 전달 방식이 기본적으로 Call By Value이기 때문에, 구조체를 함수의 매개변수로 전달하는 경우, 구조체의 원본이 전달되는 것이 아니라 구조체의 복사본이 전달된다. 
// 따라서 함수 안에서는 복사본을 수정하여도 원본에는 영향을 주지 못한다. 그러나 원본에 대한 포인터를 전달하면 원본을 변경할 수 있다. 

int main(void){
  StackType s;

  init_stack(&s);
  push(&s, 1);
  push(&s, 2);
  push(&s, 3);
  printf("%d\n", pop(&s));
  printf("%d\n", pop(&s));
  printf("%d\n", pop(&s));
}

{% endraw %}

```

### 스택을 동적할 메모리 할당으로 생성

```c

int main(void){
  StackType *s;
  s = (StackType *)malloc(sizeof(StackType));
  init_stack(s);
  push(s, 1);
  push(s, 2);
  push(s, 3);
  printf("%d\n", pop(s));
  printf("%d\n", pop(s));
  printf("%d\n", pop(s));
  free(s);
}

```

### 동적배열스택

```c

typedef int element;
typedef struct {
  element *data;
  int capacity;
  int top;
} StackType;

// 스택 생성 함수 
void int_stack(StackType *s){
  s -> top = -1;
  s -> capacity = 1;
  s -> data = (element *)malloc(s -> capacity*sizeof(element));
}

// 스택 삭제 함수 
void delete(StackType *s){
  free(s);
}

void push(StackType *s, element item){
  if(is_full(s)){
    s -> capacity *= 2;
    s -> data = (element *)realloc(s->data, s->capacity * sizeof(element));
  }
  s -> data[++(s->top)] = item;
}

```

### 동적 배열 스택 프로그램

```c

{% raw %}

#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100

typedef int element;
typedef struct {
  element *data;
  int capacity;
  int top;
} StackType;

// 스택 생성 함수 
void init_stack(StackType *s){
  s -> top = -1;
  s -> capacity = 1;
  s -> data = (element *)malloc(s -> capacity * sizeof(element));
}

// 공백 상태 검출 함수 
int is_empty(StackType *s){
  return ( s -> top == -1 );
}

// 포화상태 검출 함수 
int is_full(StackType *s)
{
  return ( s -> top == (MAX_STACK_SIZE -1 ));
}

void push(StackType *s, element item){
  if(is_full(s)){
    s -> capacity *= 2;
    s -> data = ( element *)realloc(s -> data, s -> capacity * sizeof(element));
  }
  s -> data[++(s->top)] = item;
}

// 삭제 함수 
element pop(StackType *s){
  if(is_empty(s)){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return s -> data[(s->top)--];
}

int main(void){
  StackType s;
  init_stack(&s);
  push(&s, 1);
  push(&s, 2);
  push(&s, 3);
  printf("%d \n", pop(&s));
  printf("%d \n", pop(&s));
  printf("%d \n", pop(&s));
  free(s.data);
  return 0;
}

{% endraw %}

```

### 스택의 활용 : 괄호 검사 문제

- 왼쪽 괄호의 개수와 오른쪽 괄호의 개수가 같아야 한다.
- 같은 종류의 괄호에서 왼쪽 괄호는 오른쪽 괄호보다 먼저 나와야 한다.
- 서로 다른 종류의 왼쪽 괄호와 오른쪽 괄호 쌍은 서로를 교차하면 안된다.

```c

check_matching(expr);

while(입력 expr의 끝이 아니면)
ch <- expr의 다음 글자 
switch(ch)
    case '(' : case '[' : case '{' :
        ch를 스택에 삽입 
        break
    case ')' : case ']' : case '}' :
        if ( 스택이 비어 있으면 )
            then 오류 
            else 스택에서 open_ch 를 꺼낸다
                if ( ch와 open_ch가 같은 짝이 아니면 )
                    then 오류 보고 
    break
if( 스택이 비어 있지 않으면 )
    then 오류

```

```c

{% raw %}

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_STACK_SIZE 100

typedef char element;
element stack[MAX_STACK_SIZE];

int check_matching(const char *in){
  StackType s;
  char ch, open_ch;
  int i, n = strlen(in);
  init_stack(&s);

  for ( i = 0; i < n ; i ++){
    ch = in[i];
    switch(ch){
      case '(' : case '[' : case '{' :
        push(&s, ch);
        break;
      case ')' : case ']' : case '}' :
        if(is_empty(&s)) return 0;
        else {
          open_ch = pop(&s);
          if((open_ch == '(' && ch != ')' ) ||
            (open_ch == '[' && ch != ']' ) ||
            (open_ch == '{' && ch != '}' ) 
          ){
            return 0;
          }
          break;
        }  
    }
  }
  if(!is_empty(&s)) return 0;
  return 1;
}

int main(void){
  char *p = "{ A[(i+1)]=0; }";
  if(check_matching(p) == 1)
    printf("%s 괄호검사성공\n", p);
  else 
    printf("%s 괄호검사실패\n", p);
  return 0;
}

{% endraw %}

```

### 스택의 응용 : 후위 표기 수식의 계산

```c

calc_posfix:
    스택 s를 생성하고 초기화한다. 
    for item in 후위표기식 do 
        if ( item이 피연산자이면 )
            push(s, item)
        else if ( item이 연산자 op이면 )
            second <- pop(s)
            first <- pop(s)
            result <- first op second // op 는 +-*/ 중의 하나 
            push(s, result)
        final_result <- pop(s);                                  
                        
```

```c
{% raw %}

#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100

typedef char element;

int eval(char exp[]){
  int op1, op2, value , i = 0;
  int len = strlen(exp);
  char ch;
  StackType s;

  init_stack(&s);
  for( i = 0; i < len ; i ++){
    ch = exp[i];
    if( ch != '+' && ch != '-' && ch != '*' && ch != '/' ){
      value = ch - '0';
      push(&s, value);
    }else {
      op2 = pop(&s);
      op1 = pop(&s);
      switch ( ch ) {
        case '+' : push(&s, op1 + op2); break;
        case '-' : push(&s, op1 - op2); break;
        case '*' : push(&s, op1 * op2); break;
        case '/' : push(&s, op1 / op2); break;
      }
    }
  }
  return pop(&s);
}

int main(void){
  int result;
  printf("후위표기식은 82/3-32*+\n");
  result = eval("82/3 - 32 *+");
  printf("결과값은 %d\n", )
}

{% endraw %}
     
```

### 중위 표기 수식을 후위 표기 수식으로 전환

```c

infix_to_postfix(exp);

스택 s를 생성하고 초기화 
while ( exp에 처리할 문자가 남아 있으면 )
    ch <- 다음에 처리할 문자 
    switch ( ch )
    case 연산자 : 
        while ( peek(s)의 우선순위 >= ch의 우선순위 ) do 
            e <- pop(s)
            e를 출력 
        push(s, ch);
        break;
    case 왼쪽 괄호 : 
        push(s, ch);
        break;
    case 오른쪽 괄호 : 
        e <- pop(s);
        while( e != 왼쪽괄호 ) do 
            e를 출력 
            e <- pop(s);
        break;
    case 피연산자 : 
        ch를 출력
        break;

while( not is_empty(s) ) do 
    e <- pop(s)
    e를 출력 

```

```c

{% raw %}

#include <stdio.h>
#include <stdlib.h>
#define MAX_STACK_SIZE 100

typedef char element;

int prec(char op)
{
  switch(op) {
    case '(' : case ')' : return 0;
    case '+' : case '-' : return 1;
    case '*' : case '/' : return 2;
  }
  return -1;
}

void infix_to_postfix(char exp[]){
  int i = 0;
  char ch, top_op;
  int len = strlen(exp);
  StackType s;

  init_stack(&s);
  for ( i = 0; i < len; i ++){
    ch = exp[i];
    switch(ch) {
      case '+' : case '-' : case '*' : case '/' : 
        while ( !is_empty(&s) && (prec(ch) <= prec(&s)))
          prinf("%sc", pop(&s));
        push(&s, ch);
        break;
      case '(' :
        push(&s, ch);
        break;
      case ')' :
        top_op = pop(&s);
        while ( top_op != '('){
          printf("%c", top_op);
          top_op = pop(&s);
        }
        break;
      default:
        printf("%c", pop(&s));
        break;
    }
  }
  while( !is_empty(&s))
    printf("%c", pop(&s));
}

init main(void){
  char *s = "(2+3)*4+9";
  printf("중위 표시 수식  %s \n", s);
  printf("후위 표시 수식 ");
  infix_to_postfix(s);
  printf("\n");
}

{% endraw %}

```

### 미로 탐색 프로그램

```c

maze_search();

스택 s과 출구의 위치 x, 현재 생쥐의 위치를 초기화 
while( 현재의 위치가 출구가 아니면 ) do 
    현재 위치를 방문한 것으로 표기 
    if( 현재위치의 위, 아래, 왼쪽, 오른쪽 위치가 아직 방문되지 않았고 갈 수 있으면 )
        then 그 위치들을 스택에 push 
    if( is_empty(s) )
        then 실패 
    else 스택에서 하나의 위치를 꺼내어 현재 위치로 만든다. 
성공;

```

```c

{% raw %}

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_SIZE 6

typedef struct {
  short r;
  short c;
} element;

element here = { 1, 0}, entry = { 1, 0 };

void push_loc(StackType *s, int r, int c){
  if( r < 0 || c < 0) return;
  if( maze[r][c] != '1' && maze[r][c] != '.' ) {
    element tmp;
    tmp.r = r;
    tmp.c = c;
    push(s, tmp);
  }
}

void maze_print(char maz[MAZE_SIZE][MAZE_SIZE]){
  print("\n");
  for( int r = 0; r < MAZE_SIZE; r++){
    for( int c = 0; c < MAZE_SIZE; c++){
      print("%c", maze[r][c]);
    }
    printf("\n");
  }
}

int main(void){
  int r,c;
  StackType s;

  init_stack(&s);
  here = entry;
  while ( maze[here.r][here.c] != 'x'){
    r = here.r;
    c = here.c;
    maze[r][c] = '.';
    maze_print(maze);
    push_loc(&s. r - 1, c);
    push_loc(&s. r + 1, c);
    push_loc(&s. r , c - 1);
    push_loc(&s. r , c + 1);
    if(is_empty(&s)){
      printf("실패\n");
      return;
    }
    else 
      here = pop(&s);
  }
  printf("성공\n");
  return 0;
}

{% endraw %}

```