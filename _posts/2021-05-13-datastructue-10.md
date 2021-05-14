---
title:  "그래프 I"
excerpt: "그래프 I에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-13T22:50:00-05:00
layout: single
---

> 원래 대부분의 사람들이 속마음을 숨기고 산다.  
> 모든걸 꺼내 놓기엔 내가 너무 외로워질 것 같아서 

***

그래프는 객체 사이의 연결관계를 표현할 수 있는 자료 구조다. 그래프의 대표적인 예는 지도이다. 지하철 노선도는 여러 개의 역들이 어떻게 연결되었는지를 보여준다. 지도를 그래프로 표현하면 지하철의 특정한 역에서 다른 역으로 가는 최단 경로를 쉽게 프로그래밍 해서 찾을 수 있다. 또한 전기 소자를 그래프로 표현하기 뒤면 전기 회로의 소자들이 어떻게 연결되어 있는지를 표현해야 회로가 제대로 동작하는지 분석할 수 있으며, 운영 체제에서는 프로세스와 자원들이 어떻게 연관되는지를 그래프로 분석하여 시스템의 효율이나 교착 상태 유무 등을 알아낼 수 있다.  
 
이러한 많은 문제들은 공통적으로 도시, 소자, 자원, 프로젝트 등의 객체들이 서로 연결되어 있는 구조로 표현가능하다. 그래프는 이러한 많은 문제들을 표현할 수 있는 훌륭한 논리적 도구이다. 우리가 여태 까지 배워온 선형 리스트나 트리의 구조로는 위와 같은 복잡한 문제들을 표현할 수 없다. 그래프 구조는 인접 행렬이나 인접 리스트로 메모리에 표현되고 처리될 수 있으므로 광벙위한 분야의 다양한 문제들을 그래프로 표현하여 해결할 수 있다.

![](https://keepinmindsh.github.io/lines/assets/img/eulerianTour.png){: .align-center} 

수학자 오일러는 Konigsberg의 다리 문제를 해결하기 위하여 그래프를 처음으로 사용하였다. Konigs시의 한 가운데는 Pregen 강이 흐르고 있고 여기에는 7개의 다리가 있다. 'Konigsbergs의 다리' 문제란 "임의의 지역에서 출발하여 모든 다리를 단 한번만 건너서 처음 출발했던 지역으로 돌아올 수 있는가"이다. 많은 사람들이 이 문제의 답을 찾기 위해 노력을 했다. 여러분도 한번 시도해보면 알 수 있지만 그런 방법은 없다는 것이 정답니다. 오일러는 어떤 한 지역에서 시작하여 모든 다리를 한 번씩만 지나서 처음 출발점으로 되돌아오려면 각 지역에 연결된 다리의 개수가 모두 짝수이어야 함을 증명하였다.
특정 지역은 정점(node)로, 다리는 간선(edge)로 표현하여 그래프 문제로 변경하였다. 오일러는 이러한 그래프에 존재하는 모든 간선을 한번만 통과하면서 처음 정점으로돌아오는 경로를 오일러 경로라고 정의하고, 그래프의 모든 정점에 연결된 간선의 개수가 짝수일 때만 오일러 경로가 존재한다는 오일러의 정의를 증명하였다.

# 그래프로 표현할 수 있는 것들

- 도로 - 도로의 교차점과 일방통행길 등을 그래프로 효과적으로 표현할 수 있다.
- 미로 - 미로도 그래프를 이용하여 효과적으로 표현이 가능하다.
- 대학교에서 전공 과목을 수강하기 위해서는 미리 들어야 하는 선수과목들이 있다. 그래프는 이러한 선수과목 관계를 효과적으로 표현할 수 있다.

# 그래프의 정의와 용어

그래프는 정점과 간선들의 유한 집합이라고 할 수 있다. 수학적으로는 G = ( V, E)와 같이 표시한다. 여기서, V(G)는 그래프 G의 정점들의 집합을, E(G)는 그래프 G의 간선들의 집합을 의미한다. 정점은 여러 가지 특성을 가질 수 있는 객체를 의미하고, 간선은 이러한 정점들 간의 관계를 의미한다. 정점은 노드라고도 부리며, 간선은 링크라고도 불린다. 이 책에서는 '정점'과 '간선'이라는 용어로 통일해서 사용하고자 한다.

### 그래프 ( 정점, 간선 )

V(G1) = { 0, 1, 2, 3 }  
E(G1) = { (0, 1), (0, 2), (0, 3), (1, 2) }  

![](https://keepinmindsh.github.io/lines/assets/img/vertextandedge.png){: .align-center} 

### 무 방향 그래프와 방향 그래프

- 무방향 그래프
무방향 그래프의 간선은 간선을 통해서 양방향으로 갈 수 있음을 나타내며 점정 A와 정점 B 를 연결하는 간선은 ( A, B) 와 같이 정점의 쌍으로 표현한다.
V(G1) = { 0, 1, 2, 3 }
E(G1) = { (0, 1), (0, 2), (0, 3), (1, 2) }

![](https://keepinmindsh.github.io/lines/assets/img/vertextandedge_01.png){: .align-center} 

- 방향 그래프
방향 그래프는 간선에 양방향이 존재하는 그래프로서 도로의 일방통행길처럼 간선을 통하여 한쪽 방향으로만 갈 수 있음을 나타낸다. 정점 A에서 정점 B로만 갈 수 있는 간선은 <A, B> 로 표시한다. 방향 그래프에서 < A, B>와 < B, A>는 서로 다른 간선이다.
V(G1) = { 0, 1, 2, 3 }
E(G1) = { <0, 1>, <0, 2>, <0, 3>, <1, 2> }

![](https://keepinmindsh.github.io/lines/assets/img/vertextandedge_03.png){: .align-center} 

- 네크워크 

간선에 가중치를 할당하게 되면, 간선의 역할이 두 정점 간에 연결 유무 뿐만 아니라 연결 강도까지 나타낼 수 있으므로 보다 복잡한 관계를 표현할 수 있게 된다. 이렇게 간선에 비용이나 가중치가 할당된 그래프를 가중치 그래프 또는 네트워크라고 하며 좌측의 이미지와 같이 나타낸다.

![](https://keepinmindsh.github.io/lines/assets/img/vertextandedge3.png){: .align-center} 

- 부분 그래프 

어떤 그래프의 정점의 일부와 간선의 일부로 이루어진 그래프를 부분 그래프라 한다. 그래프 G의 부분 그래프 S는 아래와 같은 그래프이다.

![](https://keepinmindsh.github.io/lines/assets/img/vertextandedge4.png){: .align-center} 

- 연결 그래프

그래프에서 인접 정점란 간선에 의해 직접 연결된 정점을 뜻한다. 그래스에서 정점 0의 인접 정점은 정점 1, 정점 2, 정점 3이다. 무방향 그래프에서 정점의 차수는 그 정점에 인접한 정점의 수를 말한다.

- 경로

무방향 그래프에서 정점 s로부터 정점 e까지의 경로는 정점의 나열 s, v1, v2, ... vk, e 로서 , 나열된 정점들 간에는 반드시 간선 (s, v1), ( v1, v2), ... , ( vk, e)가 존재해야 한다. 만약 방향 그래프라면 < s, v1> , < v1, v2 >, ... , < vk, e > 가 있어야 한다. 아래의 그래프에서 0, 1, 2, 3은 경로지만 0, 1, 3, 2는 경로가 아니다. 왜냐하면 간선 ( 1 , 3 ) 이 존재하지 않기 때문이다.

- 싸이클 

경로 중에서 반복되는 간선이 없을 경우에 이러한 경로는 단순 경로라 한다. 만약에 단순 경로의 시작 정점과 종료 정점이 동일하다면 이라한 경로를 싸이클이라 한다.

- 연결 그래프 

무방향 그래프 G에 있는 모든 정점 쌍에 대하여 항상 경로가 존재한다면 G는 연결되었다고 하며, 이러한 무방향 그래프 G 를 연결 그래프라 부른다. 그렇지 않은 그래프는 비 연결 그래프라고 한다. 트리는 그래프의 특수한 형태로서 사이클을 가지지 않는 연결 그래프이다.

- 완전 그래프 

그래프에 속해 있는 모든 정점이 서로 연결되어 있는 그래프를 완전 그래프라고 한다. 무방향 완전 그래프의 정점수를 n 이라고 하면, 하나의 정점은 n - 1 개의 다른 정점으로 연결되므로 간선의 수는 n * ( n - 1 ) / 2 가 된다. 만약 완전 그래프에서 n = 4라면 간선의 수는 ( 4 * 3 ) / 2 = 6 이다.


# 그래프 추상 데이터 타입 ( ADT )

```c

객체 : 정점의 집합과 간선의 집합
연산 :
  create_graph() ::= 그래프를 생성한다. 
  init(g) ::= 그래프 g를 초기화한다. 
  insert_vertex(g,v) ::= 그래프 g에 정점 v를 삽입한다. 
  insert_edge(g,u,v) ::= 그래프 g에 간선 ( u , v )를 삽입한다. 
  delete_vertex(g,v) ::= 그래프 g의 정점 v를 삭제한다. 
  delete_edge(g,u,v) ::= 그래프 g의 간선 ( u , v )를 삭제한다. 
  is_empty(g) ::= 그래프 g가 공백 상태인지 확인한다. 
  adjacent(v) ::= 정점 v에 인접한 정점들의 리스트를 반환한다. 
  destroy_graph(g) ::= 그래프 g를 제거한다. 

```

# 그래프 표현 방법 

- 인접 행렬 : 2차원 배열을 사용하여 그래프를 표현한다.
- 인접 리스트 : 연결 리스트를 사용하는 그래프를 표현한다.

### 인접행렬 

그래프의 정점 수가 n 이라면 n * n 의 2차원 배열인 인접 행렬 M 의 각 원소를 다음 규칙에 의해 할당함으로써 그래프를 메모리에 표현할 수 있다.

```c

if(간선 ( i , j)가 그래프에 존재 ) M[i][j] = 1;
otherwise                     M[i][j] = 0;

```

우리가 다루고 있는 그래프에서는 자체 간선을 허용하지 않으므로 인접 행렬의 대각선 성분은 모두 0으로 표시된다. 무방향 그래프의 인접 행렬은 대칭 행렬이 된다. 이는 무방향 그래프의 간선(i,j)는 정점 i에서 정점 j로의 연결 뿐만 아니라 정점 j에서 정점 i로의 연결을 동시에 의미하기 때문이다. 따라서 무방향 그래프의 경우, 배열의 상위 삼각이나 하위 삼각만 저장하면 메모리를 절약할 수 있다. 무 방향 그래프의 경우 인접 행렬에서 행렬의 값이 대칭으로 표시되지만, 방향 그래프는 대칭으고 값을 표시할 수 없다. 따라서 무방향 그래프에서 배열의 상위 삼각또는 하위삼각의 값만 저장하면 되기 때문에 메모리 효율이 더 좋게 처리할 수 있다.

![](https://keepinmindsh.github.io/lines/assets/img/GraphSample.png){: .align-center} 

n 개의 정점을 가지는 그래프를 인접 행렬로 표현하기 위해서는 간선의 수와 무관하게 항상 n^2 개의 메모리 공간이 필요하다. 이에 따라 인접 행렬은 G1과 같이 그래프에 간선이 많이 존재하는 밀집 그래프를 표형하는 경우에는 적합하나, G2와 같이 그래프 내에 적은 숫자의 간선만을 가지는 희소 그래프의 경우에는 메모리의 낭비가 크므로 적합하지 않다.
인접 행렬을 이용하면 두 정점을 연결하는 간선의 존재 여부를 O(1) 시간 안에 즉시 알 수 있는 장점이 있다. 즉 정점 u 와 정점 v를 연결하는 정점이 있는지를 알려면 M[u][v] 의 값을 조사하면 바로 알 수 있다. 또한 정점의 차수는 인접 행렬의 행이나 열을 조사하면 알수 있으므로 O(n) 연산에 의해 알 수 있다.

### 인접 행렬을 이용한 추상 데이터 타입의 구현

```c

#include <stdio.h>
#include <stdlib.h>

#define MAX_VERTICES 50 
typedef struct GraphType {
  int n ;
  int adj_mat[MAX_VERTICES][MAX_VERTICES];
} GraphType;


// 그래프 초기화 
void init(GraphType* g) {
  int r, c;
  g -> n = 0;
  for ( r = 0; r < MAX_VERTICES ;  r ++ ){
    for ( c = 0; c < MAX_VERTICES ; c ++ ) {
      g -> adj_mat[r][c] = 0;
    }
  }
}

// 정점 삽입 연산
void init(GraphType* g) {
  if (((g -> n) + 1) > MAX_VERTICES ){
    fprintf(stderr, "그래프 : 정점의 개수 초과 ");
    return;
  }
  g -> n++;
}

// 간선 삽입 연산 
void insert_edge(GraphType* g, int start, int end) {
  if ( start <= g -> n || end >= g -> n){
    fprintf(stderr, "그래프 : 정점 번호 오류");
    return;
  }
  g -> adj_mat[start][end] = 1;
  g -> adj_mat[end][start] = 1;
}

// 인접 행렬 출력 함수 
void print_adj_mat(GraphType* g){
  for( int i = 0; i < g -> n ; i ++ ){
    for( int j = 0; j < g -> n ; j ++ ){
      printf("%2d", g -> adj_mat[i][j]);
    } 
    printf("\n");
  }
}

void main(){
  GraphType *g;
  g = (GraphType *)malloc(sizeof(GraphType));
  init(g);
  for( int 1 = 0; i < 4; i ++){
    insert_vertex(g, i);
  }
  insert_edge( g, 0, 1);
  insert_edge( g, 0, 2);
  insert_edge( g, 0, 3);
  insert_edge( g, 1, 2);
  insert_edge( g, 2, 3);
  
  print_adj_mat(g);

  free(g);
}

```
### 인접리스트 

인접 리스트는 그래프를 표현함에 있어 각각의 정점에 인접한 정점들을 연결 리스트로 표시한 것이다. 각 연결 리스트의 노드들은 인접 정점을 저장하게 된다. 각 연결 리스트들은 헤러 노드를 가지고 있고 이 헤더 노드들은 인접 정점을 저장하게 된다.

![](https://keepinmindsh.github.io/lines/assets/img/Graph01.png){: .align-center} 

정점의 수가 n 개이고 간선의 수가 e개인 무방향 그래프를 표시하기 위해서는 n 개의 연결 리스트가 필요하고, n개의 헤더 노드와 2e 개의 노드가 필요하다. 따라서 인접 행렬 표현은 간선의 개수가 적은 희소 그래프의 표현에 적합하다. 그래프에 간선의 존재여부나 정점 i의 차수를 알기 위해서는 인접 리스트 에서의 정점 i의 연결 리스트를 탐색해야 하므로 연결 리스트에 있는 노드의 수 만큼, 즉 정점 차수 만큼의 시간이 필요하다. 즉 n 개의 정점과 e개의 간선을 가진 그래프에서 전체 간선의 수를 알아내려면 헤더 노드를 포함하여 모든 인접 리스트를 조사해야하므로 O(n + e)의 연산이 요구된다.

```c

#include <stdio.h>
#include <stdlib.h>

#define MAX_VERTICES 50

typedef struct GraphNode {
  int vertex;
  struct GraphNode* link;
} GraphNode;

typedef struct GraphType {
  int n; // 정점의 개수 
  GraphNode* adj_list[MAX_VERTICES];
} GraphType;

// 그래프 초기화 
void init(GraphType* g){
  int v;
  g -> n = 0;
  for ( v = 0; v < MAX_VERTICES; v ++){
    g -> adj_list[v] = NULL;
  }
}

// 정점 삽입 연산 
void insert_vertex(GraphType* g, int v){
  if ((g -> n) + 1) > MAX_VERTICES ) {
    fprintf(stderr, "그래프 : 정점의 개수 초과");
    return;
  }
  g -> n ++;
}

// 간선 삽입 연산, v를 u의 인접 리스트에 삽입한다. 
// 정점  u에 간선 ( u , v )를 삽입하는 연산은 정점 u의 인접 리스트에 간선을 나타내는 노드를 하나 생성하여 삽입하면 된다. 위치는 상관 없으므로, 삽입을 쉽게 하기 위하여 연결 리스트의 맨 처음에 삽입한다. 
void insert_edge(GraphType* g, int u, int v){
  GraphNode* node;
  if ( u >= g -> n || v >= g -> n ) {
    fprintf(stderr, "그래프: 정점 번호 오류 ");
    return;
  }
  node = (GraphNode*)malloc(sizeof(GraphNode));
  node -> vertex = v;
  node -> link = g -> adj_list[u];
  g -> adj_list[u] = node;
}

void print_adj_list(GraphType* g){
  for( int i = 0 ; i < g -> n ; i++){
    GraphNode* p = g -> adj_list[i];
    printf("정점 %d의 인접 리스트 ", i);
    while( p != NULL ){
      printf("-> %d", p -> vertex);
      p = p -> link;
    } 
    printf("\n");
  }
}

int main() {
  GraphType *g;
  g = (GraphType *)malloc(sizeof(GraphType));
  init(g);
  for(int i = 0; i < 4 ; i ++)
    insert_vertex(g, i);
  
  insert_edge(g, 0, 1);
  insert_edge(g, 1, 0);
  insert_edge(g, 0, 2);
  insert_edge(g, 2, 0);
  insert_edge(g, 0, 3);
  insert_edge(g, 3, 0);
  insert_edge(g, 1, 2);
  insert_edge(g, 2, 1);
  insert_edge(g, 2, 3);
  insert_edge(g, 3, 2);
  print_adj_list(g);
  free(g);
  return 0;
}

```

# 그래프 탐색

그래프 탐색은 가장 기본적인 연산으로서 하나의 정점으로부터 시작하여 차례대로 모든 정점들을 한 번씩 방문하는 것이다. 그래프 탐색은 아주 중요하다. 많은 문제들이 단순히 그래프의 노드를 탐색하는 것으로 해결된다.  

**예**
- 도시를 연결하는 그래프가 있을 때, 특정 도시에서 다른 도시로 갈 수 있는지 없는지를 그래프를 특정노드에서 탐색하여 보면 알 수 있다.
- 전자회로에서 특정 단자와 단자가 서로 연결되어 있는지 연결되어 있지 않느지를 탐색을 통하여 알 수 있다.

**탐색 방식**
- 깊이 우선 탐색 ( DFS : depth first search ) : 트리를 생각하면 이해하기 쉽다. 트리를 탐색할 때 시작 정점에서 한 방향으로 계속 가다가 더이상 갈 수 없게 되면 가장 가까운 갈림길로 돌아와서 다른 방향으로 다시 탐색을 진행하는 방법과 유사
- 너비 우선 탐색 ( DFS : breath first search ) : 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져있는 정점을 나중에 방문하는 순회 방법이다.

### 깊이 우선 탐색 ( 인접 행렬 버전 )

```c

#include <stdio.h>
#include <stdlib.h> 

#define TRUE 1
#define FALSE 0
#define MAX_VERTICES 50

typedef struct GraphType {
  int n;
  int adj_map[MAX_VERTICES][MAX_VERTICES];
} GraphType;

int visited[MAX_VERTICES];

// 그래프 초기화 
void init(GraphType* g){
  int r, c;
  g -> n = 0;
  for ( r = 0; r < MAX_VERTICES; r ++)
    for ( c = 0; c < MAX_VERTICES; c ++)
      g -> adj_mat[r][c] = 0;
}

// 정점 삽입 연산 
void insert_vertex(GraphType* g, int start, int end){
  if(((g -> n ) + 1 ) > MAX_VERTICES){
    fprintf(stderr, "그래프: 정점의 개수 초과");
    return;
  } 
  g -> n ++;
}

// 간선 삽입 연산 
void insert_edge(GraphType* g, int start, int end){
  if( start >= g -> n ||  end >= g -> n ){
    fprintf(stderr, "그래프 : 정점 번호 오류");
    return;
  }
  g -> adj_mat[start][end] = 1;
  g -> adj_mat[end][start] = 1;
}

// 인접 행렬로 표현된 그래프에 대한 깊이 우선 탐색 
void dfs_mat(GraphType* g, int v){
  int w;
  visited[v] = TRUE; // 정점 v의 방문 표시 
  printf("정점 %d ->", v); // 방문한 정점 출력
  for ( w = 0; w < g -> n ) // 인접 정점 탐색
    if ( g -> adj_mat[v][w] && !visited[w] )
      dfs_mat(g,w); // 정점 w에서 DFS 새로 시작 
}

int main(void){
  GraphType *g;
  g = (GraphType *)malloc((sizeof(GraphType)));
  init(g);
  for( int i = 0; i < 4 ; i ++ ){
    insert_vertex(g,i);
  }
  insert_vertex(g,0,1);
  insert_vertex(g,0,2);
  insert_vertex(g,0,3);
  insert_vertex(g,1,2);
  insert_vertex(g,2,3);

  printf("깊이 우선 탐색\n");
  dfs_mat(g,0);
  printf("\n");
  free(g);
  return 0;
}

```

###### 깊이 우선 탐색의 구현

그래프가 인접리스트로 표현되었을 경우의 깊이 우선 탐색 프로그램이다. 인접 리스트는 다수의 연결 리스트로 구성되는데, 각 연결 리스트의 노드는 데이터 필드와 링크 필드로 이루어지는데, 데이터 필드에는 인접 정점의 번호가 저장되고 링크 필드에는 다음 인접 정점을 가리키는 포인터가 저장된다.


```c

int visited[MAX_VERTICES];

// 인접 리스트로 표현된 그래프에 대한 깊이 우선 탐색 
void dfs_list(GraphType* g, int v){
  GraphNode* w;
  visited[v] = TRUE;
  printf("정점 %d -> ", v);                      // 정점 V의 방문 표시 
  for ( w = g -> adj_list[v]; w; w = w -> link){ // 인접 정점 탐색 
    if ( !visited[w -> vertex])
      dfs_list(g, w -> vertex); // 정점 w에서 DFS 새로 시작 
  }
}

```

###### 명시적인 스택을 이용한 깊이 우선 탐색의 구현

```c

DFS-iterative(G, v):

    스택 S를 생성한다. 
    S.push(v)
    while (not is_empty(S)) do 
        v = S.pop();
        if ( v가 방문되지 않았으면 )
            v를 방문되었다고 표시 
            for all u ∈ ( v에 인접한 정점 ) do 
                if ( u가 아직 방문되지 않았으면 )
                    S.push(u)

  스택을 하나 생성하여서 시작 정점을 스택에 넣는다. 이어서 스택에서 하나의 정점을 꺼내서 탐색을 시작한다. 정점을 
  방문한 후에 정점의 모든 인접 정점들을 스택에 추가한다. 스택에 하나도 남지 않을 때 까지 알고리즘은 계속된다. 

```

깊이 우선 탐색은 그래프의 모든 간선을 조사하므로 정점의 수가 n이고 간선의 수가 e인 그래프인 경우, 그래프가 인접 리스트로 표현되어 있다면 시간 복잡도가 O(n+e) 이고, 인접행렬로 표시되어 있다면 O(n^2)이다. 이는 희소 그래프인 경우 깊이 우선 탐색은 인접 리스트의 사용이 인접 행력보다 시간적으로 유리함을 뜻한다.

### 너비우선탐색

너비 우선 탐색은 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법이다.

```c

breadth_first_search(v):
  
    v를 방문되었다고 표시:
    큐 Q에 정점 v 를 삽입;
    while ( Q가 공백이 아니면 ) do 
      Q에서 정점 w를 삭제;
        for all u ∈ ( w에 인접한 정점 ) do 
          if ( u 가 아직 방문되지 않았으면 )
            then u를 큐에 삽입;
              u를 방문되었다고 표시;

```

##### 너비 우선 탐색의 구현(인접행렬버전)

너비 우선 탐색은 큐를 사용하여야 깊이 우선 탐색보다 코드가 복잡해진다.

```c

#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0
#define MAX_QUEUE_SIZE 10

typedef int element;

typedef struct { // 큐 타입
  element queue[MAX_QUEUE_SIZE];
  int front, rear;
} QueueType;

// 오류 함수 
void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
} 

// 공백 상태 검출 함수 
void queue_init(QueueType *q){
  q -> front = q -> rear = 0;
}

// 공백 상태 검출 함수 
void is_empty(QueueType *q){
  return ( q -> front == q -> rear );
}

// 포화 상태 검출 함수 
void is_full(QueueType *q){
  return ((q -> rear + 1) % MAX_QUEUE_SIZE == q -> front );
}

// 삽입 함수 
void enqueue(QueueType *q, element item){
  if(is_full(q)){
    error("큐가 포화 상태 입니다. ");
  }

  q -> rear = ( q -> rear + 1 ) % MAX_QUEUE_SIZE;
  q -> queue[q -> rear] = item;
}

// 삭제함수 
element dequeue(QueueType *q){
  if(is_empty(q)){
    error("큐가 공백 상태입니다.");
  }
  q -> front = ( q -> front + 1) % MAX_QUEUE_SIZE;
  return q -> queue[q -> front];
}

#define MAX_VERTICES 50
typedef struct GraphType {
  int n;
  int adj_mat[MAX_VERTICES][MAX_VERTICES];
} GraphType;

int visited[MAX_VERTICES];

// 그래프 초기화
void graph_init(GraphType* g){
  int r, c;
  g -> n = 0;
  for ( r = 0; r < MAX_VERTICES ; r ++){
    for ( c = 0; c < MAX_VERTICES ; c ++ ){
      g -> adj_mat[r][c] = 0;
    }
  }
}

// 정점 삽입 연산
void insert_vertex(GraphType* g, int v){
  if(((g -> n) + 1)  > MAX_VERTICES ){
    fprintf(stderr, "그래프: 정점의 개수 초과");
    return;
  }
  g -> n ++;
}

// 간선 삽입 연산 
void insert_edge(GraphType* g, int start, int end){
  if ( start >= g -> n || end >= g -> n ) {
    fprintf(stderr, "그래프: 정점 번호 오류");
    return;
  }
  g -> adj_mat[start][end] = 1;
  g -> adj_mat[end][start] = 1;
}

void bfs_mat(GraphType* g, int v){
  int w;

  QueueType q;

  queue_init(&q);

  visited[v] = TRUE;
  printf("%d 방문 -> ", v);
  enqueue(&q, v);
  while(!is_empty(&q)){
    v = dequeue(&q);
    for ( w = 0; w < g -> n ; w ++){
      if( g -> adj_mat[v][w] && !visited[w]) {
        visited[w] = TRUE;
        printf("%d 방문 ->", w);
        enqueue(&q , w);
      }
    }
  }
}

int main(void){
  GraphType *q;
  g = (GraphType *)malloc(sizeof(GraphType));
  graph_init(g);
  for ( int i = 0 ; i < 6 ; i ++){
    insert_vertex(g, i);
  }
  insert_edge(g, 0, 2); 
  insert_edge(g, 2, 1);
  insert_edge(g, 2, 3);
  insert_edge(g, 0, 4);
  insert_edge(g, 4, 5);
  insert_edge(g, 1, 5);

  printf("너비 우선 탐색\n");
  bfs_mat(g, 0);
  free(g);
  return 0;
}

```

###### 너비 우선 탐색의 구현(인접 리스트 버전)

```c

void bfs_list(GraphType* g, int v){
  GraphNod* w;
  QueueType q;

  init(&q);                                            // 큐 초기화 
  visited[v] = TRUE;                                   // 정점 v 방문 표시 
  printf("%d 방문 -> ", v);
  enqueue(&q, v);                                      // 시작 정점을 큐에 저장 
  while(!is_empty(&q)) {
    v = dequeue(&q);                                   // 큐에 저장된 정점 탐색 
    for ( w = g -> adj_list[v] ; w; w = w -> link ){   // 인접 정점 탐색
      if(!visited[w -> vertex]){                       // 미방문 정점 탐색
        visited[w -> vertex] = TRUE;                   // 방문 표시
        printf("%d 방문 -> ", w -> vertex);
        enqueue(&q , w -> vertex);                     // 정점을 큐에 삽입
      }
    }
  }
}

```