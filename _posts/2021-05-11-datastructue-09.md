---
title:  "우선순위 큐"
excerpt: "우선순위 큐에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-11T22:50:00-05:00
---

> 인생은 늘 불공평하다. 누구에게나 힘들고, 아프다.  
> 그래도 우리가 살아가는 이유는  
> 그 속에서도 소중한 순간들이 있기 때문에 

***

우선 순위 큐는 우선 순위의 개념을 큐에 도입한 자료구조 이다. 보통의 큐는 선입 선출의 원칙에 의하여 먼저 들어온 데이터가 먼저 나가게 된다. 그러나 우선 순위 큐에선는 데이터들이 우선 순위를 가지고 있고 우선 순위가 높은 데이터가 먼저 나가게 된다.

```c
객체 : n개의 element 형의 우선 순위를 가진 요소들의 모임 
연산 :
    create() ::= 우선 순위 큐를 생성한다. 
    init(q) ::= 우선 순위 큐 q를 초기화한다. 
    is_empty(q) ::= 우선 순위 큐 q가 비어있는지를 검사한다. 
    is_full(q) ::= 우선 순위 큐 q가 가득 찼는가를 검사한다. 
    insert(q, x) ::= 우선 순위 큐 q에 요소 x를 추가한다. 
    delete(q) ::= 우선 순위 큐로부터 가장 우선순위가 높으면 높은 요소를 삭제하고 이 요소를 반환한다. 
    find(q) ::= 우선 순위가 가장 높은 요소를 반환한다. 
```

### 우선 순위 큐의 구현 방법 

- 배열 활용
정렬이 되었냐 안되었느냐에 따라서 시간 복잡도가 차이가 발생한다. 삽입의 경우에는 제일 마지막 위치에 넣으면 되기 때문에 문제가 되지 않지만 삭제를 해야하는 경우에는 시간복잡도가 O(n)으로 증가한다. 정렬이 되어 있는 배열의 경우 순차 탐색이나 이진 탐색 과 같은 방법을 이용할 수 있다.
- 연결 리스트 활용
삽입시 배열과 달리 다른 노드 들로 이동할 필요가 없다. 포인터만 변경하면 된다. 따라서 삽입의 시간 복잡도는 O(1) 이다. 삭제시에는 포인터를 따라서 모든 노드를 뒤져보아야 한다.
- 히프 활용
완전 이진트리의 일종으로 우선 순위 큐를 위하여 특별히 만들어진 자료구조. 히프는 일정의 느슨한 정렬 상태를 유지한다.
히프의 효율은 O(log2n)으로서 다른 방법보다 상당히 유리하다. 여기서 다시 한번 O(n)과 O(log2n)은 큰 차이가 있다는 것을 유념하라.

### 히프 

히프는 영어사전에 찾아보면 "더미"라고 되어 있다. 컴퓨터 분야에서 히프는 완전이진트리 기반의 "더미"와 모습이 비슷한 특정한 자료구조를 의미한다. 히프는 여러 개의 값들 중에서 가장 큰 값이나 가장 작은 값을 빠르게 찾아내도록 만들어진 자료 구조이다. 히프는 간단히 말하면 부모노드의 키 값이 자식 노드의 키 값보다 항상 큰 이진 트리를 말한다.
이진 탐색 트리에서는 중복된 값을 허용하지 않았지만 히프에서는 중복된 값을 허용함에 유의하라.  

  - 최대 히프
부모노드의 값이 자식 노드의 키값보다 크거나 같은 완전 이진 트리
  - 최소 히프
부모 노드의 키값이 자식 노드의 키값보다 작거나 같은 완전 이진 트리

###### 배열을 이용하여 히프를 저장하면 완전 이진 트리에서처럼 자식 노드와 부모 노드를 쉽게 알 수 있다. 어떤 노드의 왼쪽이나 오른쪽 자식의 인덱스를 알고 싶으면 다음과 같은 식을 이용하면 된다.
  - 왼쪽 자식의 인덱스 = (부모의 인덱스) * 2
  - 오른쪽 자식의 인덱스 = (부모의 인덱스) * 2 + 1

###### 부모의 인덱스를 알고 싶은 경우
  - 부모의 인덱스 = (자식의 인덱스) / 2

###### 이진트리의 분류

- 포화 이진 트리 : 트리의 각 레벨의 노드가 꽉 차있는 이진 트리를 의미한다. 즉 높이 k 인 포화 이진 트리는 정확하게 2^k-1 개의 노드를 가진다.
- 완전 이진 트리 : 높이가 k 일 때 레벨 1부터 k - 1 까지는 노드가 모두 채워져 있고, 마지만 레벨 k 에서는 왼쪽부터 오른쪽으로 노드가 순서대로 채워져있는 이진트리이다. 마지막 레벨에서는 노드가 꽉 차있지 않아도 되지만 중간에 빈곳이 있어서는 안된다.
- 기타 이진 트리

###### 히프트리에서의 삽입 알고리즘 ( 의사 코드 버전 )

```c
{% raw %}
insert_max_heap(A, key); 

1. heap_size <- heap_size + 1;
2. 1 <- heap_size;
3. A[i] <- key;
4. while i /= 1 and A[i] > A[PARENT(i)] do 
5.      A[i] <-> A[PARENT];
6.      i <- PARENT(i);
{% endraw %}
```

###### 히프트리에서 삭제 알고리즘 ( 의사코드 버전 ) 

```c
{% raw %}
delete_max_heap(A);

1. item <- A[1]
2. A[1] = A[headp_size];
3. heap_size <- heap_size - 1;
4. i <- 2;
5. while i <=  heap_size do 
6.    if i < heap_size and A[i+1] > A[i];
7.      then largest <- i + 1;
8.      else largest <- i;
9.    if A[PARENT(largest)] > A[largest]
10.     then break;
11.   A[PARENT(largest)] <-> A[largest];
12.   i <- CHILD(largest);
13. 
14. return item; 
{% endraw %}
```

### 히프 트리 

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#define MAX_ELEMENT 200

typedef struct {
  int key;
} element;

typedef struct {
  element heap[MAX_ELEMENT];
  int heap_size;
} HeapType;

// 생성함수 
HeapType* create(){
  return (HeapType*)malloc(sizeof(HeapType));
}

// 초기화 함수 
void init(HeapType* h){
  h -> heap_size = 0;
}

// 현재 요소의 개수가 heap_size인 히프 h에 item을 삽입한다. 
// 삽입 함수 
void insert_max_heap(HeapType* h, element item){
  int i;
  i = ++ ( h -> heap_size );

  // 트리를 거슬러 올라가면서 부모 노드와 비교하는 과정 
  while( ( i != 1 ) && ( item.key > h -> heap[i / 2].key )) {
    h -> heap[i] = h -> heap[i / 2];
    i /= 2;
  }
  h -> heap[i] = item;
}

// 삭제 함수 
element delete_max_heap(HeapType* h){
  int parent, child;
  element item, temp;
  
  item = h -> heap[i];
  temp = h -> heap[(h -> heap_size)--];
  parent = 1;
  child = 2;

  while(chile <= h -> heap_size ){
    // 현재 노드의 자식노드 중 더 큰 자식노드를 찾는다. 
    if((child < h -> heap_size) && ( h-> heap[child].key ) < h -> heap[child + 1].key)
      child ++;
    if ( temp.key >= h -> heap[child].key ) break;

    h -> heap[parent] = h -> heap[child];
    parent = child;
    child *= 2;
  }
  h -> heap[parent] = temp;
  return item;
}

int main(void){
  element e1 = { 10 }, e2 = { 5 }, e3 = { 30 };
  element e4, e5, e6;
  HeapType* heap;

  heap = create();

  init(heap);

  insert_max_heap(heap, e1);
  insert_max_heap(heap, e2);
  insert_max_heap(heap, e3);

  e4 = delete_max_heap(heap);
  printf("< %d > ", e4.key );
  e5 = delete_max_heap(heap);
  printf("< %d > ", e5.key );
  e6 = delete_max_heap(heap);
  printf("< %d > \n", e6.key);
  
  free(heap);
  return 0;
}
{% endraw %}
```

### 히프정렬 

최대 히프를 이용하면 정렬을 할 수 있다. 한 번에 하나씩 요소를 꺼내서 배열의 뒤쪽부터 저장하면 된다. 배열 요소들은 값이 증가되는 순서로 정렬되게 된다. 이렇게 히프를 사용하는 정렬 알고리즘을 히트 정렬이라고 한다.

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h> 

void heap_sort(element a[], int n){
  int i;
  HeapType* h;

  h = create();
  init(h);
  for ( i = 0; i < n ; i ++){
    insert_max_heap(h, a[i]);
  }
  for ( i = ( n - 1 ); i >= 0; i -- ){
    a[i] = delete_max_heap(h);
  }
  free(h);
}

#define SIZE 8
int main(void){
  element list[SIZE] = { 23, 56, 11, 9, 56, 99, 27, 34 };
  heap_sort(list, SIZE);
  for(int i = 0; i < SIZE; i ++){
    printf("%d ", list[i].key);
  }
  printf("\n");
  return 0;
}
{% endraw %}
```

### 머신 스케쥴링 

어떤 공장에 동일한 기계가 m개 있다고 하자. 우리는 처리해야 하는 작업을 n개 가지고 있다. 각 작업이 필요로하는 기계의 사용시간은 다르다고 하자. 우리의 목표는 모든 기계를 풀가동하여 가장 최소의 시간안에 작업들을 끝내는 것이다. 이것을 머신 스케쥴링이라고 한다.
이 문제는 알고리즘 분야에서 상당히 유서 깊은 문제로 많은 응용 분야를 가지고 있다. 예를 들어서 서버가 여러 개 있어서 서버에 작업을 분재할 때도 사용할 수 있다. 최적의 해를 찾는 것은 상당히 어렵다. 하지만 근사의 해를 찾는 방법이 있다. 그 중 한 가지가 LPT(longest processing time first) 방법이다.
히프 정렬을 사용한다고 할 때 여기서는 최대 히프가 아닌 최소 히프를 사용한다. 최소 히프는 모든 기계의 종료시간을 저장하고 있다. 처음에는 어떤 기계도 사용되지 않으므로 모든 기계의 종료시간은 0이다. 히프에서 최소의 종료 시간을 가지는 기계를 삭제하여서 작업을 할당한다. 선택된 기계의 종료 시간을 업데이트하고 다시 히프에 저장한다.

```c
{% raw %}
#include <stdio.h>
#defined MAX_ELEMENT 200

typede struct {
  int id;
  int avail;
} element;

typedef struct {
  element heap[MAX_ELEMENT];
  int heap_size;
} HeapType;

// 생성함수 
HeapType* creat() {
  return (HeapType*)malloc(sizeof(HeapType));
}

// 초기화 함수 
voi init(Heaptype* h){
  h -> heap_size = 0;
}

// 현재 요소의 개수가 heap_size 인 히프 h에 item을 삽입한다. 
// 삽입 함수 
void insert_min_heap(HeapType* h, element item){
  int i;
  i = ++( h -> heap_size );

  // 트리를 거슬러 올라가면서 부모 노드와 비교하는 과정 
  while( ( i != 1) && ( item.avail < h -> heap[ i/ 2].avail)){
    h -> heap[i] = h -> heap[i / 2];
    i /= 2;
  }
  h -> heap[i] = item;
}

// 삭제 함수
element delete_min_heap(HeapType* h){
  int parent, child;
  element item, temp;

  item = h -> heap[i];
  temp = h -> heap[(h->heap_size)--];
  parent = 1;
  child = 2;
  while ( child <= h -> heap_size) {
    // 현재 노드의 자식 노드 중 더 작은 자식 노드를 찾는다. 
    if ( ( child < h -> heap_size ) && ( h -> heap[child].avail ) > h -> heap[child + 1].avail)
      child ++;
    if ( temp.avail < h -> heap[child].avail ) break;
    // 한 단계로 이동 
    h -> heap[parent] = h -> heap[child];
    parent = child;
    child *= 2;
  }
  h -> heap[parent] = temp;
  return item;
}


#define JOBS 7
#define MACHINES 3

int main(void){
  int jobs[JOBS] = { 8, 7, 6, 5, 3, 2, 1 };
  element m = { 0, 0 };
  HeapType* h;
  h = creat();
  init(h);

  // 여기서 avail의 값은 기계가 사용 가능하게 되는 시간이다. 
  for ( int i = 0 ; i < MACHINES ; i ++) {
    m.id = i + 1;
    m.avail = 0;
    insert_min_heap(h, m);
  }

  // 최소 히프에게 기계를 꺼내서 작업을 할당하고 사용 가능 시간을 증가 시킨 후에 
  // 다시 최소 히프에 추가한다. 
  for ( int i = 0 ; i < JOBS ; i ++ ) {
    m = delete_min_heap(h);
    printf("JOB %d을 시간 = %d 로부터 시간=%d 가지 기계 %d 번에 할당한다. ",
      i, m.avail, m.avail + jobs[i] - 1 , m.id );
    m.avail += jobs[i];
    insert_min_heap(h, m);
  }
  return 0;
}
{% endraw %}
```

### 허프만 코드

이진 트리는 각 글자의 빈도가 알려져 있는 메세지 내용을 압축하는데 사용될 수 있다. 이런 특별한 종류의 이진트리를 허프만 코딩 트리라고 부른다.  
테이블의 숫자는 빈도수라 불리운다. 각 숫자들은 영문 텍스트에서 해당 글자가 나타내는 회수이다. 이 빈도수를 이용하여 데이터를 압축할 때 각 글자들을 나타내는 최소길이의 엔코딩 비트열을 만들 수 있다. 데이터를 압축할 때는 우리가 흔히 사용하는 아스키 코드를 사용하지 않는다. 보통 전체 데이터의 양을 줄이기 위하여 고정된 길이를 사용하지 않고 가변 길이의 코드를 사용한다. 각 글자의 빈도 수에 따라서 가장 많이 등장하는 글자에는 짧은 비트열을 사용하고 잘 나오지 않는 글자에는 긴 비트열을 사용하여 전체의 크기를 줄이자는 것이다. 즉 많이 등장하는 e를 나타내기 위하여 2비트를 사용하고 잘 나오니 않는 z를 나타내기 위하여 15비트를 사용하나는 것이다.  
글자를 해독하는 문제를 생각하여 보자. 만약 한 글자당 3비트가 할당된다면 메세지를 해독하는 것은 아주 쉽다. 메시지를 3비트씩 끊어서 읽으면 된다. 만약 가변 길이 코드가 사용되었을 경우에는 어떻게 될 것인가? teen의 경우, 가변코드를 사용하여 코딩하면 01000010이 된다. 이 메시지를 어디서 끊어서 읽어서 해독해야 되는가? 첫 번째 글자의 경우, 하나의 글자가 3비트까지 가능하므로 00, 01, 010중의 하나이다. 그러나 코드 테이블을 보면 0 이나 010인 코드는 없기 때문에 첫 번째 글자는 첫번째 글자는 01이 분명하고 01은 t이다. 또 다음 코드는 0, 00, 000 중의 하나이다. 그러나 같은 이유로 다음 코드는 00이 된다.  
이러한 해독과정을 가능하게 하는 원인은 코드를 관찰하여 보면 모든 코드가 다른 코드의 첫 부분이 아니라는 것이다. 따라서 코딩된 비트열을 왼쪽에서 오른쪽을 조사하여 보면 정확히 하나의 코드만 일치하는 것을 알 수 있다. 이러한 특수한 코드를 만들기 위하여 이진 트리를 사용할 수 있다. 이런 종류의 코드를 호프만 코드라고한다.  
이 허프만 트리에서 왼쪽 간선은 비트 1을 나타내고 오른쪽 간선은 비트 0을 나타낸다.  
각 글자에 대한 호프만 코드는 단순히 루트 노드에서 단말 노드까지의 경로에 있는 간선의 라벨값을 읽으면 된다. 즉 빈도수 6에 해당하는 글자인 i의 코드는 100이 된다. 같은 식으로 하여 다른 글자에 대한 허프만 코드 값을 얻을 수 있다. 허프만 코드 알고리즘에서 가장 작은 2개의 빈도수를 얻는 과정이 있다. 이것은 히프트리를 이용하면 가장 효율적으로 구성될 수 있다. 여기서는 최소 히프를 이용하여야 한다. 따라서 히프 트리 코드를 약간 변경하였다. 다음의 프로그램은 먼저 각 빈도수를 단일 노드로 만든 다음에 가장 작은 빈도수를 갖는 노드 2개를 합쳐서 하나의 트리로 만드는 과정을 되풀이 한다.  

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#define MAX_ELEMENT 200

typedef struct TreeNode {
  int weight;
  char ch;
  struct TreeNode *left;
  struct TreeNode *right;
} TreeNode;

typedef struct {
  TreeNode* ptree;
  char ch;
  int key;
} element;

typedef struct {
  element heap[MAX_ELEMENT];
  int heap_size;
} HeapType;

// 생성함수 
HeapType* create() {
  return (HeapType*)malloc(sizeof(HeapType));
}

// 초기화 함수 
void init(HeapType* h){
  h -> heap_size = 0;
}

// 현재 요소의 개수가 heap_size 인 히프 h 에 item을 삽입한다. 
// 삽입 함수 
void insert_min_heap(HeapType* h, element item){
  int i;
  i = ++ ( h -> heap_size );

  // 트리를 거슬러 올라가면서 부모 노드와 비교하는 과정 
  while( ( i != 1) && ( item.key < h -> heap[ i / 2].key )) {
    h -> heap[i] = h -> heap[i / 2];
    i /= 2;
  }
  h -> heap[i] = item;
}

// 삭제 함수 
element delete_min_heap(HeapType* h){
  int parent, child;
  element item, temp;

  item = h -> heap[1];
  item = h -> heap[(h -> heap_size)--];
  parent = 1;
  child = 2;
  while ( child <= h -> heap_size ) {
    // 현재 노드의 자식 노드 중 더 작은 자식 노드를 찾는다. 
    if ( ( child < h -> heap_szie ) && ( h -> heap[child].key > h -> heap[child + 1].key ) )
      child ++;
    if( temp.key < h -> heap[child].key ) break;
      h -> heap[parent] = h -> heap[child];
      parent = child;
      child *= 2;
  }
  h -> heap[parent] = temp;
  return item;
}


// 이진 트리 생성 함수 
TreeNode* make_tree(TreeNode* left, TreeNode* right){
  TreeNode *node = (TreeNode*)malloc(sizeof(TreeNode));
  node -> left = left;
  node -> right = right;
  return node;
}

// 이진 트리 제거 함수 
void destroy_tree(TreeNode* root){
  if ( root == NULL ) return;
  destroy_tree(root -> left);
  destroy_tree(root -> right);
  free(root);
}

int is_leaf(TreeNode* root){
  return !(root -> left) && !(root -> right);
}

void print_array(int codes[], int n){
  for ( int i = 0; i < n ; i ++){
    printf("%d", codes[i]);
  }
  printf("\n");
}

void print_codes(TreeNode* root, int codes[], int top){
  // 1 을 저장하고 순환 호출한다. 
  if ( root -> left ) {
    codes[top] = 1;
    print_codes(root -> left, codes, top + 1);
  }

  // 0을 저장하고 순환호출한다. 
  if( root -> right) {
    codes[top] = 0;
    print_codes(root -> right, codes, top + 1);
  }

  // 단말노드이면 코드를 출력한다. 
  if(is_leaf(root)){
    printf("%c : ", root -> ch);
    print_array(codes, top);
  }
}

// 허프만 코드 생성 함수 
void huffman_tree(int freq[], char ch_list[], int n ){
  int i;
  TreeNode *node , *x;
  HeapType* heap;
  element e1, e2, e3;
  int codes[100];
  int top = 0;

  heap = create();
  init(heap);
  for( i = 0; i < n ; i ++){
    node = make_tree(NULL, NULL);
    e.ch = node -> ch = ch_list[i];
    e.key = node -> weight = freq[i];
    e.ptree = node;
    insert_min_heap(heap, e);
  }
  for ( i = 1; i < n ; i ++){
    // 최소값을 가지는 두개의 노드를 삭제 
    e1 = delete_min_heap(heap);
    e2 = delete_min_heap(heap);
    // 두개의 노드를 합친다. 
    x = make_tree(e1.ptree, e2.ptree);
    e.key = x => weight = e1.key + e2.key;
    e.ptree = x;
    printf("%d + %d -> %d \n", e1.key, e2.key, e.key );
    insert_min_heap(heap, e);
  }
  e = delete_min_heap(heap);
  print_codes(e.ptree, codes, top);
  destroy_tree(e.ptree);
  free(heap);
}

int main(void){
  char ch_list[] = { 's', 'i' , 'n', 't', 'e' };
  int freq[] = { 4, 6, 8, 12, 15 };
  huffman_tree(freq, ch_list, 5);
  return 0;
}
{% endraw %}
```