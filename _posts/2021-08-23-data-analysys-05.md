---
title: "데이터 분석 5일차 - 선형 회귀 분석 정리"
excerpt: "기본적인 선형 회귀 분석"

categories:
  - Deep Learning
tags:
  - Deep Learning
classes: wide
last_modified_at: 2021-07-29T07:31:00-05:00
---

> 하고 싶은게 있으면 시간이 조금 걸리더라도, 돌아가더라도 하는 게 맞을거야. 

***

x의 값 →  [ 6, 8, 19, 14, 18 ] : 피자의 지름
 
y의 값 → [ 7, 9, 13, 17.5, 18 ] : 피자의 가격

 

### y = α + βx 의 값을 구하기 위해서 x의 분산, x와 y의 공분산을 계산하였음.

- x는 설명 변수
- y는 반응 변수 

 

### x의 분산
집합의 모든 수가 동일하면 분산은 0이다. 분산이 작으면 숫자들이 평균 근처에 밀집해 있다는 뜻이고, 평균으로부터 멀리 떨어질 수록 분산은 커진다. 

 

### x와 y의 공분산
공분산은 두 변수가 얼마나 함께 변화하는지 측정한다.  두 변수가 같이 증가한다면 공분산은 양수이다. 한 변수가 증가할 때 다른 변수는 감소하면 공분산은 음수가 된다. 두 변수 사이에 선형 관계가 없다면 공분산은 0이된다. 

 

β의 값은 x와 y의 공분산에 x의 분산을 나눈 값으로 계산이 가능하다.   

α의 값은 x의 평균값, y의 평균값 , 계산된 β의 값을 이용해서 구할 수 있다.   

제공된 설명변수[x] 와 반응변수[y] 의 값의 각각의 평균과 β 를 구하기 위한 설명변수의 분산 , 설명변수와 반응변수의 공분산 값을 차례로 구하면   

 
- x 의 평균 : 11.2
- y의 평균 : 12.9

```python

# X는 훈련데이터의 특징이고, 여기서는 피자의 지름이다.
# scikit-learn은 특징 벡터 이름을 X로 사용한다.
# 대문자는 행렬을 의미하고 소문자는 벡터를 의미한다.
X = np.array([[6], [8], [10], [14], [18]]).reshape(-1, 1)
y = [ 7, 9, 13, 17.5, 18 ] # y 피자가격을 나타내는 벡터이다.

```

- x의 분산 : 23.2

```python

import numpy as np

# X는 훈련데이터의 특징이고, 여기서는 피자의 지름이다.
# scikit-learn은 특징 벡터 이름을 X로 사용한다.
# 대문자는 행렬을 의미하고 소문자는 벡터를 의미한다.
X = np.array([[6], [8], [10], [14], [18]]).reshape(-1, 1)

x_bar = X.mean()

print(x_bar)

# 표본 분산을 계산할 때 훈련 인스턴스 개수로 부터 1을 차감하는 것에 주목하자.
# 이 기법은 베셀 교정(Bessel's correction)이라 불린다. 표본으로부터 모집단의
# 분산을 추정할 때 편향을 줄여준다.
variance = (( X - x_bar) ** 2).sum() / (X.shape[0] -1)
print(variance)

``` 

- 산술 평균 : arithmetic mean ( numpy.mean() )

numpy.mean(arr, axis = None)  

  - axis = 1 일 경우, 컬럼을 따라서 평균값 계산 
  - axis = 0 일 경우, 행을 따라서 평균값 계산 


mean의 계산 방법  

```python 

# Python Program illustrating 
# numpy.mean() method 
import numpy as np
    
# 1D array 
arr = [20, 2, 7, 1, 34]
  
print("arr : ", arr) 
print("mean of arr : ", np.mean(arr))

```

출력 값   

```

arr :  [20, 2, 7, 1, 34]
mean of arr :  12.8

```

axis의 활용  

```python 

# Python Program illustrating 
# numpy.mean() method   
import numpy as np
    
  
# 2D array 
arr = [[14, 17, 12, 33, 44],  
       [15, 6, 27, 8, 19], 
       [23, 2, 54, 1, 4, ]] 
    
# mean of the flattened array 
print("\nmean of arr, axis = None : ", np.mean(arr)) 
    
# mean along the axis = 0 
print("\nmean of arr, axis = 0 : ", np.mean(arr, axis = 0)) 
   
# mean along the axis = 1 
print("\nmean of arr, axis = 1 : ", np.mean(arr, axis = 1))
  
out_arr = np.arange(3)
print("\nout_arr : ", out_arr) 
print("mean of arr, axis = 1 : ", 
      np.mean(arr, axis = 1, out = out_arr))

```

결과 값 

```python 

mean of arr, axis = None :  18.6

mean of arr, axis = 0 :  [17.33333333  8.33333333 31.         14.         22.33333333]

mean of arr, axis = 1 :  [24.  15.  16.8]

out_arr :  [0 1 2]
mean of arr, axis = 1 :  [24 15 16]

```

- x와 y의 공분산 : 22.65

```python


# 공분산은 두 변수가 얼마나 함께 변화하는지 측정한다. 두 변수가 같이 증가한다면 공분산은 양수다. 한 변수가 증가할 때 다른 변수는 감소하면 공분산은 음수가 된다.
# 두 변수 사이의 선형 관계가 없다면 공분산은 0이 된다. 그러나 선현으로 상관되지 안았다고 해서 반드시 서로 독립적이라는 의미는 아니다.

y = np.array([7, 9, 13, 17.5, 18])
y_bar = y.mean();

# 두 벡터가 모두 행 벡터가 되야 하므로 X를 전치(transpose)한다.

covariance = np.multiply((X - x_bar).transpose(), y - y_bar).sum() / ( X.shape[0] - 1 )
print(covariance)
 
```

- β의 값 : 0.98 

β는 x,y의 공분산의 값에 x의 분산을 나눈 값. 

```

β = cov(x,y) / var(x) 

```
 

- α의 값 : 1.924


```

α = y - βx 

```


- y는 평균값, x는 평균값  

α = 12.9 - 0.98 * 11.2 

 

### 위의 값을 기준으로 y = α + βx 를 이용해 11인치의 피자의 값을 예측 하면,   

y = 1.924 + 0.98 * 11 → 12.70