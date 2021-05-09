---
title:  "연결리스트"
excerpt: "연결리스트에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-09T14:32:00-05:00
layout: single
---

> 삶을 행복하게 산다는 것은    
> 내가 가진 것들의 사소한 충만함에도 만족할 줄 아는 것.

***

**리스트와 소개**  
리스트에는 항목들이 차례대로 저장되어 있다. 리스트의 항목들은 순서 또는 위치를 가진다. 앞에서 살펴본 스택과 큐도 넓게 보면 리스트의 일종이다. 리스트는 기호로 같이 다음과 같이 표현한다. 리스트는 집합하고는 다르다. 집합은 각 항목 간에 순서의 개념이 없다.  

###### L = ( item, tiem, time , ... , item )

```c

객체 : n개의 element형으로 구성된 순서 있는 모임
연산 : 
    insert(list, pos, timer) ::= pos 위치에 요소를 추가한다. 
    insert_last(list, item) ::= 맨 끝에 요소를 추가한다. 
    insert_first(list, item) ::= 맨 처음에 요소를 추가한다. 
    delete(list, pos) ::= pos 위치의 요소를 제거한다. 
    clear(list) ::= 리스트의 모든 요소를 제거한다. 
    get_entry(list, pos) ::= pos 위치의 요소를 반환한다. 
    get_length(list) ::= 리스트의 길이를 구한다. 
    is_empty(list) ::= 리스트가 비었는지를 검사한다. 
    is_full(list) ::= 리스트가 꽉 차있는지를 검사한다. 
    print_last(list) ::= 리스트의 모든 요소를 표시한다.   

```

배열을 사용하여 리스트를 구현하면 장점과 단점이 존재한다. 장점은 구현이 간단하고 속도가 빠르다는 것이다. 단점으로는 리스트의 크기가 고정된다는 것을 들 수 있다.

### 배열로 구현된 리스트

```c 
{% raw %}
#define MAX_LIST_SIZE 100

typedef int element;
typedef struct {
  element array[MAX_LIST_SIZE];
  int size;
} ArrayListType;

void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

void init(ArrayListType *L){
  return L->size == 0;
}

int is_empty(ArrayListType *L){
  return L -> size == 0;
}

int is_full(ArrayListType *L){
  return L -> size == MAX_LIST_SIZE;
}

element get_entry(ArrayListType *L, int pos){
  if( pos < 0  || pos >= L->size )
    error("위치 오류");
  return L -> array[pos];
}

void print_list(ArrayListType *L){
  int i;
  for ( i = 0; i < L -> size; i ++){
    printf("%d ->" , L -> array[i]);
  }
  printf("\n");
}

void insert_last(ArrayListType *L, element item){
  if( L -> size >= MAX_LIST_SIZE ){
    error("리스트 오버플로우");
  }
  L -> array[L->size++] = item;
}

void insert(ArrayListType *L, int pos, element item){
  if(!is_full(L) && ( pos >= 0 ) && ( pos <= L -> size )){
    for( int i = ( L -> size - 1 ); i >= pos; i --)
      L -> array[i+1] = L -> array[i];
    L -> array[pos] = item;
    L -> size ++;
  }
}

element delete(ArrayListType *L, int pos){
  element item;

  if( pos < 0 || pos => L -> size )
    error("위치오류")
  item = L -> array[pos];
  for ( int i = pos; i < ( L -> size - 1); i ++)
    L -> array[i] = L -> array[i+1];
  L -> size --;
  return item;
}

int main(void){
  ArrayListType list;

  init(&list);
  insert(&list, 0, 10); print_list(&list);
  insert(&list, 0, 20); print_list(&list);
  insert(&list, 0, 30); print_list(&list);
  insert_last(&list, 40); print_list(&list);
  delete(&list, 0); print_list(&list);
  return 0;
}

{% endraw %}
```

실행 시간 분석  

인덱스를 사용하여 항목에 바로 접근할 수 있으므로 명백히 O(1)이다. 삽입이나 삭제 연산은 다른 항목들을 이동하는 경우가 많으로 최악의 경우 O(n)이 된다. 하지만 리스트의 맨 끝에 삽입하는 경우는 O(1)이다.

### 연결리스트

물리적으로 흩어져 있는 자료들을 서로 연결하여 하나로 묶는 방법을 연결 리스트라고 한다. 상자를 연결하는 줄은 포인터로 구현한다. 연결 리스트에서는 앞뒤에 있는 데이터들을 이동할 필요가 없이 줄만 변경시켜주면 된다.
연결리스트는 노드들의 집합이다. 노드들은 메모리의 어떤 위치에나 있을 수 있으며 다른 노드로 가기위해서는 현재 노드가 가지고 있는 포인터를 이용하면 된다. 노드는 데이터 필드와 링크 필드로 구성되어 있다.

- 연결리스트의 종류
  - 단순 연결 리스트
  - 원형 연결 리스트
  - 이중 연결 리스트

### 단순 연결리스트 

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
                          
typedef int element;
typedef struct ListNode {
  element data;
  struct ListNode *link;
} ListNode;

// 오류 처리 함수 
void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

ListNode* insert_first(ListNode *head, int value){
  ListNode *p = ( ListNode * )malloc(sizeof(ListNode));
  p -> data = value;
  p -> link = head; // 헤드 포인터의 값을 복사 
  head = p;         // 헤드 포인터 변경 
  return head;      // 벼녕된 헤드 포인터 변환 
}

// 노드 pre 뒤에 새로운 노드 삽입 
ListNode* insert(ListNode *head, ListNode *pre, element value){
  ListNode *p = (ListNode *)malloc(sizeof(ListNode));
  p -> data = value;
  p -> link = pre -> link;
  pre -> link = p;
  return head;
}

ListNode* delete_first(ListNode *head){
  ListNode *remove;
  if ( head == NULL ) return NULL;
  removed = head;
  head = removed -> link;
  free(removed);
  return head;
}

// pre가 가리키는 노드의 다음 노드를 삭제한다. 
ListNode* delete(ListNode *head, ListNode *pre){
  ListNode *removed;
  removed = pre -> link;
  pre -> link = removed -> link;
  free(removed);
  return head;
}

void print_list(ListNode *head){
  for( ListNode * = head; p != NULL; p = p -> link ){
    printf("%d->"), p -> data);
  }
  printf("NULL \n");
}

int main(void){
  ListNode *head = NULL;

  for( int i = 0; i < 5 ; i ++){
    head = insert_first(head, i);
    print_list(head);
  }

  for(int i = 0; i < 5 ; i ++ ){
    head = delete_first(head);
    print_list(head);
  }

  return 0;
}
{% endraw %}
```

### 단어들을 저장하는 연결리스트 

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
$include <string.h>

typedef struct {
  char name[100];
} element;

typedef struct ListNode {
    element data;
    struct ListNode *link;
} ListNode;

// 오류 처리 함수 
void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

ListNode* insert_first(ListNode *head, element value){
  ListNode * = (ListNode *)malloc(sizeof(ListNode));
  p -> data = value;
  p -> link = head;
  head = p;
  return head;
}

void print_list(ListNode *head){
  for ( ListNode *p = head; p != NULL; p = p -> link )
    printf("%s ->", p -> data.name);
  printf("NULL \n");
}

int main(void){
  ListNode *head = NULL;
  element data;

  strcpy(data.name, "APPLE");
  head = insert_first(head, data);
  print_list(head);

  strcpy(data.name, "KIWI");
  head = insert_first(head, data);
  print_list(head);

  strcpy(data.name, "BANANA");
  head = insert_first(head, data);
  print_list(head);

  return 0;
}
{% endraw %}
```

### 특정 한 값을 탐색하는 함수

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

typedef int element;

typedef struct ListNode{
    element data;
    struct ListNode *link;
} ListNode;

ListNode* insert_first(ListNode *head, element value){
  ListNode *p = (ListNode *)malloc(sizeof(ListNode));
  p -> data = value;
  p -> link = head;
  head = p;
  return head;
}

void print_line(ListNode *head){
  for ( ListNode *p = head; p != NULL; p = p -> link)
    printf("%d -> ", p -> data);
  printf("NULL \n");
}

ListNode* search_list(ListNode *head, element x){
  ListNode *p = head;

  while( p != NULL){
    if( p -> data == x) return p;
    p = p -> link;
  }
  return NULL;
}

int main(void){
  ListNode *head = NULL;

  head = insert_first(head, 10);
  print_last(head);
  head = insert_first(head, 20);
  print_last(head);
  head = insert_first(head, 30);
  print_last(head);
  if( search_list(head, 30) != NULL){
    printf("리스트에서 30을 찾았습니다. \n");
  }else {
    printf("리스트에서 30을 찾지 못했습니다. \n");
  }
  return 0;
}
{% endraw %}
```

### 두 개의 리스트를 하나로 합치는 것

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct LietNode {
  element data;
  struct ListNode *link;
} ListNode;

ListNode* insert_first(ListNode *head, element value){
  ListNode *p = (ListNode *)malloc(sizeof(ListNode));
  p -> data = value;
  p -> link = head;
  head = p;
  return head;
}

void print_list(ListNode *head){
  for ( ListNode *p = head; p != NULL; p = p -> link)
    printf("%d -> ", p -> data);
  printf("NULL \n");
}

ListNode* concat_list(LietNode *head1, ListNode *head2){
  if(head1 == NULL) return head2;
  else if(head2 == NULL) return head1;
  else {
    ListNode *p;
    p = head1;
    while(p -> link != NULL)
      p = p -> link;
    p -> link = head2;
    return head1;
  }

  int main(void){
    ListNode* head1 = NULL;
    ListNode* head2 = NULL;

    head1 = insert_first( head1, 10);
    head1 = insert_first( head1, 20);
    head1 = insert_first( head1, 30);
    print_list(head1);

    head2 = insert_first( head2, 10);
    head2 = insert_first( head2, 10);
    print_list(head2);

    ListNode *total = concat_list(head1, head2);
    print_list(total);
    return 0;
  }
}

{% endraw %}
```

### 리스트를 역순으로 만드는 연산

```c
{% raw %}
#include <stdio.h> 
#include <stdlib.h>

typedef int element;

typedef struct ListNode {
  element data;
  struct ListNode *link;
} ListNode;

ListNode* insert_first(ListNode *head, element value){
  ListNode *p = (LietNode *)malloc(sizeof(ListNode));
  p -> data = value;
  p -> link = head;
  head = p;
  return head;
}

void print_list(LietNode *head){
  for ( ListNode *p = head; p != NULL; p = p -> link)
    printf("%d->",  p -> data);
  printf("NULL \n");
}

ListNode* reverse(ListNode *head){
  ListNode *p, *q, *r;

  p = head;
  q = NULL;
  while( p != NULL ){
    r = q;

    q = p;

    p = p -> link;
    q -> link;
  }
  return d;
}

int main(void){
  ListNode* head1 = NULL;
  ListNode* head2 = NULL;

  head1 = insert_first(head1, 10);
  head1 = insert_first(head1, 20);
  head1 = insert_first(head1, 30);
  print_list(head1);

  head2 = reverse(head1);
  print_list(head2);
  return 0;
}
{% endraw %}
```

### 다향식 덧셈 프로그램

```c 
{% raw %}
#include <stdio.h>
#include <stdlib.h>

typedef struct ListNode {
  int coef;
  int expon;
  struct ListNode *link;
} ListNode;

typedef struct ListType {
  int size;
  ListNode *head;
  ListNode *tail;
} ListType;

void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

ListType* create(){
  ListType *plist = (ListType *)malloc(sizeof(ListType));
  plist -> size = 0;
  plist -> head = plist -> tail  = NULL;
  return plist;
}

void insert_last(ListType* plist, int coef, int expon){
  ListNode* temp = (ListNode *)malloc(sizeof(ListNode));
  if ( temp == NULL ) error("메모리 할당 에러");
  temp -> coef = coef;
  temp -> expon = expon;
  temp -> link = NULL;
  if(plist -> tail == NULL) {
    plist -> head = plist -> tail = temp;
  }else{
    plist -> tail -> link = temp;
    plist -> tail = temp;
  }
  plist -> size ++;
}

void poly_add(ListType* plist1, ListType* plist2, ListType* plist3){
  ListNode* a = plist1 -> head;
  ListNode* b = plist2 -> head;

  int sum;

  while( a && b ){
    if ( a -> expon == b -> expon ){
      sum = a -> coef + b -> coef;
      if( sum != 0) insert_last(plist3, sum, a -> expon);
      a = a -> link; b = b -> link;
    }else if( a -> expon -> b -> expon) {
      insert_last(plist3, a -> coef, a -> expon );
      a = a -> link;
    }else {
      insert_last(plist3, b -> coef, b -> expon );
      b = b -> link;
    }
  }

  for( ; a != NULL ; a = a -> link)
    insert_last(plist3 , a -> coef, a -> expon );
  for( ; b != NULL ; b = b -> link)
    insert_last(plist3 , b -> coef, b -> expon );

}

void poly_print(ListType* plist){
  ListNode* p = plist -> head;

  printf("polynomial = ");
  for( ; p != NULL ; p = p -> link)
    printf("%d^%d + " , p -> coef, p -> expon );
  pintf("\n");
}

int main(void){
  ListType *list1, *list2, *list3;

  list1 = create();
  list2 = create();
  list3 = create();

  insert_last(list1, 3, 12);
  insert_last(list1, 2, 8);
  insert_last(list1, 1, 0);

  insert_last(list2, 8, 12);
  insert_last(list2, -3, 10);
  insert_last(list2, 10, 6);

  poly_print(list1);
  poly_print(list2);

  poly_add(list1, list2, list3);

  poly_print(list3);

  free(list1);
  free(list2);
  free(list3);
}
{% endraw %}
```

### 원형 연결 리스트

원형 연결 리스트란 마지막 노드가 마지막 노드가 벛 번재 노드를 가리키는 리스트이다. 즉 마지막 노드의 링크 필드가 NULL이 아니라 첫번째 노드 주소가 되는 리스트이다.
원경 연결 리스트에서는 하나의 노드에서 다른 모든 노드로의 접근이 가능하다. 하나의 노드에서 링크를 계속 따라가면 결국 모든 모드를 거쳐서 자기 자신으로 되돌아 올 수 있는 것이다.
원형 연결 리스트가 특히 유용한 경우는 리스트의 끝에 노드를 삽입하는 연산이 단순 연결 리스트 보다 효율적일 수 있다는 것이다. 단순 연결 리스트에서 리스트의 끝에 노드를 추가하려면 첫 번째 노드에서부터 링크를 따라서 노드의 개수만큼 진행하여 마지만 노드까지 가야한다. 그러나 만약 원형 연결 리스트에서 다음과 같이 헤드 포인터가 마지만 노드를 가라키도록 구성한다면 상수 시간 안에 리스트의 처음과 끝에 노드를 삽입할 수 있다.

```c
{% raw %}
  
#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct ListNode {
  element data;
  struct ListNode *link;
} ListNode;

void print_list(ListNode* node){
  ListNode* p;

  if ( head == NULL) return;
  p = head -> link;
  do {
    printf("%d->",  p -> data);
    p = p -> link;
  } while ( p != head );
  printf("%d->", p -> data);
}

ListNode* insert_first(ListNode* head, element data){
  ListNode *node = (ListNode *)malloc(sizeof(ListNode));
  node -> data = data;
  if( head == NULL ){
    head = node;
    node -> link = head;
  }else {
    node -> link = head -> link;
    head -> link = node;
  }
  return head; // 변형된 헤드 포인터를 반환한다. 
}

ListNode* insert_last(ListNode* head, element data){
  ListNode *node = (ListNode *)malloc(sizeof(ListNode));
  node -> data = data;
  if( head == NULL ){
    head = node;
    node -> link = node;
  }else {
    node -> link = head -> link;
    head -> link = node;
    head = node;
  }
  return head; // 변경된 헤드 포인터를 반환한다. 
}

int main(void){
  ListNode *head = NULL;

  head = insert_last(head, 20);
  head = insert_last(head, 30);
  head = insert_last(head, 40);
  head = insert_first(head, 10);
  print_last(head);
  return 0;
}
{% endraw %}
```

### 용례 

컴퓨터에서 여러 응용 프로그램을 하나의 CPU를 이용하여 실행할 때에 필요하다. 현재 실행중인 모든 응용 프로그램은 원형 연결 리스트에 보관되며 운영 체제는 원형 연결 리스트에 있는 프로그램의 실행을 위해 고정된 시간 슬롯을 제공한다.
멀티플레이어 게임이다. 모든 플에이어는 원형 연결 리스트에 저장되며 한 플레이어의 기회가 끝나면 포인터 앞으로 움직여서 다음 플레이어의 순서가 된다.
원형 연결 리스트는 원형 큐를 만드는데도 사용할 수 있다.

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char element[100];
typedef struct ListNode {
  element data;
  struct ListNode *link;
} ListNode;

ListNode* insert_first(ListNode* head, element data){
  ListNode *node = (ListNode *)malloc(sizeof(ListNode));
  strcpy(node -> data, data);
  if ( head == NULL ) {
    head = node;
    node -> link = head;
  }else {
    node -> link = head -> link;
    head -> link = node;
  }
  return head;
}

int main(void){
  ListNode *head = NULL;

  head = insert_first(head, "KIM");
  head = insert_first(head, "PARK");
  head = insert_first(head, "CHOI");

  ListNode* p = head;

  for(int i = 0; i < 10; i ++){
    printf("현재 차례=%s \n", p -> data);
    p = p -> link;
  }
  return 0;0
}
{% endraw %}
```

### 이중 연결 리스트 

단순 연결 리스트에서 어떤 노드에서 후속 노드를 찾기는 쉽지만, 선행 노드를 찾으려면 구조상 아주 어렵다. 원형 연결 리스트라고 하더라도 거의 전체 노드를 거쳐서 돌아 와야 한다. 따라서 응용 프로그램에서 특정 노드에서 양방향으로 자유롭게 움직일 필요가 있다면 단순 연결 리스트 구조는 부적합하다. 이중 연결 리스트는 이러한 문제점을 해결하기 위하여 만들어진 자료구조이다.
 
```c
{% raw %}

#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct DListNode {
  element data;
  struct DListNode* llink;
  struct DListNode* rlink;
} DListNode;

void init(DListNode* phead){
  phead -> llink = phead;
  phead -> rlink = phead;
}

void print_dlist(DListNode* phead){
  DListNode* p;
  for( p = phead -> rlink; p != phead; p = p -> rlink){
    printf("<-| |%d| |->", p -> data);
    printf("\n");
  }
}

void dinsert(DListNode *before, element data){
  DListNode *newnode = (DListNode *)malloc(sizeof(DListNode));
  strcpy(newnode -> data, data);
  newnode -> llink = before;
  newnode -> rlink = before -> rlink;
  before -> rlink -> llink = newnode;
  before -> rlink = newnode;
}

void ddelete(DListNode* head, DListNode* removed){
  if( removed == head ) return;
  removed -> llink -> rlink = removed -> rlink;
  removed -> rlink -> llink = removed -> llink;
  free(removed);
}

int main(void){
  DListNode* head = (DListNode *)malloc(sizeof(DListNode));
  init(head);
  printf("추가 단계 \n");
  for( int 1 = 0; i < 5 ; i ++ ) {
    dinsert(head, i);
    print_dlist(head);
  }
  printf("\n삭제단계\n");
  for( int i = 0; i < 5 ; i ++){
    print_list(head);
    ddelete(head, head -> rlink);
  }
  free(head);
  return 0;
}

{% endraw %}
```
### MP3

```c
{% raw %}
#include <stdio.h> 
#include <stdlib.h> 
#include <string.h> 

typedef char element[100];
typedef struct DListNode {
  element data;
  struct DListNode* link;
  struct DListNode* rlink;
} DListNode;

DListNode* current;

void init(DListNode* phead){
  phead -> llink = phead;
  phead -> rlink = phead; 
}

void print_dlist(DListNode* phead){
  DListNode* p;
  for ( p = phead -> rlink ; p != phead; p = p -> rlink ){
    if( p == current )
      printf("<- | #%s# | ->", p -> data);
    else 
      printf("<- | #%s# | ->". p -> data);
  }
  printf("\n");
}

void dinsert(DListNode *bedore, element data){
  DListNode *newnode = (DListNode *)malloc(sizeof(DListNode));
  strcpy(newnode -> data, data);
  newnode -> llink = before;
  newnode -> rlink = before -> rlink;
  before -> rlink = llink = newnode;
  before -> rlink = newnode;
}

void delete(DListNode* head, DListNode* removed){
  if ( removed == head) return;
  removed -> llink -> rlink = removed -> rlink;
  removed -> rlink -> llink = removed -> llink;
  free(removed);
}

int main(void){
  char ch;
  DListNode* head = (DListNode *)malloc(sizeof(DListNode));
  init(head);

  dinsert(head, "Mamamia");
  dinsert(head, "Dancing Queen");
  dinsert(head, "Fernade");

  current = head -> rlink;
  print_dlist(head);

  do {
    printf("\n명령어를 입력하시오(<,>, q");
    ch = getchar();
    if(ch == '<') {
      current = current -> llink;
      if ( current == head )
        current = current -> llink;
    }else if(ch == '>'){
      current = current -> rlink;
      if ( current ==  head )
        current = current -> rlink; 
    }
    print_dlist(head);
    getchar();
  } while( ch != 'q');
}
   
{% endraw %}
```

### 연결 리스트로 구현한 스택

```c
{% raw %}
typedef int element;
typedef struct StackNode {
  element data;
  struct StackNode *link;
} StackNode;

typedef struct {
  StackNode *top;
} LinkedStackType;

void init(LinkedStackType *s){
  return ( s -> top == NULL ); 
}

int is_full(LinkedStackType *s){
  return 0;
}

void push(LinkedStackType *s, element item){
   StackNode *temp = (StackNode *)malloc(sizeof(StackNode));
   temp -> data = item;
   temp -> link = s -> top;
   s -> top = temp;
}

void print_stack(LinkedStackType *s){
  for ( StackNode *p =  s -> top ; p != NULL; p = p -> link )
    printf("%d -> ", p -> data);
  printf("NULL \n");
}

void pop(LinkedStackType *s){
  if(is_empty(s)){
    fprintf(stderr, "스택이 비어있음\n");
    exit(1);
  }else {
    StackNode *temp = s -> top;
    int data = temp -> data;
    s -> top = s -> top -> link;
    free(temp);
    return data;
  }
}

element peek(LinkedStackType *s){
  if(is_empty(s)){
    fprintf(stderr, "스택이 비어있음\n");
    exit(1);
  }else{
    return s -> top -> data;
  }
}

int main(void) {
  LinkedStackType s;
  init(&s);

  push(&s, 1); print_stack(&s);
  push(&s, 2); print_stack(&s);
  push(&s, 3); print_stack(&s);

  pop(&s); print_stack(&s);
  pop(&s); print_stack(&s);
  pop(&s); print_stack(&s);

  return 0;
}
{% endraw %}
```

### 연결 리스트로 구현한 큐

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct QueueNode {
    element data;
    stuct QueueNode *link;
} QueueNode;

typedef stuct {
  QueueNode *front, *rear;
} LinkedQueueType;

void init(LinkedQueueType *q){
  q -> front = q -> rear = 0;
}

int is_full(LinkedQueueType *q){
  return 0;
}

void enqueue(LinkedQueueType *q, element data){
  QueueNode  *temp = (QueueNode *)malloc(sizeof(QueueNode));
  temp -> data = data;
  temp -> link = NULL;
  if(is_empty(q)){
    fprintf(stderr, "스택이 비어 있음\n");
    exit(1);
  }else {
    data = temp -> data;
    q -> front = q -> front -> link;
    if ( q -> front == NULL )
      q -> rear = NULL;
    free(temp);
    return data;
  }
}


void print_queue(LinkedQueueType *q){
  QueueNode *p;
  for ( p = q -> front ; p != NULL; p = p -> link)
    printf("%d ->", p -> data);
  printf("NULL \n");
}

int main(void){
  LinkedQueueType queue;

  init(&queue);

  enqueue(&queue, 1); print_queue(&queue);
  enqueue(&queue, 2); print_queue(&queue);
  enqueue(&queue, 3); print_queue(&queue);
  dequeue(&queue); print_queue(&queue);
  dequeue(&queue); print_queue(&queue);
  dequeue(&queue); print_queue(&queue);
  
  return 0;
}
   
{% endraw %}
```