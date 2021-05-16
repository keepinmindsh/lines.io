---
title:  "탐색"
excerpt: "탐색에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-15T09:00:00-05:00
---

> 삶에 대한 예의를 지킬 것.

***

탐색은 기본적으로 여러 개의 자료 중에서 원하는 자료를 찾는 작업이다. 탐색을 위하여 사용되는 자료 구조는 배열, 연결 리스트, 트리, 그래프 등 매우 다양할 수 있다. 탐색 중에서 가장 기초적인 방법은 배열을 사용하여 자료를 저장하고 찾는 것이다. 그러나 탐색 성능을 향상하고자 한다면 이진 탐색 트리와 같은 보다 진보된 방법으로 자료를 저장하고 탐색해야 한다.  

탐색의 단위는 항목이다. 항목은 가장 간단하게는 숫자일 수도 있고 아니면 구조체가 될 수 도 있다. 항목 안에는 항목과 항목을 구별시켜주는 키가 존재한다. 이를 탐색 키라고 한다. 탐색이란 탐색키와 데이터로 이루어진 여러 개의 항목 중에서 원하는 탐색 키를 가지고 있는 항목을 찾는 것이다.  

### 순차 탐색 

```c

int seq_search(int key, int low, int high){
  int i;

  for ( i = low; i <= high ; i ++ )
    if ( list[i] == key ) {
      return i;
    }
    return -1;
}
    
```

### 개선된 순차 탐색

리스트의 끝을 테스트하는 비교연산이 있고 반복문 안에 키 값의 비교 연산이 있다.리스트의 끝을 테스트 하는 비교 연산을 줄이기 위해 리스트의 끝에 찾고자 하는 키 값을 저장하고 반복문의 탈출 조건을 키 값을 찾을 때 까지로 설정하도록 아래와 같이 수정되었다. 탐색이 성공했을 때는 반복문의 인덱스 i는 찾는 항목의 위치를 가리키게 되고, 이 값을 반환하는 반면에 탐색이 실패했을 경우에는 -1을 반환한다. 이는 비교 연산의 수를 반으로 줄일 수 있으므로 탐색 성능을 향상 시킨다. 즉 i 값이 리스트의 끝에 도달하였는지를 매번 비교하지 않아도 된다.

```c

int seq_search2(int key, int low, int high){
  int i;

  list[high + 1] = key;
  for ( i = low; list[i] != key; i ++)
    ;
  if ( i == ( high + 1)) return -1;
  else return i;
}

```

###### 순차탐색의 시간복잡도

순차 탐색 알고리즘은 리스트의 처음부터 탐색을 시작하여 해당 항목을 찾거나 모든 항목을 검색할 때 까지 항목의 키값을 비교한다. 따라서 순차 탐색 알고리즘의 복잡도는 두가지 경우로 나누어 볼 수 있다. 탐색에 성공하는 경우에는 리스트에 있는 키의 위치에 따라 비교 횟수가 결정되는데, 모든 키가 탐색될 확률이 동일하다고 가정하면 평균 비교 횟수는 아래와 같다.

```c

( 1 + 2 + 3 + ... + n ) / n = ( n + 1) / 2 

```

###### 정렬된 배열에서의 탐색

정렬된 배열의 탐색에는 이진 탐색이 가장 적합하다 이진 탐색은 어느 중앙에 있는 값을 조사하여 찾고자 하는 항목이 왼쪽 또는 오른쪽 부분 배열에 있는지를 알아내어 탐색의 범위를 반으로 줄인다. 이러한 방법에 의해 매 단계에서 검색해야할 리스트의 크기를 반으로 줄인다. 이진 탐색에서는 비교가 이루어질 때마다 탐색 범위가 급격하게 줄어든다. 찾고자 하는 항목이 속해있지 않는 부분은 전혀 고려할 필요가 없기 때문이다. 이러한 방법을 반복적으로 사용하는 방법이 이진 탐색이다. 이진 탐색을 적용하려면 탐색하기 전에 배열이 반드시 정렬되어 있어야 한다. 따라서 이진 탐색은 데이터의 삽입이나 삭제가 빈번할 시에 적합하지 않고, 주로 고정된 데이터에 대한 탐색에 적합하다.

```c

search_binary( list, low, high ) :

  middle <- low에서 high사이의 중간 위치 
  if( 탐색값 == list[middle] )
      return middle;
  else if ( 탐색값 < list[middle] ) 
      return list[0] 부터 list[middle - 1]에서의 탐색;
  else if( 탐색값 > list[middle] )
      return list[middle + 1]부터 list[high] 에서의 탐색;     

```

### 이진 탐색 구현(순환)

```c

int search_binary(int key, int low, int high){
  int middle;

  if ( low <= high ){
    middle = ( low + high ) / 2;
    if ( key == list[middle] )
      return middle;
    else if ( key < list[middle] )
      return search_binary(key, low, middle - 1);
    else 
      return search_binary(key, middle + 1, high);
  }
  return -1;
}

```

### 이진 탐색 구현(반복)

```c

int search_binary2(int key, int low, int high ){
  int middle;

  while (low <= high ) {
    middle = ( low + high ) / 2;
    if ( key == list[middle] )
      return middle;
    else if ( key > list[middle] ) 
      low = middle + 1;
    else 
      high = middle - 1;
  }
  return -1;
}

```

### 정렬된 배열에서의 색인 순차 탐색

색인 순차 탐색 방법은 인덱스라 불리는 테이블을 사용하여 탐색의 효율을 높이는 방법이다. 인덱스 테이블은 주 자료 리스트에서 일정 간격으로 발췌한 자료를 가지고 있다.

```c

#define INDEX_SIZE 256
typedef struct {
  int key;
  int index;
} itable;
itable index_list[INDEX_SIZE];

// INDEX_SIZE는 인덱스 테이블의 크기, n은 전체 데이터의 수 
int search_index(int key, int n ){
  int i, low, high;

  // 키 값이 리스트 범위 내의 값이 아니면 탐색 종료 
  if( key < list[0] || key > list[n - 1] ){
    return -1;
  }

  // 인덱스 테이블을 조사하여 해당키의 구간 결정 
  for ( i = 0 ; i < INDEX_SIZE ; i ++ ) {
    if(index_list[i].key <= key && index_list[i + 1].key > key )
      break;
  }

  if ( i == INDEX_SIZE ) {
    low = index_list[i - 1].index;
    high = n;
  }elseP{
    low = index_list[i].index;
    high = index_list[i + 1].index;
  }
  // 예상되는 범위만 순차 탐색 
  return seq_search(key, low, high);
}
           
```

색인 순차 탐색 알고리즘의 탐색 성능은 인덱스 테이블의 크기에 자우된다. 인덱스 테이블의 크기를 줄이면 주 자료 리스트에서의 탐색 시간을 줄이고, 인덱스 테이블의 크기를 크게 하면 인덱스 테이블의 탐색 시간을 증가시킨다. 인덱스 테이블의 크기를 m이라 하고 주자료 리스트의 크기를 n이라 하면 색인 순차 탐색의 복잡도는 O(m + n/m) 와 같다.


### 보간탐색 

보간 탐색은 사전이나 전화번호부를 탐색하는 방법과 같이 탐색 키가 존재할 위치를 예측하여 탐색하는 방법이다. 이는 우리가 사전을 찾을 때 'ㅎ'으로 시작하는 단어는 사전의 뒷부분에서 찾고 'ㄱ'으로 시작하는 단어는 앞부분에서 찾는 것과 같은 원리다. 보간 탐색은 이진 탐색과 유사하나 리스트를 반으로 분할하지 않고 불균등하게 분할하여 탐색한다.
이진 탐색에서 탐색 위치는 항상 ( low + high ) / 2 이나, 보간 탐색에서는 찾고자하는 키값과 현재의 low, high 위치의 값을 고려하여 다음과 같이 다음 탐색 위치를 결정한다.

![](https://keepinmindsh.github.io/lines/assets/img/InterpolationSearch.jpeg){: .align-center}

```c

int interpol_search(int key, int n){
  int low, high, j;

  low = 0;
  high = n - 1;
  while((list[high] >= key) && ( key > list[low])) {
    j = ((float)(key - list[key]) / ( list[high] - list[low]) * ( high - low)) + low;
    if ( key > list[j] ) low = j + 1;
    else if ( key < list[j]) high = j - 1;
    else return -1;
  }
  if ( list[low] == key ) return (low); // 탐색 성공
  else return -1; // 탐색 실패 
}

```

### 이진 탐색 트리 

이진 탐색과 이진 탐색 트리와의 차이점을 살펴보자. 이진 탐색과 이진 탐색 트리는 근본적으로 같은 원리에 의한 구조이다. 히자만 이진 탐색은 자료들이 배열에 저장되어 있으므로 삽입과 삭제가 상당히 힘들다. 즉 자료를 삽입하고 삭제할 때마다 앞뒤의 원소들을 이동시켜야 한다. 반면에 이진 탐색 트리는 비교적 빠른 시간안에 삽입과 삭제를 끝마칠 수 있는 구조로 되어 있다. 따라서 삽입과 삭제가 심하지 않는 정적인 자료를 대상으로 탐색이 이루어지는 경우에는 이진 탐색도 무난한 방법이나, 삽입,삭제가 빈번히 이루어진다면 반드시 이진 탐색 트리를 사용해야한다.
이진 탐색 트리는 만약 트리가 균형 트리라면 탐색 연산은 O(log2n)의 시간 복작도를 가지고 있다. 이진트리에서의 삽입, 삭제 연산을 다루었지만 이들 연산들은 이진 탐색 트리를 유지시키기는 하지만 균형 트리를 보장하지는 않는다. 만약 이진 탐색 트리가 균형 트리가 아닐 경우에는 탐색의 시간 복잡도가 O(n)으로 높아지게 된다. 만약 균형트리가 아닌 경사트리일 경우, 비교 횟수는 경사트리로 구성된 모든 노드의 횟수만큼 균일하게 탐색되기 때문에 시간 복잡도면에서 좋지 않다. 따라서 이진 탐색 트리에서는 균형을 유지하는 것이 무엇보다 중요하다.

### AVL 트리

AVL 트리는 Adelson-Velskii와 Landis에 의해 1962년에 제안된 트리로서 각 노드에서 왼쪽 서브 트리의 높이가 오른쪽 서브 트리의 높이 차이가 1이하인 이즌 트리를 말한다. AVL 트리는 트리가 비균형 상태로 되면 스스로 노드을을 재배치하여 균형상태로 만든다. 따라서 AVL 트리는 균형 트리가 항상 보장되기 때문에 탐색이 O(logn)시간 안에 끝나게 된다. 또한 삽입과 삭제 연상도 O(logn)시간 안에 할 수 있다.  

AVL 트리도 탐색에서는 일반적인 이진 탐색트리와 동일하다.  

균형을 이룬 이진 탐색트리에서 균형 상태가 깨지는 것은 삽입 연산과 삭제 연산 사이다. 삽입 연산 시에는 삽입되는 위치에 루트 까지의 경로에 있는 조상 노드들의 균형 이수에 영향을 줄 수 있다. 따라서 즉 새로운 노드의 삽입 후에 불균형 상태로 벼한 가장 가까운 조상 노드, 즉 균형 인수가 +-2가 된 가장 가까운 조상 노드의 서브 트리들에 대하여 다시 균형을 잡아야 한다.
AVL 트리에 새로운 노드를 추가하면 균형이 깨어질 수 있다. 이때는 트리를 부분적으로 회전화여 균형 트리로 되돌려야 한다. 큔형이 깨지는 경우에는 아래의 4가지가 있다.  

```c

#include <stdio.h>
#include <stdlib.h>

typedef struct AVLNode {
  int key;
  struct AVLNode *left;
  struct AVLNode *right;
}

```
- LL 타입
노드 Y의 왼쪽 자식의 왼쪽에 노드가 추가됨으로 해서 발생한다. 노드들을 오른쪽을 회전시키면 된다.
- RR 타입
노드 Y의 오른쪽 자식의 오른쪽에 노드가 차됨으로 해서 발생한다. 노드들을 왼쪽을 회전시키면 된다.
- RL 타입
노드 Y의 오른쪽 자식의 왼쪽에 노드가 추가됨으로 해서 발생한다. RL 타입은 균형 트리를 만들 기 위하여 2번의 회전이 필요하다.
- LR 타입
노드 A의 왼쪽 자식의 왼쪽에 노드가 추가됨으로 해서 발생한다. LR 타입도 균형 트리를 만들기 위하여 2번의 회전이 필요하다.

```c

// 오른쪽으로 회전시키는 함수 
AVLNode *rotate_right(AVLNode *prejent){
  AVLNode* child = parent -> left;
  parent -> left = child -> right;
  child -> right = parent;
}

// 왼쪽으로 회전시키는 함수 
AVLNode *rotate_left(AVLNode *parent){
  AVLNode *child = parent -> right;
  parent -> right = child -> left;

  // 새로운 루트 반환 
  return child;
}

// 트리의 높이를 반환
int get_height(AVLNode *node){
  int hegiht = 0;

  if ( node != NULL )
    height = 1 + max(get_height(node -> left),
                    get_height(node -> right));

  return height;
} 

// 노드의 균형 인수를 반환 
int get_balance(AVLNode* node){
  if ( node == NULL ) return 0;

  return get_hegiht(node -> left ) - get_hegiht(node -> right);
}

// AVL 트리에 새로운 노드 추가 함수 
// 새로운 루트를 반환한다. 
AVLNode* insert(AVLNode* node, int key){
  // 이진 탐색 트리의 노드 추가 수행 
  if ( node == NULL )
    return (create_node(key));

  if( key < node -> key)
    node -> left = insert(node -> left, key);
  else if ( key > node -> key)
    node -> right = insert(node -> right, key);
  else // 동일한 키는 허용되지 않음 
    retu4n node;

  // 노드 들의 균형 인수 재계산 
  int balance = get_balance(node);

  // LL 타입 
  if(balance > 1 && key < node -> left -> key)
    return rotate_right(node);

  // RR 타입 
  if(balance < -1 && key > node -> right -> key )
    return rotate_left(node);

  // LR 타입 
  if (balance > 1 && key > node -> left -> key){
    node -> left = rotate_left(node -> left);
    return rotate_right(node);
  }

  // RL 타입 
  if ( balance < -1 && key &`lt; node -> right -> key ){
      node -> right = rotate_right(node -> right);
      return rotate_left(node);
  }

  return node;
}

void preorder(AVLNode *root){
  if ( root != NULL ){
    printf("[%d] ", root -> key);
    preorder(root -> left);
    preorder(root -> right);
  }
}

int main(void){
  AVLNode *root = NULL;

  // 예제 트리 구축 
  root = insert(root, 10);
  root = insert(root, 20);
  root = insert(root, 30);
  root = insert(root, 40);
  root = insert(root, 50);
  root = insert(root, 29);

  printf("전위 순회 결과 \n");
  preorder(root);

  return 0;
}

```

### 2-3 트리

2-3트리는 차수가 2 또는 3인 노드를 가지는 트리로서 삽입이나 삭제 알고리즘이 AVL 트리보다 간단하다. 2-3 트리는 하나의 노드가 두 개 또는 세 개의 자식 노드를 가진다. 차수가 2인 노드를 2-노드라고 하며 2-노드는 일반 이진 탐색 트리 처럼 하나의 데이터 k1과 두 개의 자식 노드를 가진다. 차수가 3인 노드를 3-노드는 3개의 데이터 k1, k2 와 3개의 자식 노드를 가진다. 왼쪽 서브 트리에 있는 데이터들은 모두 k1보다 작은 값이다. 중간 서브 트리에 있는 값들은 모두 k1보다 크고 k2보다 작다. 오른쪽에 있는 데이터들은 모두 k2보다 크다.  


2-3 트리의 탐색 연산은 이진 탐색 트리의 알고리즘을 조금만 확장하면 된다. 노드가 2-노드이냐 3-노드이냐에 따라서 탐색을 진행하면 된다.  

```c

Tree23Node *tree23_seach(Tree23Node* root, int key){
  if( root == NULL ){
    return FALSE;
  }else if ( key  == root -> data) {
    return TRUE;
  }else if ( root -> type == TWO_NODE ) {
    if( key < root -> key ){
      return tree23_search(root -> left, key);
    }else{
      return tree23_search(root -> right, key);
    }
  }else{
    if ( key < root -> key1 ){
      return tree23_search(root -> left, key);
    }else if ( key > root -> key2){
      return tree23_search(root -> right, key);
    }else{
      return tree23_search(root -> middle, key);
    }
  }
}

```

2-3 트리의 노드는 3개의 데이터 값을 저장할 수 있다. 2-3 트리에 데이터 추가시에 노드에 추가할 수 있을 때까지 데이터는 추가되고 더 이상 저장할 장소가 없는 경우에는 노드를 분리하게 된다.
- 단말 노드를 분리하는 경우
- 비단말 노드를 분리하는 경우
- 루트 노드를 분리하는 경우

### 2-3-4 트리

2-3-4 하나의 노드가 4개의 자식까지 가질 수 있도록 2-3 트리를 확장한 것이다. 4개의 자식을 가질 수 있는 노드는 4-노드라고 불리우며 3개의 데이터를 가질 수 있다. 4-노드의 3개의 데이터를 각각 small, middle, large 라고 하면 4-노드의 서브트리에는 그림과 같은 범위에 속하는 데이터들이 들어가게 된다. 2-3-4 트리를 탐색하는 것은 2-3 트리의 탐색 알고리즘에 4-노드를 처리하는 부분만 추가되면 된다.  

2-3-4 트리에서 키를 삽입해야할 단말 노드가 만약 2-노드 또는 3-노드이면 간단하게 삽입만 하면 된다. 문제는 삽입해야할 단말 노드가 4-노드이면 후진 분할이 일어나게 된다. 따라서 20304 노드에서 후진 분할 연산을 방지하기 위하여 삽입 노드를 찾는 순휘 시에 4-노드를 만다면 미리 분할을 수행한다. 따라서 미리 분할을 수행하였으므로 후진 분할을 할 필요가 없다. 이는 2-3 트리와 비교된다. 2-3 트리는 삽입 또는 삭제를 위한 순회와 분할과 합병의 영향으로 인한 순회가 필요하다. 따라서 2-3 트리에 비하여 2-3-4 트리의 장점은 루트에서 단말 노드로 한번만 이동하면서 삽입이나 삭제가 가능하다는 것이다.  