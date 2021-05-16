---
title:  "정렬"
excerpt: "정렬에 대한 구조, 작성방법에 대해서 알아보기"

categories:
  - Data Structure
tags:
  - Data Structure
classes: wide
last_modified_at: 2019-05-15T09:00:00-05:00
---

> 이상과 현실은 늘 다르다.  
> 늘 그 사이에서 삶의 균형을 잡는게 중요하겠지. 

***

정렬은 물건을 크기 순으로 오름차순이나 내림차순으로 나열하는 것을 의미한다. 예를 들어 책들은 제목순이나 저자순, 또는 발간 연도순으로 정렬이 가능하다. 사람도 나이나 키, 이름등을 이용하여 정렬할 수 있다. 물건 뿐만 아니라 어떤 형태의 것도 서로 비교만 가능하면 정렬 할 수 있다.  

일반적으로 보통 정렬시켜야 될 대상은 레코드라고 분린다. 레코드는 다시 필드라고 하는 단위로 나누어진다. 예를 들어 학생들의 레코드라면 이름, 학번, 주소, 전화번호 등이 필드가 될 것이다. 여러 필드 중에서 특별히 레코드와 레코드를 식별해주는 역할을 하는 필드를 키라고 한다. 학생들의 레코드의 경우에는 학번이 키가 될 수 있다. 정렬이란 결국 레코드들을 키값의 순서로 재배열하는 것이다.  

정렬 알고리즘을 평가하는 효율성의 기준으로는 정렬을 위해 필요한 비교 연산의 횟수와 이동 연산의 횟수다.  

- 단순하지만 비효율적인 방법
삽입 정렬, 선택 정렬, 버블 정렬 등
- 복잡하지만 효율적인 방법
퀵 정렬, 히프 정렬, 합병 정렬, 기수 정렬 등
- 안정성을 충족하는 방법
삽입 정렬 , 버블 정렬, 합병 정렬 등

# 선택 정렬  

선택 정렬은 가장 이해하기귀 쉬운 정렬 방법이다. 먼저 왼쪽 리스트와 오른쪽 리스트, 두 개의 리스트가 있다고 가정하자. 왼쪽 리스트에는 정렬이 완료된 숫자들이 들어가게 되며 오른쪽 리스트에는 정렬되지 않은 숫자들이 들어 있다. 선택 정렬은 오른쪽 리스트에서 가장 작은 숫자를 선택하여 왼쪽 리스트로 이동하는 작업을 되풀이 한다. 선택 정렬은 오른쪽 리스트가 공백 상태가 될 때 까지 이 과정을 되풀이 하는 정렬 기법이다.  

위와 같은 방식은 정렬을 위해서 정렬할 대상과 동일한 메모리 공간이 필요하기 때문에 추가 메모리를 요구하지 않는 제자리 정렬(in-place sorting) 방법을 이용한다.

- 선택 정렬 알고리즘

```c

selection_sort(A, n) :

for i <- 0 to n - 2 do 
    least <- A[i], A[i+1], ... , A[n-1] 중에서 가장 값의 인덱스 ;
    A[i] 와 A[least] 의 교환;
    i ++;

```

```c

#define SWAP(x, y, t) ( (t)=(x), (x)=(y), (y)=(t) )

void selection_sort(int list[], int n){
  int i, j, least, temp;

  for ( i = 0; i < n -1 ; i ++){
    least = i;
    for ( j = i + 1 ; j < n ; j ++ ){
      if ( list[j] < list[least]) least = j;
    }
    SWAP(list[i], list[least], temp);
  }
}

```

```c

#include <stdio.h>
#include <stdlib.h> 
#define MAX_SIZE 10
#define SWAP(x, y, t) ( (t)=(x), (x)=(y), (y)=(t) ) 

int list[MAX_SIZE];
int n;


void selection_sort(int list[], int n){
  int i, j, least, temp;

/*
   n = 5

  1회차 - i = 0 , j = 1 , n = 5
  2회차 - i = 1 , j = 2 , n = 5
  3회차 - i = 2 , j = 3 , n = 5
  4회차 - i = 3 , j = 4 , n = 5
*/

  for ( i = 0; i < n -1 ; i ++){
    least = i;
    for ( j = i + 1 ; j < n ; j ++ ){
      if ( list[j] < list[least]) least = j;
    }
    SWAP(list[i], list[least], temp);
  }
}

int main(void){
  int i;
  n = MAX_SIZE;
  srand(time(NULL));
  for ( i = 0; i < n ; i ++){  // 난수 생성 및 출력 
    list[i] = rand() % 100;       // 난수 발생 범위 0 ~ 99
  }

  selection_sort(list, n);        // 선택 정렬 

  for ( i = 0; i < n ; i ++){
    printf("%d ", list[i]);
  }
  printf("\n");
  return 0;
}

````

레코드의 교환 횟수는 외부 루프의 실행 횟수와 같으며 한번 교환하기 위하여 3번의 이동이 필요하므로 전체 이동 횟수는 3(n-1)이 된다. 선택 정렬의 장점은 자료 이동 횟수가 미리 결정된다는 점이다. 그러나 이동횟수 3(n - 1)으로 상당히 큰편이다. 또한 자료가 정렬된 경우에는 불필요하게 자기 자신과 딩을 하게 된다. 따라서 이문제를 개선하려면 다음과 같은 if문을 추가해야한다.

```c

if( i != least )
    SWAP(list[i], list[least], temp);
                        

```

###### 삽입 정렬

삽입 정렬은 정렬되어 있는 리스트에 새로운 레코드를 적절한 위치에 삽입하는 과정을 반복한다. 선택 정렬과 마찬가지로 입력 배열을 선택 정렬과 유사하게 입력 배열을 정렬된 부분과 정렬되지 않은 부분으로 나누어서 사용한다.

```c


insertion_sort(A, n):

1.  for i <- 1 to n - 1 do 
2.      key <- A[i];
3.      j <- i - 1;
4.      while j >= 0 and A[j] > key do 
5.          A[j+1] <- A[j];
6.          j <- j - 1;
7.      A[j + 1] <- key

1. 인덱스 1부터 시작한다. 인덱스 0은 이미 정렬된 것으로 볼 수 있다. 
2. 현재 삽입될 숫자인 i 번째 정수를 key 변수로 복사 
3. 현재 정렬된 배열은 i - 1까지 이므로 i-1 번째부터 역순으로 조사한다. 
4. j 값이 음수가 아니어야 하고 key 값보다 정렬된 배열에 있는 값이 크면 
5. j 번째를 j + 1 번째로 이동한다. 
6. j 를 하나 감소한다. 
7. j 번째 정수가 key 보다 작으므로 j + 1 번째가 key 값에 들어갈 위치다. 
                        

void insertion_sort(int list[], int n){
  int i, j, key;
  for ( i = 1 ; i < n ; i ++){
    key = list[i];
    for ( j = i; j >= 0 && list[j] > key ; j -- ){
      list[ j + 1] = list[j];
    }
    list[j + i] = key;
  }
}

```

삽입 정렬이 복잡도는 입력 자료의 구성에 따라서 달라진다. 먼저 입력 자료가 이미 정렬되어 있는 경우는 가장 빠르다. 삽입 정렬을 외부 루프는 n - 1 번 실행되고 각 단계에서 1번의 비교와 2번이 이동만 이루어지므로 총 비교 횟수는 n - 1 번 실행되고 각 단계에서 1번의 비교와 2번의 이동만 이루어지므로 총 비교 횟수는 n-1 번 , 총 이동횟수는 2(n-1)번이 되어 알고리즘의 시간 복잡도는 O(n)이다. 최악의 복잡도는 입력 자료가 역순일 경우이다.

###### 버블 정렬

버블 정렬은 인접한 2개의 레코드를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환하는 비교-교환 과정을 리스트의 왼쪽 끝에서 시작하여 오른쪽 끝 까지 진행한다. 이러한 리스트의 비교-교환 과정이 한번 완료되면 가장 큰 레코드가 리스트의 오른쪽 끝으로 이동돈다. 이러한 레코드의 이동 과정이 마치 거품 이 보글보글 떠오르는 것과 유사하여 버블 정렬이라고 부른다.

```c

BubbleSort(A, n);

for i <- 1 to 1 do 
    for j <- 0 to i - 1 do 
        i 와 j + 1 번째의 요소가 크기준이 아니면 교환 
        j ++ ;
    i--; 

```

버블 정렬의 알고리즘은 간단한데, 한번 실행 시 j = 0 부터 j = i -1 까지 반복하는 루프로 구성되고 j번째 요소와 j + 1번째 요소를 비교하여 크기 순으로 되어 있지 않으면 교환한다. i는 하나의 스캔이 감소할 때 마다 1씩 감소한다. 이런 스캔 과정이 n - 1 번 되풀이되면 정렬이 끝나게 된다.

```c

#define SWAP(x, y, t )  ( (t)=(x), (x)=(y), (y)=(t))

void bubble_sort(int list[], int n){
  int i, j, temp;
  for ( i = n - 1; i > 0 ; i -- ){
    if ( list[j] > list[j+1]) {
      SWAP(list[j], list[j + 1], temp );
    }
  }
}

```

- 버블 정렬의 복잡도 분석
버블 정렬의 비교 횟수와 이동 횟수를 계한하여 보자. 버블 정렬의 비교 횟수는 최선, 평균, 최악의 어떠한 경우에도 항상 일정하다.
이동 횟수는 입력 자료가 역순으로 정렬되어 있는 경우에 발생하고 그 횟수는 비교 연산의 횟수에 3을 곱한 값이다. 왜냐하면 하나의 SWAP 함수가 3개의 이동을 포함하고 있기 때문이다.
이런 경우에는 자료 이동이 한 번도 발생하지 않는다. 평균적인 경우에는 자료 이동이 0번에서 i번까지 같은 확률로 일어날 것이다. 따라서 이를 기반으로 계산하여 보면 O(n^2)의 알고리즘임을 알 수 있다. 버블 정렬의 가장 큰 문제 점은 순서에 맞지 않은 요소를 인접한 요소와 교확한다는 것이다. 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다. 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되은 일이 일어난다. 일반적으로 자료의 교환 작업이 자료의 이동 작업보다 더 복잡하기 때문에 버블 벙렬은 그 단순성에도 불구하고 거의 쓰이지 않는다.

###### 쉡정렬

쉘 정렬은 Donald L. Shell 이라는 사람이 제안한 방법으로 삽입 정렬이 어느 정도 정렬된 배열에 대해서는 대단이 빠른 것에 착안한 방법이다. 셀 정렬은 삽입 정렬의 O(n^2)보다 빠르다. 삽입 정렬의 최대 문제점은 요소들이 삽입될때, 이웃한 위치로만 이동한다는 것이다. 만약 삽입되어야 할 위치가 현재 위치에서 상당히 멀리 떨어진 곳이라면 많은 이동을 해야만이 제자리로 갈수 있다.  

삽입 정렬과 다르게 셀정렬은 전체 리스트를 한 번에 정렬하지 않는다. 대신에 먼저 정렬해야할 리스트를 일정한 기준에 따라 분류하여 연속적이지 않는 여러 개의 부분 리스트를 만들고, 각 부분 리스트를 삽입 정렬을 이용하여 정렬한다. 모든 부분 리스트가 정렬되면 셀 정렬은 부분 리스트의 개수가 1이 될 때까지 되풀이 된다. 부분 리스트를 구성할 때는 주어진 리스트의 각 k 번째 요소를 추출하여 만든다. 이 k를 간격 이라고 한다. 셀 정렬에서는 각 스탭 마다 간격 k를 줄여가므로 수행 과정이 반복될 때마다 하나의 부분 리스트에 속하는 레코드들의 개수는 증가된다. 마지막 스택에서는 간격의 값이 1이 된다.  

![](https://keepinmindsh.github.io/lines/assets/img/shell_sort.png){: .align-center} 

```c

// gap 만큼 떨어진 요소들을 삽입 정렬 
// 정렬의 범위는 first 에서 last 
inc_insertion_sort(int list[], int first, int gap){
  int i, j, key;
  for ( i = first + gap ; i < last ; i = i - gap ){
    key = list[i];
    for ( j = i - gap ; j >= first && key < list[j] ; j = j - gap )
  }
  list[j + gap] = key;
}

void shell_sort(int list[], int n ){
  int i, gap;
  for ( gap = n / 2 ; gap > 0 ; gap = gap / 2){
    if((gap % 2) == 0) gap ++ ;
    for ( i =0 ; i < gap ; i ++ ){
      inc_insertion_sort(list, i , n - 1 , gap );
    }
  }
}

```

- 연속적이지 않은 부분 리스트에서 자료의 교환이 일어나면 더 큰 거리를 이동한다. 반면 삽입 정렬에서는 한 번에 한 칸만 이동된다. 따라서 교환되는 아이템들이 삽업 졍렬보다는 최종 위치에 더 가까이 있을 가능성이 높아진다.
- 부분 리스트는 어느 정도 정렬이 된 상태이기 때문에 부분 리스트의 개수가 1이 되게 되면 셀 정렬은 기본적으로 삽입 정렬을 수행하는 것이지만 빠르게 수행된다. 이것은 삽입 정렬이 거의 정렬된 리스트에 대해서는 빠르게 수행되기 때문이다.

###### 합병 정렬

합병 정렬은 하나의 리스트를 두 개의 균등한 크기로 분할하고 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트를 얻고자 하는 것이다. 합병 정렬은 분할 정복 기법에 바탕을 두고 있다. 분할 정복 기법은 문제를 작은 2개의 문제로 분리하고 각각 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 전략이다. 분리된 문제가 아직도 해결하기 어렵다면, 즉 충분히 작지 않다면 분할 정복 방법을 연속하여 다시 적용한다.
- 분할 : 입력 배열을 같은 크기의 2개의 부분 배열로 분할한다.
- 정복 : 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 순환 호출을 이용하여 다시 분할 정복 기법을 적용한다.
- 결합 : 정렬된 부분 배열들을 하나의 배열에 통합한다.

**합병 정렬 알고리즘**

```c

merge_sort(list, left, right);

1. if left < right
2.    mid = ( left + right ) / 2;
3.    merge_sort(list, left, mid);
4.    merge_sort(list, mid + 1, right);
5.    merge(list, left, mid, right);


```

합병 정렬에서 실제로 정렬이 이루어지는 시점은 2개의 리스트를 합병하는 단계이다.

```c

merge(list, left, mid, last);
// 2개의 인접한 배열 list[left..mid] 와 list[mid+1..right]를 합병 

i <- left;
j <- mid + 1;
k <- left;

sorted 배열을 생성;
while ismid and j < right do 
    if ( list[i] < list[j] ) {
      then 
        sorted[k] <- list[i];
        k ++;
        i ++;
      else 
        sorted[k] <- list[j];
        k ++;
        j ++;
    }

// 요소가 남아 있는 부분 배열을 sorted로 복사한다. sorted를 list로 복사한다. 

```

```c

int sorted[MAX_SIZE]; // 추가 공간이 필요 

/*
  i는 정렬된 왼쪽 리스트에 대한 인덱스 
  j는 정렬된 오른쪽 리스트에 대한 인덱스 
  k는 정렬된 리스트에 대한 인덱스 
*/
void merge(int list[], int left, int right){
  int i, j, k, l;
  i = left; j = mid - 1; k = left;

  /* 분할 정렬된 list의 합병 */ 
  while (i <= mid && j <= right){
    if ( list[i] <= list[j] ){
      sorted[k++] = list[i++];
    }else {
      sorted[k++] = list[j++];
    }
  }
  if ( i > mid ){ // 남아 있는 레코드의 일괄 복사 
    for ( l = j ; l <= right ; l ++){
      sorted[k++] = list[l];
    }
  }else{ // 남아 있는 레코드의 일괄 복사 
    for ( l = i ; l <= right ; l ++ ){
      sorted[k++] = list[l];
    }
  }

  // 배열 sorted[]의 리스트를 배열 list[]로 재복사 
  for ( l = left; l <= right ; l ++ ){
    list[l] = sorted[l];
  }
}

void merge_sort(int list[], int left, int right){
  int mid;
  if( left < right ) {
    mid = ( left + right ) / 2;
    merge_sort(list, left, mid);
    merge_sort(list, mid + 1, right);
    merge(list, left, mid, right);
  }
}

```

- 합병 정렬은 순환 호출 구조로 되어 있다.
{% raw %}
배열이 부분 배열로 나누어지는 단계에서는 비교 연산이나 이동 연산은 수행되지 않는다. 부분 배열이 합쳐지는 merge 함수에서 비교 연산과 이동 연산이 수행되는 것이다. 순환 호출의 깊이 만큼의 합병 단계가 필요하다. 그러면 각 합병 단계에서 비교연산은 몇 번이사 수행되는 것일까? 이전의 에제인 n=2^3인 경우를 살펴보면 크기 1인 부분 배열 2개를 합병하는 데는 최대 2개의 비교 연산이 필요하고, 부분 배열의 쌍이 4개이므로 2*4=8의 비교연산이 필요하다. 다음 단계에서는 크기가 2인 부분 배열을 2개를 합치는데 최대 4번의 비교 연산이 필요하고, 부분 배열 2쌍이 있으므로 4*2=8번의 연산이 필요함을 알 수 있다. 마지막 합병 단계인 크기가 4인 부분 배열 2개를 합병하는 데는 최대 8번의 비교 연산이 필요하다. 따라서 또한 **1번의 연산이 필요함을 알 수 있다. 일반적인 경우를 유추해보면 하나의 합병 단계에서는 최대 n 번의 비교 연산이 필요함을 알 수 있다. 그러나 합병 단계가 k = log2n번 만큼 있으므로 총 비교 연산은 최대 nlog2n번 필요하다. 이동연산은 얼마나 수행되는 것일까? 하나의 합병 단계에서 보면 임시 배열에 복사했다가 다시 가져와야 되므로 이동 연산은 총 부분 배열에 들어 있는 요소의 개수가 n 인 경우, 레코드 이동이 2n번 발생하므로 하나의 합병 단계에서 2n개가 필요하다. 따라서 log2n개의 합병단계가 필요하므로 총 2nlog2n개의 이동 연산이 필요하다. 결론적으로 합병정렬은 비교 연산과 이동연산의 경우 O의 복잡도를 가지는 알고리즘이다. 합병 정렬의 다른 장점은 안정적인 정렬 방법이며 데이터의 분포에 영향을 덜 받는다. 즉 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다. 즉, 최악, 평균, 최선의 경우가 다같이 O(nlog2n)인 정렬 방법이다. 합병 정렬의 단점은 임식 배열이 필요하다는 것과 만약 레코드들의 크기가 큰 경우에는 이동 횟수가 많으므로 합병 정렬은 매우 큰 시간적 낭비를 초래한다. 그러나 만약 레코드를 연결 리스트로 구성하여 합병 정렬 할 경우, 링크 인덱스만 변경되므로 데이터의 이동은 무시할 수 있을 정도로 작아진다. 따라서 크기가 큰 레코드를 정렬할 경우, 만약 연결 리스트를 사용한다면, 합병 정렬은 퀵 정렬을 포함한 다른 어떤 정렬 방법 보다 효율적일 수 있다.
{% endraw %}

###### 퀵정렬

퀵 정렬은 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법이다. 퀵 정렬도 분할-정복 법에 근거한다. 퀵 정렬은 합병 정렬과 비슷하게 전체 리스트를 2개의 부분 리스트로 분할하고 , 각각의 부분 리스트를 다시 퀵 정렬하는 전형적인 분할-정복법을 사용한다.  

퀵 정렬은 리스트를 비균등하게 분할 한다. 먼저 리스트 안에 있는 한 요소를 피벗으로 선택한다. 피벗보다 작은 요소들은 모두 피벗의 왼쪽으로 옮겨지고 피벗보다 큰 요소들은 모두 피벗의 오른쪽으로 옮겨진다. 결과적으로 피벗을 중심으로 왼쪽은 피벗보다 작은 요소들로 구성되고, 오른쪽은 피벗보다 큰 요소들로 구성된다. 이 상태에서 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬하게 되면 전체리스트가 정렬된다. 

- 퀵 정렬 알고리즘

```c

void quick_sort(int list[], int left, int right){
  if ( left < right ) {
    int q = partition(list, left, right );
    quick_sort(list, left, q - 1);
    quick_sort(list, q + 1 , right);
  }
}

```

- 정렬할 범위가 2개 이상의 데이터 이면
- partition 함수를 호출하여 피벗을 기준으로 2개이 리스트로 분할한다. partition 함수의 반환 값은 피봇의 위치가 된다.
- left 에서 피봇 위치 바로 앞까지를 대상으로 순환 호출한다.
- 피봇 위치 바로 다음부터 right 까지를 대상으로 순환 호출한다.

```c

int partition(int list[], int left, int right ){
  int pivot, temp;
  int low, high;

  low = left;
  high = right + 1;
  pivot = list[left];

  do {
    do 
      low ++;
    while( list[low] < pivot );
    do 
      high --;
    while ( list[high] > pivot );
    if ( low < high ) SWAP(list[low], list[high], temp);
  } while ( low < high );

  SWAP(list[left], list[high], temp);

  return high;
}

```

- low 는 left + 1 에서 출발, do-while 루프에서 먼저 증가 시킴을 주의하라.
- high 는 right에서 출발, do-while 푸르에서 먼저 감소 시킴을 주의하라.
- 정렬할 리스트의 가장 왼쪽 데이터를 피벗으로 선택한다.
- low 와 high 교차할 때 까지 계속 반복 한다.
- list[low] 가 pivot 보다 작으면 계속 low를 증가시킨다.
- list[high] 가 pivot 보다 크면 계속 high를 증가시킨다.
- low 와 high가 아직 교차하지 않았으면 list[low] 와 list[high]를 교환한다.
- 만약 low와 high 가 교차하였으면 반복을 종료한다.
- 피봇을 중앙에 위치시킨다.
- 피봇의 위치를 반환한다.

```c

#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_SIZE 10
#define SWAP(x, y, t) ( (t) = (x) , (x) = (y) , (y) = (t) )

int list[MAX_SIZE];
int n;

int partition(int list[], int left, int right ){
  int pivot, temp;
  int low, high;

  low = left;
  high = right + 1;
  pivot = list[left];

  do {
    do 
      low ++;
    while( list[low] < pivot );
    do 
      high --;
    while ( list[high] > pivot );
    if ( low < high ) SWAP(list[low], list[high], temp);
  } while ( low < high );

  SWAP(list[left], list[high], temp);

  return high;
}

void quick_sort(int list[], int left, int right){
  if ( left < right ) {
    int q = partition(list, left, right );
    quick_sort(list, left, q - 1);
    quick_sort(list, q + 1 , right);
  }
}

int main(void){
  int i;
  n = MAX_SIZE;
  srand(time(NULL));
  for ( i = 0 ; i < n ; i ++){
    list[i] = rand() % 100;
  }

  quick_sort(list, 0, n -1 );
  for( i = 0; i < n ; i ++ ){
    printf("%d ", list[i]);
  }
  printf("\n");
  return 0;
}

```

**시간복잡도**  
n이 2의 거듭제곱이라고 가정하고 만약에 퀵 정렬에서의 리스트 분할이 항상 리스트의 가운데에서 이루어진다고 가정하면 합병 정렬의 복잡도 분석과 마찬가지로 n개이 레코드를 가지는 리스트는 n/2, n/4, n/8, ... , n/2^k 의 크기로 나누어질 것이다. 크기가 1이 될때까지 나누어지므로 n/2^k = 1 일 때까지 나누어질 것이고 따라서 k = log2n 개의 패스가 필요하게 된다. 각각의 패스에서는 전체 리스트의 대부분의 레코드를 비교해야하므로 평균 n 번 정도의 비교가 이루어지므로 퀵정렬은 비교 연산을 총 nlog2n 번 실행하게 되어 O(nlog2n)의 복잡도를 가지는 알고리즘이 된다.
퀵 정렬은 평균적인 경우의 시간 복잡도가 O(nlog2n)으로 나타난다. 특히 O(nlog2n)의 복잡도를 가지는 다른 정렬 알고리즘과 비교하였을 때도 가장 빠른 것으로 나타났다. 이는 퀵 정렬이 불필요한 데이터의 이동을 중리고 먼 거리의 데이터를 교환할 뿐만 아니라, 한번 결정된 피벗들이 추후 연산에서 제외되는 특성 등렉 기인한다고 보인다. 퀵 정렬은 속도가 빠르고 추가 메모리 공간을 필요로 하지 않는 등의 장점이 있는 반면에 정렬된 리스트에 대해서는 오히려 수행시간이 더 많이 걸리는 등의 단점도 가진다. 이러한 불균현 분할을 방지하기 위하여 피벗을 선택할 때 리스트의 왼쪽 데이터를 사용하는 대신에 보다 리스트의 중앙 부분을 분할 할 수 있는 데이터를 선택한다. 많이 사용되는 방법은 리스트 내의 몇 개의 데이터 중에서 중간값을 피벗으로 선택하는 것이다. 일반적으로 리스트의 뢴쪽, 오른쪽, 중간의 3개의 데이터 중에서 중간 값을 선택하는 방법이 많이 사용된다.   

**활용**  
대개의 C언어 실행시간 라이브러리에 퀵 정렬 함수가 제공된다. 대개 qsort란 이름으로 제공된다. qsort 함수는 일반적인 구조체 배열을 정렬하기 위하여 제작되었다.

**함수의 원형**  
```c

void qsort(
  void *base,     // 배열의 시작주소
  size_t num,     // 배열 요소의 개수 
  szie_t width,   // 배열 요소 하나의 크기 ( 바이트 단위 )
  int (*compare)(const void *, const void *)
  // 포인터를 통하여 두개의 요소를 비교하여 비교 결과를 정수로 
  // 반한하는 함수 
)   

```

위의 함수는 각 요소가 width 바이트인 num 개의 요소를 가지는 배열에 대하여 퀵정렬을 수행한다. 입력 배열은 정렬된 값으로 덮어 씌워진다. compare는 배열 요소 2개를 서로 비교하는 사용자 제공 함수로 qsort 함수가 요소들을 비교할 때마다 다음과 같이 호출하여 사용한다.

```c

compare( (void *)elem1, (void *)elem2 )

```

```c

#include <stdlib.h> 
#include <string.h> 
#include <stdio.h> 

int compare(const void *arg1, const void *arg2){
  if(*(double *)arg1 > *(double *)arg2) return 1;
  else if (*(double *)arg1 == *(double *)arg2) return 2;
  else return -1;
}

//
int main(void){
  int i;
  double list[5] = { 2.1, 0.9, 1.6, 3.8, 1.2 };
  qsort((void *)list, (size_t)5, sizeof(double), compare);
  for ( i = 0; i < 5; i ++ ){
    printf("%f", list[i]);
  }
  return 0;
}

```

###### 히프정렬

히프는 우선 순위 큐를 완전 이진 트리로 구현하는 방법으로 최댓값이나 최솟값을 쉽게 추출 할 수 있는 자료 구조이다. 히프에는 최소 히프와 최대 히프가 있고 정렬에서는 최소 히프를 사용하는 것이 프로그램이 더 쉬워진다. 최소 피히는 부모 노드의 값이 자식 노드의 값보다 작다. 따라서 최소 히프의 루트 노드는 가장 작은 값을 갖게 된다. 최소 히프의 이러한 특성을 이용하여 정렬할 배열을 먼저 최소 히프로 변환한 다음, 가장 작은 원소부터 차례대로 추출하여 정렬하는 방법을 히프 정렬이라 한다.

###### 기수정렬

기수 정렬은 레코드를 비교하지 않도록 정렬하는 방법이다. 기수 정렬은 입력 데이터에 대해서 어떤 비교 연산도 실행하지 않고, 데이터를 정렬할 수 있는 색다른 정렬기법이다. 정렬에 기초한 방법들은 절대 O(nlog2n)이라는 이론적인 하한선을 깰수 없는데 반하여 기수 정렬은 이 하한선을 깰 수 있는 유일한 기법이다. 사실 기수 정렬은 O(kn)의 시간 복잡도를 가지는데 대부분은 K < 4 이하 이다. 다만 문제는 기수 정렬이 추가적인 메모리를 필요로 한다는 것인데 이를 감안하더라도 기수 정렬이 다른 정렬 기법보다 빠르기 때문에 데이터를 정렬하는 상당히 인기있는 정렬 기법 중의 하나이다. 기수 정렬의 단점은 정렬할 수 있는 레코드의 타입이 한정된다는 점이다. 즉 기수 정렬을 사용하려면 레코드의 키들이 동일한 길이를 가지는 숫자나 문자열로 구성되어 있어야 한다.
기수(radix)란 숫자의 자리수이다. 예를 들면 숫자 42는 4와 2의 두 개의 자리수를 가지고 이것이 기수가 된다. 기수 정렬은 이러한 자리수의 값에 따라서 정렬하기 때문에 기수 정렬이라는 이름을 얻었다. 기수 정렬은 다단계 정렬이다. 단계의 수는 데이터의 자리수의 개수와 일치한다.

```c

RadixSort(list, n);

for d <- LSD의 위치 to MSD의 위치 do 
{
  d번째 자릿수에 따라 0번부터 9번 버킷에 집어놓는다. 
  버킷에서 숫자들을 순차적으로 읽어서 하나의 리스트로 합친다. 
  d++;
}

```

각각의 버킷에 들어간 숫자들은 먼저 나와야 한다. 따라서 각각의 버킷은 큐로 구현되어야 한다. 큐로 구현되어야 리스트 사에 있는 요소들의 상대적인 숫자가 유지된다. 버킷에 숫자를 집어넣는 연산은 큐의 삽입 연산이 되고 버킷에서 숫자를 읽는 연산은 삭제 연산으로 대치하면 된다.
                        
```c

#include <stdio.h>
#include <stdlib.h>

#define MAX_QUEUE_SIZE 100
typedef int element;

typedef struct {
  element data[MAX_QUEUE_SIZE];
  int front, rear;
} QueueType;

// 오류 함수 
void error(char *message){
  fprintf(stderr, "%s\n", message);
  exit(1);
}

// 공백 상태 검출 함수 
void init_queue(QueueType *q){
  q -> front = q -> rear = 0;
}

// 공백 상태 검출 함수 
int is_empty(QeueuType *q){
  return ( q -> front == q -> rear);
}

// 포화 상태 검출 함수 
int is_full(QueueType *q){
  return ((q->rear +1) % MAX_QUEUE_SIZE == q -> front );
}

// 삽입 함수 
void enqueue(QueueType *q, element item){
  if(is_full(q))
    error("큐가 포화상태입니다. ");
  q -> rear = ( q -> rear + 1) % MAX_QUEUE_SIZE;
  q -> data[q->rear] = item;
}

// 삭제 함수 
element dequeue(QueueType *q){
  if(is_empty(q))
    error("큐가 공백 상태입니다.");
  
    q -> front = ( q -> front + 1) % MAX_QUEUE_SIZE;
    return q -> data[ q -> front ];
}

#define BUCKETS 10
#define BUCKETS 4
void radix_sort(int list[], int n){
  int i, b, d, factor = 1;
  QueueType queues[BUCKETS];

  for ( b = 0; b < BUCKETS; b ++ ) init_queue(&queues[b]);

  for ( d = 0; d < DIGITS ; d ++){
    for ( i = 0; i < n ; i ++ )
      enqueue(&queues[(list[i] / factor) % 10], list[i]);

    for ( b = i = 0; b < BUCKETS; b ++ ){
      while(!is_empty(&queues[b]))
        list[i++] = dequeue(&queues[b]);
    }

    factor *= 10;
  }
}

#define SIZE 10

int main(void) {
  int list[SIZE];
  srand(time(NULL));
  for ( int i = 0; i < SIZE ; i ++){
    list[i] = rand() % 100;
  }

  radix_sort(list, SIZE);
  for ( int i = 0; i < SIZE ; i++ ){
    printf("%d", list[i]);
  }
  printf("\n");
  return 0;
}

```

만약 입력 리스트가 n개의 정수를 가지고 있다고 하면 알고리즘의 내부 루프는 n번 반복될 것이다. 만약 각 정수가 d개의 자리수를 가지고 있다고 하면 외부 루프는 d번 반복 된다,. 따라서 기수 정렬언 O(d*n) 의 시간복잡도를 가진다. 시간 복잡도가 d 에 비례하기 때문에 기수 정렬의 수행 시간은 정수의 크기와 관련이 있다

###### 정렬 알고리즘의 비교

- 삽입 정렬 : 최선 - O(n) , 평균 - O(n^2) , 최악 - O(n^2)
- 선택 정렬 : 최선 - O(n^2) , 평균 - O(n^2) , 최악 - O(n^2)
- 버블 정렬 : 최선 - O(n^2) , 평균 - O(n^2) , 최악 - O(n^2)
- 쉘 정렬 : 최선 - O(n) , 평균 - O(n^1.5) , 최악 - O(n^1.5)
- 퀵 정렬 : 최선 - O(nlog2n) , 평균 - O(nlog2n) , 최악 - O(n^2)
- 히프 정렬 : 최선 - O(nlog2n) , 평균 - O(nlog2n) , 최악 - O(nlog2n)
- 합병 정렬 : 최선 - O(nlog2n) , 평균 - O(nlog2n) , 최악 - O(nlog2n)
- 기수 정렬 : 최선 - O(dn) , 평균 - O(dn) , 최악 - O(dn)

```c

#include <stdio.h>
#include <string.h>

#define SWAP(x, y, t) ( (t)=(x), (x)=(y), (y)=(t) )
#define MAX_WORD_SIZE 50
#define MAX_MEANING_SIZE 500
#define SIZE 5

typedef struct {
  char word[MAX_WORD_SIZE];
  char meaning[MAX_MEANING_SIZE];
} element;

element list[SIZE];

int main(void){
  int i, j;
  element temp;

  printf("5개의 단어와 의미를 입력하시오. ");

  for ( i = 0; i < SIZE - i ; i++){
    scanf("%s[^\n]", list[i].word);
    scanf("%s[^\n]", list[i].meaning);
  }

  // 버블 정렬 
  for ( i = 0; i < SIZE - 1; ++i ){
    for ( j = i + 1 ; j < SIZE ; j++ ) {
      if(strcmp(list[i].word, list[j].word) > 0 ) {
        SWAP(list[i], list[j], temp);
      }
    }
  }

  printf("\n 정렬 후 사전의 내용 : \n");
  for ( i = 0; i < SIZE; i ++ ){
    printf("%s: %s \n", list[i].word , list[i].meaning);
  }
}

```