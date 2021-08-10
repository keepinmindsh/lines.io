---
title: "데이터 분석 1일차 - 선형회귀 분석"
excerpt: "기본적인 선형 회귀 분석"

categories:
  - Deep Learning
tags:
  - Deep Learning
classes: wide
last_modified_at: 2021-07-29T07:31:00-05:00
---

> Higher Hope. 

***

# 데이터 분석 - 선형 회귀 분석 1일차 


![](https://keepinmindsh.github.io/lines/assets/img/dataanalysys_20210811.png){: .align-center} 

훈련 벡터에 대한 결과 치는 위와 같음 

```python

# "np" , "plt"는 각각 NumPy 와 Matplotlib의 약칭으로 자주 사용된다.
import numpy as np
import matplotlib.pyplot as plt


# X는 훈련데이터의 특징이고, 여기서는 피자의 지름이다.
# scikit-learn은 특징 벡터 이름을 X로 사용한다.
# 대문자는 행렬을 의미하고 소문자는 벡터를 의미한다.
X = np.array([[6], [8], [10], [14], [18]]).reshape(-1, 1)
y = [ 7, 9, 13, 17.5, 18 ] # y 피자가격을 나타내는 벡터이다.

plt.figure()

# TODO 코드가 에러가 발생함.
# Traceback (most recent call last):
#   File "D:/GIT/sample/sample/python/python_deeplearning/dataanalysys_20210811.py", line 13, in <module>
#     plt.there('Pizza price plotted against diameter')
# AttributeError: module 'matplotlib.pyplot' has no attribute 'there'

#plt.there('Pizza price plotted against diameter')

plt.xlabel('Diameter in inches')
plt.ylabel('Price in dollars')
plt.plot(X, y, 'k.')
plt.axis([0, 25, 0, 25])
plt.grid(True)
plt.show()

```

이슈사항

 matplotlib - 3.4.2 버전 - 아래의 이슈가 존재함. 

```python

# TODO 2021-08-11 코드가 에러가 발생함. - there 함수가 아무래도 존재하지 않는듯 - import matplotlib.pyplot as plt 버전 확인 필요
# Traceback (most recent call last):
#   File "D:/GIT/sample/sample/python/python_deeplearning/dataanalysys_20210811.py", line 13, in <module>
#     plt.there('Pizza price plotted against diameter')
# AttributeError: module 'matplotlib.pyplot' has no attribute 'there'

#plt.there('Pizza price plotted against diameter')

```

