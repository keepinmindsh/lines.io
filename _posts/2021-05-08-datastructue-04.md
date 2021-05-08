---
title:  "배열, 구조체, 포인터"
excerpt: "자료구조를 공부하기 위한 기본기 닦기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-08T14:15:00-05:00
layout: single
---

> 힘든 날도 있겠지, 좋은 날도 있을 거고, 오늘은 어떤 날인가? 

***

# 배열

```c

객체 : < 인덱스, 값 > 쌍의 집합 
연산 : 
  - create(size) ::= size개의 요소를 저장할 수 있는 배열 생성 
  - get(A, i) ::= 배열 A의 i 번째 요소 반환. 
  - set(A, i, v) ::= 배열 A의 i 번째 위치에 값 v 저장. 

```

컴파일러에서 배열 요소에 대한 메모리 주소 구현 방식
 - list[0] : 기본주소=base
 - list[1] : base + 1*sizeof(int)
 - ... 
우리가 프로그램에서 list[i]라고 적으면 컴파일러는 주소 base + i*sizeof(int)에 있는 값을 가져온다.

### 배열의 응용 : 다항식 

- 첫번째 방법 

```c

10x^5 + 0 * x^4 + 0 * x^3 + 0 * x^2 + 6 * x + 3 


#define MAX_DEGREE 101 

typedef struct {
  int degree;
  float coef[MAX_DEGREE];
} polynomial 

polynomial a = { 5, { 10, 0 , 0, 0, 6 , 3}}

```

- 두번째 방법

```c

#define MAX_TERMS 101

typedef struct {
  float coef;
  int expon;
} terms[MAX_TERMS];
int avail; 

terms[MAX_TERMS] = {{ 8,3 }, { 7,1 } , { 1, 0 } , { 10, 3 } , { 3 , 2 } , { 1 , 0 } }

```

- 희소 행렬 

```c

// 희소 행렬의 표현 방법 #1
#define MAX_ROWS 100
#define MAX_COLS 100
int matrix[MAX_ROWS][MAX_COLS];


// 희소 행렬의 표현방법 #2
struct matrix {
  int x,
  int y, 
  char value
}

int main(){
  struct matrix m[100] = { { .x = 1 , .y= 1 , .value = 'G' }, ... }
}

```

- 전치 행렬 계산법 

```c

{% raw %}

#define ROWS 3
#define COLS 3

// 행렬 전치 함수 - 첫번째 방식 
void matrix_transpose(int A[ROWS][COLS], int B[ROWS][COLS]){
  for ( int r = 0 ; r < ROWS ; r ++){
    for ( int c = 0 ; c < COLS ; c ++){
      B[c][r] = A[r][c];
    }
  }
}

// 행렬 전치 함수 - 두번째 방식 
#include <stdio.h>
#include <stdlib.h>

#define MAX_TERMS 100
typedef struct {
  int row;
  int col;
  int value;
}

typedef struct SparseMatrix {
  element data[MAX_TERMS];
  int rows;
  int cols;
  int terms;
} SparseMatrix;

SparseMatrix matrix_transpose2(SparseMatrix a){
  SparseMatrix b;

  int bindex;
  b.rows = a.rows;
  b.cols = a.cols;
  b.terms = a.terms;

  if(a.terms > 0){
    bindex = 0;
    for ( int c = 0 ; c < a.cols ; r ++){
      for ( int i = 0 ; i < a.terms ; i ++){
        if(a.data[i].col == c){
          b.data[bindex].row = a.data[i].col;
          b.data[bindex].col = a.data[i].row;
          b.data[bindex].value = a.data[i].value;
        }
      }
    }
  }
  return b;
}

{% endraw %}

```

***

# 포인터 

포인터는 다른 변수의 주소를 가지고 있는 변수이다. 모든 변수는 메모리 공간에 저장되고 메모리의 각 바이트에는 주소가 매겨져 있다. 이 주소가 포인터에 저장된다. 주소는 컴퓨터에 따라 다를 수 있으므로 포인터 변수는 정확한 숫자보다는 그냥 화살표로 그려진다.

배열과 포인터의 관계 : 배열의 이름은 배열의 시작부분을 가리키는 포인터이다.  
배열의 이름이 점선으로 그려져 있는 이유는 실제로 컴파일러가 배열의 이름에 공간을 할당하지는 않기 때문이다. 대신에 배열의 이름이 있는 곳을 배열의 첫번째 요소의 주소로 대치한다. 따라서 배열의 이름이 포인터이기 때문에 배열이 함수의 매개변수로 전달 될 때 사실을 포인터가 전달되는 것이다.  

### 동적 메모리 할당 

```c

int *p;
// malloc() 함수가 반환했던 포인터 값을 잊어버리면 안된다는 것이다. 포인터 값을 잊어버리면 동적 메모리를 반환할 수 없다. 
// malloc()은 시스템의 메모리가 부족해서 요구된 메머리를 할당할 수 없으면 NULL을 반환한다. 
p = ( int * )malloc(sizeof(int));
*p = 1000;
free(p);                          
             
```

동적 메모리가 할당되는 공간을 히프라고 한다. 히프는 운영체제가 사용되지 않는 메모리 공간을 모아 놓은 곳이다. 필요한 만큼 할당을 받고 또 필요한 대에 사용하고 반납하기 때문에 메모리를 매우 효율적으로 사용할 수 있다.


***

# 구조체 

복잡한 객체에는 다양한 타입의 데이터들이 한데 묶여져서 있다. 배열이 타입이 같은 데이터의 모임이라면 구조체는 타입이 다른 데이터를 묶는 방법이다.

```c

{% raw %}

struct 구조체이름 {
  항목1;
  항목2;
  ...
}

// 구조체 변수 선언 방식 
struct 구조체이름 구조체변수;

#include <stdio.h>

typedef struct studentTag {
  char name[10];
  int age;
  double gpa;
} student; 

int main(void) {
  student a = { "kim", 30, 4.3 };
  student b = { "park", 21, 4.2 };
  return 0;
}

{% endraw %}

```

***

# 구조체와 포인터 

우리는 구조체에 포인터를 선언하고 포인터를 통하여 구조체 멤버에 접근할 수 있다. 여기에서 하나 주의할 것은 포인터를 통하여 구조체의 멤버에 접근하는 편리한 표기법 "->"이다.

```c

// (*ps).i 보다 ps -> i 라고 쓰는 것이 더 편리하다. 

typedef struct studentTag {
  char name[10];
  int age;
  double gpa;
} student;

int main(void) {
  student s*;

  s = (student *)malloc(sizof(student));
  if ( s == NULL ){
    fprintf(stderr, "메모리가 부족해서 할당할 수 없습니다. \n");
    exit(1);
  } 

  strcpy(s -> name,  "Park");
  s -> age = 20;

  free(s);
  return 0;
}

```