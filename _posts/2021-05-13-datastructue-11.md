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