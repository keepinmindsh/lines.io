---
title:  "Tree"
excerpt: "Tree에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-10T21:27:00-05:00
layout: single
---

> 그냥 웃으면 그렇게 화날 일도 별로 없다. 

***

만약 자료가 계층적인 구조를 가지고 있다면 어떻게 해야하는 가? 예를 들어 가족의 가계도, 회사의 조직도, 컴퓨터의 디렉토리 구조 등의 자료는 어떻게 표현해야 하는가?
트리는 이러한 계층적인 자료를 표현하는데 적합한 자료구조이다.  

A는 B의 부모 노드가 된다. 반대로 B는 A의 자식 노드(children)가 된다. B와 C와 D는 형제관계(sibiling)이다. 조상, 후손, 손자, 조부모 노드도 마찬가지이다. 조상 노드(ancestor)란 루트 노드에서 임의 노드까지 경로를 이루고 있는 노드들을 말한다. 후손노드(descendent node)는 임의의 노드 하나에 연결된 모드 노드들을 의미한다. 즉 어떤 노드의 서브 트리에 속하는 모든 노드들은 후손 노드이다. 또한 자식 노드가 없는 노드를 단말 노드라고 한다.
노드의 차수는 어떤 노드가 가지고 있는 자식 노드의 개수를 의미한다. 트리의 높이는 트리가 가지고 있는 최대 레벨을 말한다. 도한 나무가 모이는 숨이 되듯이 트리들의 집합을 포리스트라고 한다.  

- 트리의 종류 
  - 일반 트리 
  - 이진 트리

### 이진트리 

모든 노드가 2개의 서브트리를 가지고 있는 트리를 이진 트리라고 한다. 서브 트리는 공집합일 수 있다. 노드에는 최대 2개 까지의 자식 노드가 존재할 수 있고 모든 노드의 차수가 2 이하가 된다. 공집합도 이진 트리라는 점에 주의하라. 또한 이진 트리에는 서브 트리간의 순서가 존재한다. 따라서 왼쪽 서브 트리와 오른쪽 서브 트리는 서로 구별된다.
- 이진 트리의 모든 노드는 차수가 2이하이다. 즉 자신 노드의 개수가 2이하이다. 반면 일반 트리는 자식 노드의 개수에 제한이 없다.
- 일반 트리와 달리 이진 트리는 노드를 하나도 같지 않을 수도 있다.
- 서브 트리간에 순서가 존재한다는 점도 다른 점이다. 따라서 왼쪽 서브트리와 오른쪽 서브트리를 구별한다.

###### 이진트리의 성질

n 개의 노드를 가진 이진 트리는 정확하게 n - 1 의 간선을 가진다. 그 이유는 이진트리에서의 노드는 루트를 제외하면 정확하게 하나의 부모 노드를 가진다. 그리고 부모와 자식간에는 정확하게 하나의 간선만이 존재한다.
높이가 h인 이진 트리는 , 최소 h 개의 노드를 가지며 최대 2^h - 1 개의 노드를 가진다.
n 개의 노드를 가지는 이진트리의 높이는 최대 n 이거나 최소 [log2(n+1)]이 된다.

###### 이진트리의 분류

- 포화 이진 트리 : 트리의 각 레벨의 노드가 꽉 차있는 이진 트리를 의미한다. 즉 높이 k 인 포화 이진 트리는 정확하게 2^k-1 개의 노드를 가진다.
- 완전 이진 트리 : 높이가 k 일 때 레벨 1부터 k - 1 까지는 노드가 모두 채워져 있고, 마지만 레벨 k 에서는 왼쪽부터 오른쪽으로 노드가 순서대로 채워져있는 이진트리이다. 마지막 레벨에서는 노드가 꽉 차있지 않아도 되지만 중간에 빈곳이 있어서는 안된다.
- 기타 이진 트리

### 배열 표현법

```c

{% raw %}
#include <stdio.h>
#include <stdlib.h>
#include <memory.h>

typedef struct TreeNode {
  int data;
  struct TreeNode *left, *right;
} TreeNode

int main(void){
  TreeNode *n1, *n2, *n3;
  n1 = (TreeNode *)malloc(sizeof(TreeNode));
  n2 = (TreeNode *)malloc(sizeof(TreeNode));
  n3 = (TreeNode *)malloc(sizeof(TreeNode));

  n1 -> data = 10; // 첫번째 노드를 설정한다.
  n1 -> left = n2;
  n1 -> right = n3;
  n2 -> data = 20; // 두번째 노드를 설정한다. 
  n2 -> left = NULL;
  n2 -> right = NULL;
  n3 -> data = 30; // 세번째 노드를 설정한다. 
  n3 -> left = NULL;
  n3 -> right = NULL;

  free(n1);
  free(n2);
  free(n3);

  return 0;
}
{% endraw %}

```

### 이진 트리 순회 

데이터는 노드의 데이터 필드를 이용하여 저장된다. 이진 트리를 순회한다는 것은 이진 트리에 속하는 모든 노드를 한 번씩 방문하여 노드가 가지고 있는 데이터를 목적에 맞게 처리하는 것을 의미한다.

###### 이진 트리 순회 방법

이진 트리를 순회하는 표준적인 방법에는 전위, 중위, 후위의 3가지 방법이 있다. 루트 방문하는 작업을 V라고 하고 왼쪽 서브 트리 방문을 L, 오른쪽 서브트리 방문을 R이라고 하면 다음 과 같이 3가지 방법을 생각할 수 있다. 즉 루트를 서브 트리에 앞서서 먼저 방문하면 전위순회, 루트를 왼쪽과 오른쪽 서브트리 중간에 방문하면 중위순회, 루트를 서브 트리 방문 후에 방문하면 후위 순회가 된다.

- 전위 순회 : VLR
  - 루트 노드를 방문한다.
  - 왼쪽 서브트리를 방문한다.
  - 오른쪽 서브트리를 방문한다.
- 중위 순회 : LVR
  - 왼쪽 서브트리를 방문한다.
  - 루트노드를 방문한다.
  - 오른쪽 서브트리를 방문한다.
- 후위 순회 : LRV
  - 왼쪽 서브트리의 모든 노드를 방문한다.
  - 오른쪽 서브트리의 모든 모드를 방문한다.
  - 루트노드를 방문한다.

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#include <memory.h>

typedef struct TreeNode {
  int data;
  struct TreeNode *left, *right;
} TreeNode;

TreeNode n1 = { 1, NULL, NULL };
TreeNode n2 = { 4, &n1 , NULL };
TreeNode n3 = { 16, NULL, NULL };
TreeNode n4 = { 25, NULL, NULL };
TreeNode n5 = { 20, &n3, &n4 };
TreeNode n6 = { 15, &n2, &n5 };
TreeNode *root = &n6;

void inorder(TreeNode *root){
  if( root != NULL ){
    inorder(root -> left);
    printf("[%d] ", root -> data);
    inorder(root -> right);
  }
}

void preorder(TreeNode *root){
  if( root != NULL ) {
    printf("[%d] ", root -> data);
    preorder(root -> left);
    preorder(root -> right);
  }
}

void postorder(TreeNode *root){
  if( root != NULL ) {
    postorder(root -> left);
    postorder(root -> right);
    printf("[%d] ", root -> data);
  }
}

int main(void){
  printf("중위 순회=");
  inorder(root);
  printf("\n");

  printf("전위 순회=");
  pre0order(root);
  printf("\n");

  printf("후위 순회=");
  postorder(root);
  printf("\n");

  return 0;
}
{% endraw %}
```

### 반복에 의한 순회 

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#include <memory.h>

typedef struct TreeNode {
  int data;
  struct TreeNode *left, *right;
} TreeNode;

#defined SIZE 100
int top = -1;
TreeNode *stack[SIZE];

void push(TreeNode *p){
  if ( top < SIZE - 1 )
    stack[++top] = p;
}

TreeNode *pop(){
  TreeNode *p = NULL;
  if ( top >= 0 )
    p = stack[top--];
  return p;
}

void inorder_iter(TreeNode *root){
  while(1) {
    for( ; root; root = root -> left )
      push(root);
    root = pop();
    if(!root) break;
    printf("[%d] ", root -> data );
    root = root -> right;
  }
}

TreeNode n1 = { 1, NULL, NULL };
TreeNode n2 = { 4, &n1 , NULL };
TreeNode n3 = { 16, NULL, NULL };
TreeNode n4 = { 25, NULL, NULL };
TreeNode n5 = { 20, &n3, &n4 };
TreeNode n6 = { 15, &n2, &n5 };
TreeNode *root = &n6;

int main(void){
  printf("중위 순회 =");
  inorder_iter(root);
  printf("\n");
  return 0;
}
{% endraw %}
```

### 레벨 순회 

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#include <memeory.h> 

typedef stuct TreeNode {
  int data;
  struct TreeNode *left, *right;
} TreeNode;

#define MAX_QUEUE_SIZE 100
typedef TreeNode * element;
typedef struct {
  element data[MAX_QUEUE_SIZE];
  int front, rear;
} QueueType;

void error(char *message){
  fprintf(stderr, "%s\n" , message);
  exit(1);
}

void init_queue(QueueType *q){
  q -> front = q -> rear = 0;
}

void is_empty(QueueType *q){
  return ( q -> front  == q -> rear );
}

int is_full(QeueuType *q){
  return ( ( q -> rear + 1) % MAX_QUEUE_SIZE == q -> front );
}

void enqueue(QueueType *q, element item){
  if( is_full(q) )
    error("큐 포화상태입니다.");
  q -> rear = ( q -> rear + 1) % MAX_QUEUE_SIZE;
  q -> data[q-> rear] = item;
}

void dequeue(QueueType *q){
  if(is_empty(q))
    error("큐가 공백상태입니다.");
  q -> front = ( q -> front + 1) % MAX_QUEUE_SIZE;
  return q -> data[q -> front];
}

void level_order(TreeNode *ptr){
  QeueuType q;

  init_queue(&q);

  if ( ptr == NULL ) return;

  enqueue(&q, ptr);

  while(!is_empty(&q)){
    ptr = dequeue(&q);
    printf(" [%d] ", ptr -> data );
    if ( ptr -> left )
      enqueue(&q, ptr -> left);
    if ( ptr -> right )
      enqueue(&q, ptr -> right);
  }
}


TreeNode n1 = { 1, NULL, NULL };
TreeNode n2 = { 4, &n1 , NULL };
TreeNode n3 = { 16, NULL, NULL };
TreeNode n4 = { 25, NULL, NULL };
TreeNode n5 = { 20, &n3, &n4 };
TreeNode n6 = { 15, &n2, &n5 };
TreeNode *root = &n6;

int main(void){
  printf("레벨 순회 =");
  level_order(root);
  printf("\n");
  return 0;
}
{% endraw %}
```

### 수식 트리 처리 

이진 트리는 수식 트리를 처리하는 데 사용될 수 있다. 수식 트리는 산술 연산자와 피 연산자로 만들어진다. 피연산자들은 단말 노트가 되며 연산자는 비단말 노드가 된다.

```c
{% raw %}

#include <stdio.h>
#include <stdlib.h> 

typedef struct TreeNode {
  int data;
  struct TreeNode *left, *right;
} TreeNode;


TreeNode n1 = { 1, NULL, NULL };
TreeNode n2 = { 4, NULL , NULL };
TreeNode n3 = {'*', &n1 , &n2 };
TreeNode n4 = { 16, NULL, NULL };
TreeNode n5 = { 25, NULL, NULL };
TreeNode n6 = { '+', &n4 , &n5 };
TreeNode n6 = { '+', &n3 , &n6 };
TreeNode *exp = &n7;

int evaluate(TreeNode *root){
  if ( root == NULL )
    return 0;
  if ( root -> left == NULL && root -> right == NULL )
    return root -> data;
  else {
    int op1 = evaluate(root -> left);
    int op2 = evaluate(root -> right);
    printf("%d %c %d을 계산합니다. \n", op1, root -> data, op2);
    switch(root -> data) {
      case '+' :
        return op1 + op2;
      case '-' :
        return op1 - op2;
      case '*' :
        return op1 * op2;
      case '/' :
        return op1 / op2;
    }
  }
  return 0;
}

int main(void) {
  printf("수식의 값은 %d 입니다.", evaluate(exp));
  return 0;
}
{% endraw %}
```

### 디렉토리 용량 계산

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>

typedef struct TreeNode {
  int data;
  struct TreeNode *left, *right;
} TreeNode;

int calc_dir_size(TreeNode *root){
  int left_size, right_size;
  if( root == NULL ) return 0;

  left_size = calc_dir_size(root -> left);
  right_size = clac_dir_size(root -> right);
  return ( root -> data + left_size + right_size );
}

int main(void){
  TreeNode n4 = { 500, NULL, NULL };
  TreeNode n5 = { 200, NULL, NULL };
  TreeNode n3 = { 100, &n4, &n5 };
  TreeNode n2 = { 50, NULL, NULL };
  TreeNode n1 = { 0, &n2, &n3 };

  printf("디렉토리의 크기 = %d\n", calc_dir_size(&n1));
}

{% endraw %}
```

### 이진트리 추가 연산

```c
// 노드의 개수 
int get_node_count(TreeNode *node){
  int count = 0;

  if( node != NULL ){
    count = 1 + get_node_count(node -> left) + get_node_count(node -> right);
  }
}

// 단말 노드 개수 구하기 
int get_leaf_count(TreeNode *node){
  int count = 0;

  if ( node != NULL ) {
    if ( node -> left == NULL && node -> right == NULL )
      return 1;
    else 
      count = get_leaf_count(node -> left) + get_leaf_count(node -> right );
  }

  return count;
}

// 높이 구하기 
int get_height(TreeNode *node) {
  int height = 0;

  if ( node != NULL ) {
    height = 1 + max(get_height(node -> left), get_height(node -> right ));
  } 

  return height;
}
```

### 스레드 이진 트리 

이진 트리 순회는 순환 호출을 사용한다. 순환 호출은 함수를 호출해야되므로 상당히 비효율 적일 수가 있다. 이진 트리 순회도 노드의 개수가 많이지고 트리의 높이가 커지게 되면 상당히 비효율 적일 수 있다. 그렇다면 순환 없이 노드를 순회할 수 있는 방법은 무엇이 있을까?
우리는 이진 트리의 노드에 많은 NULL링크들이 존재함을 알고 있다. 만약 트리의 노드의 개수를 n개 라고 하면 각 노드당 2개의 링크가 있으므로 총 링크의 개수는 2n이 되고 이들 링크 중에서 n - 1 개의 링크들이 루트노드를 제외한 n -1 개의 다른 노드들을 가리킨다. 따라서 2n 개 중에서 n - 1 은 NULL 링크가 아니지만 나머지 n + 1 개의 링크는 NULL임을 알 수 있다. 따라서 하나의 아이디어는 이들 NULL 링크를 잘 사용하여 순환호출 없이도 트리의 노드들을 순회할 수 있도록 하자는 것이다.  

- 노드의 개수 : 2개 ( n )
- 총 링크의 수 : 4개 ( 4n )
- 루트노드를 제외한 다른 노드를 가리키는 링크 개수 : 1개 ( n - 1 )
4(2n)개 중에 1(n-1)개는 NULL링크가 아니지만 나머지 3(n + 1)개의 링크는 NULL 임을 알 수 있다.
이들 NULL 링크 중에 중위 순회 시에 선행 노드인 중위 선행자나 중위 순회시에 후속 노드인 중위 후속자를 저장시켜 놓은 트리가 스레드 이진 트리다.  

스레드 트리는 순회를 빠르게 하는 장점이 있으나 문제는 스레드를 설정하기 위하여 삽입이나 삭제 함수가 더 많은 일을 하여야 한다.

```c
{% raw %}
#include <stdio.h>
#define TRUE 1
#define FLASE 0

typedef struct TreeNode {
  int data;
  struct TreeNode *left, *right;
  int is_thread;
} TreeNode;

TreeNode n1 = { 'A'}

TreeNode * find_successor(TreeNode * p){
  TreeNode * q = p -> right;

  if( q == NULL || P -> is_thread == TRUE )
    return q;

  while( q -> left != NULL ) q = q -> left;
  return q;
}

void thread_inorder(TreeNode *t){
  TreeNode *q;
  
  q = t;

  while ( q -> left ) q = q -> left;
  do {
    printf("%c -> " , q -> data);
    q = find_successor(q);
  } while (q);
}

int main(void){
  n1.right = &n3;
  n2.right = &n7;
  n4.right = &n6;

  thread_inorder(exp);
  printf("\n");
  return 0;
}
{% endraw %}
```

### 이진 탐색 트리

이진 트리 기반의 탐색을 위한 자료 구조이다. 탐색은 가장 중요한 컴퓨터 응용 의 하나이다. 탐색은 우리의 일상 생활에서 많이 사용되는데 전화번호부에서 전화번호를 찾거나, 사전에서 단어를 찾거나, 어떤 특정한 날에 선약이 없는가를 검사할 때 사용된다.  

탐색 : 레코드의 집합에서 특정한 레코드를 찾아내는 작업을 의미한다.  
레코드 들은 보탇 키라고 불리는 하나의 필드에 의해 식별할 수 있다. 일반적인 경우 키는 다른 키와 중복되지 않는 고유한 값을 가지며 이러한 키를 사용하면 각각의 레코드를 구별할 수 있을 것이다. 이러한 키를 주요키(primary key)라고 부른다.  

- 모든 원소의 키는 유일한 키를 가진다.
- 왼쪽 서브 트리 키들은 루트 키보다 작다.
- 오른쪽 서브 트리의 키들은 루트의 키보다 크다.
- 왼쪽과 오른쪽 서브 트리도 이진 탐색 트리이다.

###### 순환적인 탐색 연산

이진 트리 탐색 연산을 위해서 중요한 요소를 결국 레코드를 저장할 때 이진 트리 구조로 얼마나 "효과적"으로 저장하느냐에 있다고 본다. 이는 내가 사용할 데이터 자료구조에 따라 탐색을 위해서 사용하는 알고리즘이 달라 질 수 있기 때문이며, 결국에는 "데이터"를 저장하는 방식이 가장 중요한 요소가 되는 것이다.

```c
// 순환적인 탐색함수                           
TreeNode *search(TreeNode *node, int key){
  if ( node == NULL ) return NULL;
  if ( key == node -> key ) return node;
  else if( key < node -> key )
    return search(node -> left, key);
  else 
    return search(node -> right , key);
}

// 반복적인 탐색함수 
TreeNode *search(TreeNode *node, int key ){
  while ( node != NULL ) {
      if ( key == node -> key ) return node;
      else if ( key < node -> key )
        node = node -> left;
      else 
        node = node -> right;
  }
  return NULL;
}

// 이진 트리 삽입 프로그램
TreeNode *insert_node(TreeNode *node, int key){
  // 트리가 공백이면 새로운 노드를 반환한다. 
  if ( node == NULL) return new_node(key);

  // 그렇지 않으면 순환적으로 트리를 내려간다. 
  if( key < node -> key )
    node -> left = insert_node(node -> left, key);
  else if( key > node -> key)
    node -> right = insert_node(node -> right, key);

  // 변경된 루트 포인터를 반환한다. 
  return node;
}

TreeNode *new_node(int item){
  TreeNode *temp = (TreeNode*)malloc(sizeof(TreeNode));
  temp -> key = item;
  temp -> left = temp -> right = NULL;
  return temp;
}
      
```

###### 이진 탐색 트리 삭제 연산

- 삭제하려는 노드가 단말 노드 일 경우
- 삭제하려는 노드가 하나의 왼쪽이나 오른쪽 서브 트리 중 하나만 가지고 있는 경우
- 삭제하려는 노드가 두 개의 서브 트리 모두 가지고 있는 경우

```c
{% raw %}
#include <stdio.h>
#include <stdlib.h> 

typedef int element;
typedef struct TreeNode {
  element key;
  struct TreeNode *left, *right;
} TreeNode;

TreeNode *search(TreeNode *node, int key){
  if ( node == NULL ) return NULL;
  if ( key == node -> key ) return node;
  else if ( key < node -> key )
    return search(node -> left, key);
  else 
    return search(node -> right, key);
}

TreeNode *new_node(int item){
  TreeNode *item = (TreeNode*)malloc(sizeof(TreeNode));

  temp -> key = item;
  temp -> left = temp -> right = NULL;
  return temp;
}

TreeNode *insert_node(TreeNode *node, int key){
  if ( node == NULL ) return new_node(key);

  if( key < node -> key){
    node -> left = insert_node(node -> left, key);
  }else if(key -> node -> key){
    node -> right = insert_node(node -> right, key);
  }

  return node;
}

TreeNode *min_value_node(TreeNode *node){
  TreeNode *current = node;

  while(current -> left != NULL){
    current = current -> left;
  }

  return current;
}

TreeNode *delete_node(TreeNode *root, int key){
  if(root == NULL) return root;
  // 만약 키가 루트보다 작으면 왼쪽 서브 트리에 있는 것임 
  if(key < root -> key){
      root -> left = delete_node(root -> left, key);
  }else if( key > root -> key ){
    // 만약 키가 루트보다 크면 오른쪽 서브 트리에 있는 것임 
    root -> right = delete_node(root -> right, key);
  }else{ // 키가 루트와 같으면 이 노드를 삭제하면 됨 
    // 첫 번째나 두 번째 경우 
    if ( root -> left == NULL ){
      TreeNode *temp = root -> right;
      free(root);
      return temp;
    }else if ( root -> right == NULL){
      TreeNode *temp = root -> left;
      free(root);
      return temp;
    }

    // 세 번째 경우 
    TreeNode *temp = min_value_node(root -> right);

    // 중외 순회시 후계 노드를 복사한다. 
    root -> key = temp -> key;
    // 중외 순회시 후계 노드를 삭제한다. 
    root -> right = delete_node(root -> right, temp -> key);
  }
  return root;
}

void inorder(TreeNode *root){
  if(root){
    inorder(root -> left);
    printf("[%d]", root -> key);
    inorder(root -> right);
  }
}

int main(void){
  TreeNode *root = NULL;
  TreeNode *tmp = NULL;

  root = insert_node(root, 30);
  root = insert_node(root, 20);
  root = insert_node(root, 10);
  root = insert_node(root, 40);
  root = insert_node(root, 50);
  root = insert_node(root, 60);

  printf("이진 탐색 트리 중위 순회 결과 \n");

  inorder(root);

  printf("\n\n");

  if(search(root, 30) != NULL){
    printf("이진 탐색 트리에서 30을 발견함 \n");
  }else{
    printf("이진 탐색 트리에서 30을 발견 못함 \n");
  }

  return 0;
}
{% endraw %}
```

###### 이진 탐색 트리 분석
{% raw %}
이진 탐색 트리에서의 탐색, 삽입, 삭제 연산의 시간 복잡도는 트리의 높이를 h라고 했을 때 O(h)가 된다, 따라서 n 개의 노드를 가지는 이진 탐색 트리의 경우, 일반적인 이진 트리의 높이는 [log2n] 이므로 이진 탐색 트리 연산의 평균적인 경우 시간 복잡도는 O(log2h)이다.  

사례 개발 : 이진 탐색 트리를 이용한 영어 사전 프로그램
{% endraw %}
```c
{% raw %}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <memory.h>

#defined MAX_WORD_SIZE  100
#defined MAX_MEANING_SIZE 200

typedef struct {
  char word[MAX_WORD_SIZE];
  char meaning[MAX_MEANING_SIZE];
} element;

typedef struct TreeNode {
  element key;
  TreeNode *left, *right;
} TreeNode;

// 만약 e1 < e2 이면 -1 반환 
// 만약 e1 == e2 이면 0 반환
// 만약 e1 > e2 이면 1 반환 
int compare(element e1, element e2){
  return strcmp(e1.word, e2.word);
}

// 이진 탐색 트리 출력 함수 
void display(TreeNode *p){
  if( p != NULL){
    printf("(");
    display( p -> left );
    printf("%s:%s", p -> key.word , p -> key.meaning );
    display(p -> right);
    printf(")");
  }
}

// 이진 탐색 트리 탐색 함수  
TreeNode *search(TreeNode *root, element key) {
  TreeNode *p = root;

  while ( p != NULL ) {
    if( compare(key, p -> key ) == 0 )
      return p;
    else if (compare(key, p -> key ) < 0)
      p = p -> left;
    else if (compare(key, p -> key ) > 0 )
      p = p -> right;
  }
  return p;
}

TreeNode *new_node(element item){
  TreeNode *temp = (TreeNode*)malloc(sizeof(TreeNode));
  temp -> key = item;
  temp -> left = temp -> right = NULL;
  return temp;
}

TreeNode *insert_node(TreeNode *node, element key){
  if( node == NULL ) return new_node(key);

  if ( compare(key, node -> key) < 0 ){
    node -> left = insert_node(node -> left, key);
  } else {
    node -> right = insert_node(node -> right, key);
  }

  return node;
}

TreeNode *min_value_node(TreeNode *node){
  TreeNode *current = node;
  while ( current -> right != NULL )
    current = current -> left;
  return current;
}

TreeNode *delete_node(TreeNode *root, element key){
  if( root == NULL ) return root;
  // 만약 키가 루트보다 작으면 왼쪽 서브 트리에 있는 것임 
  if ( compare(key, root -> key ) < 0 )
    root -> left = delete_node(root -> left, key);
  // 만약 키가 루트보다 크면 오른쪽 서브 트리에 있는 것임 
  if ( compare(key, root -> key) > 0 )
    root -> right = delete_node(root -> right, key);
  // 키가 루트와 같으면 이 노드를 삭제하면 됨 
  else {
    // 첫 번째나 두 번재 경우
    if( root -> elft == NULL) {
      TreeNode *temp = root -> right;
      free(root);
      return temp;
    }else if(root -> right == NULL){
      TreeNode *temp = root -> left;
      free(root);
      return temp;
    } 
    //새번째 경우 

    TreeNode *temp = min_value_node(root -> right);

    root -> key = temp -> key;

    root -> right = delete_node(root -> right, temp -> key);
  }
  return root
}

void help() {
  printf("\n**** i: 입력, d:삭제, s: 탐색, p: 출력, q: 종료 ****: ")
}

int main(void){
  char command;
  element e;
  TreeNode *root = NULL;
  TreeNode *tmp;

  do {
    help();
    command = getchar();
    getchar();
    switch(command){
      case 'i' :
        printf("단어 : ");
        gets(e.word);
        printf("의미 : ");
        gets(e.meaning);
        root = insert_node(root, e);
        break;
      case 'd' :
        printf("단어 : ");
        gets(e.word);
        root = delete_node(root, e);
        break; 
      case 'p' :
        display(root);
        printf("\n");
        break;
      case 's' :
        printf("단어:");
        gets(e.word);
        tmp = search(root, e);
        if ( tmp != NULL ){
          printf("의미:%s\n", e.meaning);
        }
        break;
    }
  } while ( command != 'q' );
  return 0;
}
{% endraw %}
```