---
title:  "그래프 II"
excerpt: "그래프 II에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-14T22:50:00-05:00
layout: single
---

> 좋은 것만 보고, 좋은 사람만 만나기도 짧은 인생,  
> 보기싫은 건 그냥 지나치는 것도 좋지. 

***

# 신장트리 

신장 트리란 그래프 내의 모든 정점을 포함하는 트리다. 신장 트리는 특수한 형태이므로 모든 정점들이 연결되어 있어야 하고 또한 사이클을 포함해서는 안된다. 따라서 신장 트리는 그래프에 있는 n개의 정점을 정확히 ( n - 1) 개의 간선으로 연결하게 된다. 하나의 그래프에서는 많은 신장 트리가 존재 가능하다.  

신장 트리는 깊이 우선이나 너비 우선 탐색 도중에 사용된 간선만 모으면 만들 수 있다. 신장 트리를 만들려면 깊이 우선이나 너비 우선 탐색때 사용한 간선들을 표시하면 된다.


```c

depth_first_search(v):

    v를 방문되었다고 표시; 
    for all u ∈ ( v에 인접한 정점 ) do 
        if ( u가 아직 방문되지 않았으면 )
            then ( v, u )를 신장 트리 간선이라고 표시;
                depth_first_search(u); 

```

### 최소 비용 신장 트리

통신망 , 도로망, 유통망은 간선에 가중치가 부여된 네크워크로 표현될 수 있다. 가중치는 길이 , 구축 비용, 전송 시간등을 나타낸다. 이러한 도로망, 통신망, 유통망을 가장 적은 비용으로 구축하고자 한다면, 네트워크에 있는 모든 정점들을 가장 적은 수의 간선과 비용으로 연결하는 최소 비용 신장 트리가 필요하게 된다. 최소 비용 신장 트리는 신장 트리 중에서 사용된 간선들의 가중치 합이 최소인 신장 트리를 말한다. 응용의 예
- 도로건설 - 도시들을 모두 연결하면서 도로의 길이가 최소가 되도록 하는 문제
- 전기회로 - 단자들을 모두 연결하면서 전선의 길이가 가장 최소가 되도록 하는 문제
- 통신 - 전화선의 길이가 최소가 되도록 전화 케이블 망을 구성하는 문제
- 배관 - 파이프를 모두 연결하면서 파이프의 총 길이가 최소가 되도록 연결하는 문제

### Kruskal의 MST 알고리즘

Kruskal의 알고리즘은 탐욕적인 방법을 이용한다. 탐욕적인 방법은 알고리즘 설계에서 있어서 중요한 기법 중의 하나이다. 탐욕적인 방법이란 선택할 때마다 그 순간 가장 좋다고 생각되는 것을 선택함으로써 최종적인 해답에 도달하는 방법이다.
탐욕적인 알고리즘에서 순간의 선택은 그 당시에는 최적이다. 하지만 최적이라고 생각했던 지역적인 해답들을 모아서 최종적인 해답을 만들었다고 해서, 그 해담이 반드시 전역적으로 최적이라는 보장은 없다. 따라서 탐욕 적인 방법은 항상 최적의 해답을 주는지를 검증해야 한다. 다행히 Kruskal의 알고리즘은 최적의 해답을 주는 것으로 증명되어 있다. Kruskal의 알고리즘은 최소 비용 신장 트리가 최소 비용의 간선으로 구성됨과 동시에 사이클을 포함하지 않는다는 조건에 근거하여, 각 단계에서 사이클을 이루지 않는 최소 비용 간선을 선택한다. 이러한 과정을 반복함으로써 네트워크의 모든 정점을 최소 비용으로 연결하는 최적 해답을 구할 수 잇다.

```c

// 최소비용 신장 트리를 구하는 Krusal의 알고리즘 
// 입력 : 가중치 그래프 G = ( V , E), n은 노드의 개수 
// 출력 : Et, 최소 비용 신장 트리를 이루는 간선들의 집합 

```

![](https://keepinmindsh.github.io/lines/assets/img/Kruskal.png){: .align-center} 

Kruskal 의 알고리즘을 이용하여 최소 비용 신장 트리를 만드는 과정을 보여준다. 먼저 간선들을 가중치의 오름차순으로 정렬한다.
간선의 양끝 정점이 같은 집합에 속하면 간선을 추가하였을 경우, 사이클이 형성된다.
간선이 서로 다른 집합에 속하는 정점을 연결하면 사이클이 형성되지 않는다.

### union-find 연산

union-find 연산은 Kruskal의 알고리즘에서만 사용되는 것이 아니고 일반적으로 널리 사용된다. union(x,y) 연산은 원소 x 와 원소 y가 속해있는 집합을 입력으로 받아 2개의 집합의 합집합을 만든다. find(x) 연산은 원소 x가 속해있는 집합을 반환한다.
union-find 연산의 구현
집합을 구현하는 데는 여러가지 방법이 있을 수 있다. 즉 비트 벡터, 배열, 연결 리스트를 이용하여 구현될 수 있다. 그러나 가장 효율적인 방법은 트리형태를 사용하는 것이다.
우리는 부모 노드만 알면 되므로 "부모 포인터 표현"을 사용한다. "부모 포인터 표현"이란 각 노드에 대해 그 노드의 부모에 대한 포인터만 저장하는 것이다. 이것은 일반적인 목적에는 부적합하다. 즉 노드의 가장 왼쪽 자식 또는 오른쪽 자식을 찾는 것과 같은 중요한 작업에는 부적절하기 때문이다.

```c

UNION(a, b) :
    root1 = FIND(a);
    root2 = FIND(b);
    if root1 != root2 
        parent[root1] = root2;
    
    FIND(curr);  // curr의 루트를 찾는다. 
    if ( parent[curr] == -1 )
        return curr;
    while( parent[curr] != -1 ) curr = parent[curr];
    return curr;  

int parent[MAX_VERTICES]; // 부모 노드 

void set_init(int n) 
{
  for ( int i = 0; i < n ; i ++){
    parent[i] = -1;
  }
}

// curr 가 속하는 집합을 반환한다. 
int set_find(int curr)
{
  if ( parent[curr] == -1){
    return curr;
  }
  while(parent[curr] != -1 ) curr = parent[curr];
  return curr;
}

// 두개의 원소가 속한 집합을 합친다. 
void set_union(int a, int b){
  int root1 = set_find(a);
  int root2 = set_find(b);
  if ( root1 != root2 ){
    parent[root1] = root2;
  }
}

```

### kruskal 알고리즘 구현

```c

#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0

#define MAX_VERTICES 100
#define INF 1000

void set_init(int n){
  for ( int i = 0; i < n ; i ++ ){
    parent[i] = -1;
  }
}

// curr 가 속하는 집합을 반환한다. 
int set_find(int curr){
  if(parent[curr] == -1){
    return curr;
  }
  while( parent[curr] != 1 ) curr = parent[curr];
  return curr;
}

// 두개의 원소가 속한 집합을 합친다. 
void set_union(int a, int b){
  int root1 = set_find(a);
  int root2 = set_find(b);
  if ( root1 != root2 )
    parent[root1] = root2;
}

struct Edge {
  int start, end, weight;
};

typedef struct GraphType {
  int n;
  struct Edge edges[2 * MAX_VERTICES];
} GraphType;

void graph_init(GraphType* g){
  g -> n = 0;
  for(int i = 0; i < 2 * MAX_VERTICES ; i ++){
    g -> edge[i].start = 0
    g -> edge[i].end = 0;
    g -> edge[i].weight = INF;
  }
}

// 간선 삽입 연산 
void insert_edge(GraphType* g){
  g -> edge[i].start = 0
  g -> edge[i].end = 0;
  g -> edge[i].weight = INF;
  g -> n ++;
}

// qsort()에 사용되는 함수 
int compare(const void* a, const void* b){
  struct Edge* x = (struct Edge*)a;
  struct Edge* y = (struct Edge*)b;
  return ( x -> weight - y -> weight );
}

// kruskal의 최소 비용 신장 트리 프로그램 
void kruskal(GraphType *g){
  int edge_accepted = 0; // 현재까지 선택된 간선의 수 
  int uset, vset;        // 정점 u 와 정점 v 의 집합번호 
  struct Edge e;

  set_init(g -> n); // 집합 초기화 
  qsort(g -> edges, g -> n, sizeof(struct Edge), compare);

  printf("크루스칼 최소 신장 트리 알고리즘 \n");
  int i = 0;
  while ( edge_accepted < ( g -> n - 1)) // 간선의 수 
  {
    e = g -> edges[i];
    uset = set_find(e.start);
    vset = set_find(e.end);
    if(uset != vset){
      printf("간선 (%d,%d) %d 선택 \n", e.start, e.end, e.wegiht );
      edge_accepted ++;
      set_union(uset, vset);
    }
    i ++;
  }
}

int main(void) {
  GraphType *g;

  g = (GraphType *) malloc(sizeof(GraphType));
  graph_init(g);

  insert_edge(g, 0, 1, 29);
  insert_edge(g, 1, 2, 16);
  insert_edge(g, 2, 3, 12);
  insert_edge(g, 3, 4, 22);
  insert_edge(g, 4, 5, 27);
  insert_edge(g, 5, 0, 10);
  insert_edge(g, 6, 1, 15);
  insert_edge(g, 6, 3, 18);
  insert_edge(g, 6, 4, 25);

  kruskal(g);
  free(g);
  return 0;
}

```
###### 시간 복잡도 분석

Kruskal의 알고리즘의 시간 복잡도는 간선들을 정렬하는 시간에 좌우된다. 따라서 효율적인 정렬 알고리즘을 사용한다면 Kruskal의 알고리즘의 시간 복잡도는 |e|log2|e|이다.

### Prim의 MST 알고리즘

Prim의 알고리즘은 시작 정점에서부터 출발하여 신장 트리 집합을 단게적으로 확장해 나가는 방법이다. 시작 단계에서는 시작 정점만이 신장 트리 집합에 포함된다. Prim의 방법은 앞 단계에서 만들어진 신장 트리 집합에, 인접한 정점들 중에서 최저 간선으로 연결된 정점을 선택하여 트리를 확장한다. 이 과정은 트리가 n-1개의 간선을 가질 때까지 계속된다. Kruskal 의 알고리즘과 비교를 해보면 먼저 Kruskal의 알고리즘은 간선을 기반으로 하는 알고리즘인 반면 Prim의 알고리즘은 정점을 기반으로 하는 알고리즘이다. 또한 Kruskal의 알고리즘에서는 이전 단계에서 만들어진 신장 트리와는 상관 없이 무조건 최저 간선만을 선택하는 방법인었던게 반하여 Prim 알고리즘은 이전 단계에서 만들어진 신장 트리를 확장하는 방식이다.

```c

// 최소 비용 신장 트리를 구하는 Prim의 알고리즘 
// 입력 : 네트워크 G = ( V, E ), S 는 시작 정점 
// 출력 : Vt , 최소 비용 신장 트리를 이루는 정점들의 집합 

```

###### Prim의 알고리즘의 구현

```c

// 최소 비용 신장 트리를 구하는 Prim의 알고리즘 
// 입력 : 네트워크 G = ( V , G ) , s 는 시작 정점 
// 출력 : 최소 비용 신장 트리를 이루는 정점들의 집합      

```

정점들이 트리 집합에 추가되면서 distance 값은 변경된다. 다음에 우선 순위 큐 Q 가 하나 필요하다. 배열로 구현할 수도 있고 아니면 히프를 사용하면 보다 효율적인 프로그램이 될 것이다. 우선 순위 큐에 모든 정점을 삽입한다. 이때의 우선 순위는 distance 배열 값이 된다. 다음은 while 루프로 우선순위 큐에서 가장 작은 distnace 값을 가지는 정점을 끄집어 낸다. 바로 이정점이 트리 집합에 추가된다. 여기서는 그냥 화면에 이 정점의 번호를 출력하기로 하는 것으로 만족하자. 다음에는 트리 집합에 새로운 정점 u가 추가되었으므로 u에 인접한 정점 v 들의 distance 값을 변경시켜준다. 즉 기존의 distance[v] 값 보다 간선 ( u , v )의 가중치 값이 적으면 간선 ( u, v) 의 가중치 값으로 dist[v]를 변경시킨다. Q에 있는 모든 정점들이 소진될 때까지 이것을 되풀이 하면 된다. 한번 선택된 정점은 Q에서 삭제되므로 다시 선택되지는 않음을 명심하라. 그리고 트리 집합에 인접하지 않는 정점들의 distance 값은 무한대이므로 역시 선택되지 않을 것이다. 코드를 간단하게 하기 위하여 오류 처리를 생략했음을 유의해야 한다.

```c
#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0
#define MAX_VERTICES 100
#define INF 1000L

typedef struct GraphType {
  int n ;
  int weight[MAX_VERTICES][MAX_VERTICES];
} GraphType;

int selected[MAX_VERTICES];
int distance[MAX_VERTICES];

// 최소 dist[v] 값을 갖는 정점을 반환 
int get_min_vertex(int n){
  int v, i;
  for ( i = 0; i < n; i ++){
    if(!selected[i]){
      v = i;
      break;
    }
  }

  for ( i= 0; i < n ; i ++ ){
    if ( !selected[i] && ( distance[i] < distance[v])) v = i;
  }

  return (v);
}

void prim(GraphType* g, int s){
  int i, u, v;

  for( u = 0; u < g -> n ; u ++){
    distance[u] = INF;
  }
  distance[s] = 0;
  for ( i = 0; i < g -> n ; i ++ ){
    u = get_min_vertex(g -> n);
    selected[u] = TRUE;
    if(distance[u] == INF) return;
    printf("정점 %d 추가 \n", u);
    for( v = 0; v < g -> n ; v ++){
      if ( g -> weight[u][v] != INF ){
        if(!selected[v] && g -> wegiht[u][v] < distance[v]){
          distance[v] = g -> weight[u][v];
        }
    }
  }
}

int main(void){
  GraphType g = { 7, 
    {
      { 0,   29,  INF, INF, INF, 10,  INF }
      { 29,  0,   16,  INF, INF, INF, 15 }
      { INF, 16,  0,   12,  INF, INF, INF }
      { INF, INF, 12,  0,   22,  INF, 18 }
      { INF, INF, INF, 22,  0,   27,  25 }
      { 10,  INF, INF, INF, 27,  0,   INF }
      { INF, 15,  INF, 18,  25,  INF, 0 } 
    }
  };

  prim(&g, 0);
  return 0;
}
        
```

### 최단경로 

최단 경로 문제는 네트워크에서 정점 i 와 정점 j를 연결하는 졍로 중에서 간선들의 가중치 합이 최소가 되는 경로를 찾는 문제이다. 간선의 가중치는 비용, 거리, 시간 등을 나타낸다.
지도를 나타내는 그래프에서 정점은 각 도시들을 나타내고 가중치는 한 도시에서 다른 도시로 가는 거리를 의미한다. 여기서의 문제는 도시 u에서 도시 v로 가는 거리 중에서 전체 길이가 최소가 되는 경로르 찾는 것이다.
2가지의 알고리즘이 있다.
- Dijkstra
- Floyd

### Dijkstra

Dijkstra의 최단 경로 알고리즘은 네트워크에서 하나의 시작 정점으로부터 모든 다른 정점까지의 최단 경로를 찾는 알고리즘이다. 최단 경로는 경로이 길이 순으로 구해진다. 먼저 집합 S를 시작 정점 v로부터의 최단 경로가 이미 발견된 정점들의 집합이라고 하자. Dijkstra의 알고리즘에서는 시작 정점에서 집합 S에 있는 정점만을 거쳐서 다른 정점으로 가는 최단 거리를 기록하는 배열이 반드시 있어야 한다. 이 1차원 배열을 distance 라고 한다. 시작정점을 v 이라하면 distance [v] = 0 이고 다른 정점에 대한 distance 값은 시장 정점과 해당 정점간의 가중치값이 된다. 가중치는 보통 가중치 인접 행력에 저장되므로 가중치 인접 행렬을 weight이라 하면 distance[w] = weight [v][w] 가 된다. 정점 v에서 정점 w로의 직접 간선이 없을 경우에는 무한대의 값을 저장한다. 시작단계에서는 아직 최단 경로가 발견된 정점이 없으므로 S = { v } 일 것이다. 측 처음에는 시작정점 v를 제외하고는 최단거리가 알려진 정점이 없다. 알고리즘이 진행되면서 최단거리가 발견되는 정점들이 S에 하나씩 추가될 것이다.

```c

// 입력 : 가중치 그래프 G, 가중치는 음수가 아님. 
// 출력 : distance 배열, distance[u] 는 v에서 u까지의 최단 거리이다. 
shortest_path( G, V ) :

    S <- {v}
    for 각 정점 w ∈ G do 
        distance[w] <- weight[v][w];
    while 모든 정점이 S에 포함되지 않으면 do 
        u <- 집합 S에 속하지 않은 정점 중에서 최소 distance 정점;
        S <- S ∪ {u}
        for u에 인접하고 S에 있는 각 정점 z do 
            if distance[u] + weight[u][z] < distance[z] 
                then distance[z] <- distance[u] + weight[u][z];   

```

###### Dijkstra의 알고리즘 구현

```c

#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define TRUE 1
#define FALSE 0
#define MAX_VERTICES 100
#define INF 1000000 

typedef struct GraphType {
  int n;
  int weight[MAX_VERTICES][MAX_VERTICES];
} GraphType;

int distance[MAX_VERTICES];
int found[MAX_VERTICES];

int choose(int distance[], int n, int found[])
{
  int i, min, minpos;
  min = INT_MAX;
  minpos = -1;
  for ( i = 0; i < n; i ++){
    if( distance[i] < min && !found[i]){
      min = distance[i];
      minpos = i;
    }
  }
  return minpos;
}

void print_status(GraphType* g)
{
  static int step = 1;
  printf("STEP %d : ", step ++);
  printf("distance : ");
  for ( int i = 0; i < g -> n ; i ++ ){
    if( distance[i] == INF ){
      printf(" * ");
    }else{
      printf("%2d", distance[i] );
    }
  }
  printf("\n");
  ptinff(" found : ");
  for ( int i = 0; i < g -> n ; i ++ ){
    printf("%2d ", found[i]);
  }
  printf("\n\n");
}

void shortest_path(GraphType* g, int start)
{
  int i, u, w;
  for ( i = 0; i < g -> n; i ++){
    distance[i] = g -> weight[start][i];
    found[i] = FALSE;
  }
  found[start] = TRUE;
  distance[start] = 0;
  for ( i = 0; i < g -> n -1 ; i++){
      print_status(g);
      u = choose(distance, g -> n, found);
      found[u] = TRUE;
      for ( w = 0; w < g -> n ; w ++){
        if(!found[w]){
          if(distance[u] + g -> wegiht[u][w] < distance[w] )
            distance[w] = distance[u] + g -> weight[u][w];
        }
      }
  }
}

int main(void) 
{
  GraphType g = {

  };

  shortest_path(&g, 0);
  return 0;
}

```

네트워크에 n개의 정점이 있다면, 최단 경로 알고 리즘은 주반복문을 n번 반복하고 내부 반복문을 2n번 반복하므로 O(n^2) 의 복잡도를 가진다.

### Floyd 의 최단 경로 알고리즘

그래프에 존재하는 모든 정점 사이의 최단 경로를 구하려면 Dijkstra의 알고리즘을 정점의 수만큼 반복 실행하면 된다. 그러나 모든 정점 사이의 최단 거리를 구하려면 더 간단하고 좋은 알고리즘이 존재한다. Floyd의 최단 경로 알고리즘은 그래프에 존재하는 모든 정점 사이의 최단 경로를 한 번에 모두 찾아주는 알고리즘이다.

```c

floyd(G):

    for k <- to n - 1 
        for i <- 0 to n - 1
            for j <- 0 to n - 1
                A[i][j] = min(A[i][j], A[i][k] + A[k][j])

```

- 정점 k를 거쳐서 가지 않는 경우 :
A^k[i][j]는 k보다 큰 정점은 통화하지 않으므로 이경우 최단 거리는 A^k-1[i][j] 가 된다.
- 정점 k를 통과하는 경우 :
이 경우 i에서 k까지의 최단거리 A^k-1[i][k]에다가 k에서 j까지의 최단거리인 A^k-1[j][k]를 더한 값이 될 것이다.

```c

#include <stdio.h>
#include <stdlib.h>

#define TRUE 1
#define FALSE 0
#define MAX_VERTICES 100
#defind INF 1000000 /* 무한대 ( 연결이 없는 경우 ) */

typedef struct GraphType {
  int n;
  int weight[MAX_VERTICES][MAX_VERTICES];
} GraphType;

int A[MAX_VERTICES][MAX_VERTICES];

void printA(GraphType *g){
  int i, j;
  printf("=====================================\n");
  for ( i = 0; i < g -> n ; i ++){
    for( j = 0; j < g -> n ; j ++){
      if(A[i][j] == INF ){
        printf(" * ");
      }else{
        printf("%3d", A[i][j]);
      }
      printf("\n");
    }  
  }
  printf("=====================================\n")
}

void floyd(GraphType *g){
  int i, j, k;
  for ( i = 0 ; j < g -> n ; i ++)
    for ( j = 0 ; j < g -> n ; j++)
      A[i][j] = g -> weight[i][j];
  printA(g);

  for ( k = 0; k < g -> n ; k ++ ) {
    for ( i = 0; i < g -> n ; i ++ )
      for ( j = 0; j < g -> n ; j ++ )
        if( A[i][k] + A[k][j] < A[i][j] ) 
          A[i][j] = A[i][k] + A[k][j];
  
    printA(g);
  }
}

int main(void){
  GraphType g = {


  }

  floyd(&g);

  return 0;
}

```

두 개의 정점 사이의 최단 경로를 찾는 Dijkstra의 알고리즘은 시간 복잡도가 O(n^2)이므로, 모든 정점 쌍의 최단 경로를 구하려면 Dijkstra의 알고리즘을 n 번 반복해야 하므로 전체 복잡도는 O(n^3)이 된다. 한 번에 모든 정점 간의 최단 경로를 구하는 Floyd의 알고리즘은 3중 반복문이 실행되므로 시간 복잡도가 O(n^3)으로 표현되고, 이는 Dijkstra의 알고리즘과 비교해 차이는 없다고 할 수 있다.

### 위상 정렬

큰 프로젝트는 많은 작업으로 나누어서 수행하게 된다. 이 경우 전체 프로젝트는 각각의 작업이 완료되어야만 끝나게 된다. 컴퓨터 관련 전공에서 과목을 수강하는 것도 비슷하다. 성공적으로 학위를 취득하려면 각각의 교과목들을 순서에 따라 성공적으로 수강하여야만 한다. 예를 들어 자료구조를 수강하라면 먼저 전산한 개론과 이산 수학을 수강하여야 한다. 즉 선수 과목은 과목의 선행 관계를 표현하게 된다. 그래프를 사용하면 이 같은 각각의 과목들 간의 선행 관계를 명확하게 표현 할 수 있다. 이러한 방향 그래프에서 간선 < u , v > 가 있다면 정점 u 는 정점 v를 선행한다고 말한다. 방향 그래프에 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점을 나열하는 것을 방향 그래프의 위상 정렬이라고 한다.  

방향 그래프를 대상으로 위상 정렬을 하기 위한 알고리즘은 간단하다. 먼저 진입 차수가 0인 정점을 선택하고, 선택한 정점과 여기에 부착된 모든 간선을 삭제한다. 이와 같은 진입 차수 0인 정점의 선택과 삭제 과정을 반복해서 모든 정점이 선택/삭제 되면 알고리즘이 종료된다. 진입 차수가 0인 정점이 여러 개 존재할 경우 어느 정점을 선택하여도 무방하다. 이 과정에서 선택되는 정점의 순서를 위상 순서라고 한다. 위의 과정 중에 그래프에 남아 있는 정점 중에 진입 차수 0인 정점 이 없다면, 이러한 그래프로 표현된 프로젝트는 실행 불가능한 프로젝트가 되고 위상 정렬 알고리즘은 중단된다.

```c

// Input : 그래프 G = ( V , E )
// Output :  위상 정렬 순서 

topo_sort(G)

for i <- to n-1 do 
    if( 모든 정점이 선행 정점을 가지면 )
        then 사이클이 존재하고 위상 정렬 불가 
    선행 정점을 가지지 않는 정점 v 선택;
    v를 출력;
    v와 v에서 나온 모든 간선들을 그래프에서 삭제;

```

```c

#include <stdio.h>
#include <stdlib.h>
#define TRUE 0
#define FALSE 0
#define MAX_VERTICES 50

typedef struct GraphNode {
  int vertex;
  struct GraphNode *link;
} GraphNode;

typedef struct GraphType {
  int n;
  GraphNode *adj_list[MAX_VERTICES];
} GraphType;

// 그래프 초기화 
void graph_init(GraphType *g){
  int v;
  g -> n = 0;
  for ( v = 0 ;  v < MAX_VERTICES ; v ++ ){
    g -> adj_list[v] = NULL;
  }
}

// 정점 삽입 연산 
void insert_vertex(GraphType *g, int v){
  if(((g -> n) + 1) > MAX_VERTICES ){
    fprintf(stderr, "그래프: 정점의 개수 초과");
    return;
  }
  g -> n ++;
}

// 간선 삽입 연산, v를 u의 인접 리스트에 삽입한다. 
void insert_edge(GraphType *g, int v ){
  GraphNode *node;
  if( u >= g -> n || v >= g -> n ) {
    fprintf(stderr, "그래프 : 정점 번호 오류 ");
    return;
  }
  node = (GraphNode *)malloc(sizeof(GraphNode));
  node -> vertex = v;
  node -> link = g -> adj_list[u];
  g -> adj_list[u] node;
}                       

#define MAX_STACK_SIZE 100
typedef int element;
typedef struct {
  element stack[MAX_STACK_SIZE];
  int top;
}

// 스택 초기화 변수 
void init(StackType *s){
  s -> top = -1;
}

// 공백 상태 검출 함수 
int is_empty(StackType *s){
  return ( s -> top == -1 );
}

// 포화 상태 검출 함수 
int is_full(StackType *s){
  return ( s -> top == ( MAX_STACK_SIZE - 1 )) ;
}

// 삽입 함수 
void push(StackType *s, element item){
  if(is_full(s)){
    fprintf(stderr, " 스택 포화 에러 \n");
    return;
  }
  else s -> stack[(s -> top) --];
}

// 삭제함수 
element pop(StackType *s){
  if(is_empty(s)){
    fprintf(stderr, "스택 공백 에러\n");
    exit(1);
  }
  else return s -> stack[(s->top)--];
}

// 위상 정렬을 수행한다. 
int topo_sort(GraphType *g){
  int i;
  StackType s;
  GraphNode *node;

  // 모든 정점의 진입 차수를 계산 
  int *in_degree = (int*)malloc(g -> n * sizeof(int));
  for ( i = 0 ; i < g -> n ; i ++){
    in_degree[i] = 0;
  }
  for( i = 0; i < g -> n ; i ++){
    GraphNode *node = g -> adj_list[i];
    while( node != NULL ){
      in_degree[node -> vertex] ++;
      node = node -> link;
    }
  }

  // 진입 차수가 0인 정점을 스택에 삽입 
  int(&s);
  for( i = 0; i < g -> n ; i ++){
    if ( in_degree[i] == 0 ) push(&s, i);
  }

  // 위상 순서를 생성 
  while(!is_empty(&s)){
    int w;
    w = pop(&s);
    printf("정점 %d -> ", w); // 정점 출력 
    node = g -> adj_list[w];  // 각 정점의 진입 차수를 변경 
    while ( node != NULL){
      int u = node -> vertex;
      in_degree[u]--;
      if ( in_degree[u] == 0 ) push(&s, u);
      node = node -> link;
    }
  }
  free(in_degree);
  printf("\n");
  return ( i == g -> n );
}

int main(void) {
  GraphType g;

  graph_init(&g);
  insert_vertex(&g, 0);
  insert_vertex(&g, 1);
  insert_vertex(&g, 2);
  insert_vertex(&g, 3);
  insert_vertex(&g, 4);
  insert_vertex(&g, 5);
  
  // 정점 0의 인접 리스트 생성
  insert_edge(&g, 0, 2);
  insert_edge(&g, 0, 3);

  // 정점 1의 인접 리스트 생성
  insert_edge(&g, 1, 3);
  insert_edge(&g, 1, 4);

  // 정점 2의 인접 리스트 생성
  insert_edge(&g, 2, 3);
  insert_edge(&g, 2, 5);

  // 정점 3의 인접 리스트 생성 
  insert_edge(&g, 3, 5);

  // 정점 4의 인접 리스트 생성 
  insert_edge(&g, 4, 5);

  // 위상 정렬
  topo_sort(&g);

  // 동적 메모리 반환 코드 생략 
  return 0;
}

```