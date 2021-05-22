---
title: "머신러닝을 이해하기 위해서"
excerpt: "용어들에 대해서 정리"

categories:
  - Deep Learning
tags:
  - Deep Learning
classes: wide
last_modified_at: 2019-05-16T16:00:00-05:00
---

> 지금 이 순간의 모습은 모두 본인의 선택으로 인한 것이므로 누구도 탓할 수 없다.  

***

# 기계학습 ( Machine Learning )

'어떤 컴퓨터 프로그램이 P에 의해서 측정된 테스크 T에서의 성능이 경험 E를 학습헸다고 할 수 있다'라고 정의하였다. 

 - 머신러닝에서 중요한 요소는 학습 데이터, loss 함수, 최적화 알고리즘, 그리고 당연하지만 모델 자치입니다. 
 - 벡터화는 모든것(수학)을 좋게 만들고, (코드를) 빠르게 만들어 줍니다. 


***

# 선형회귀 

회귀 문제는 주어진 데이터 포인트 x에 해당하는 실제 값으로 주어지는 타겟 y를 예측하는 과제입니다. 회귀 문제는 현실에서 많이 보이는 문제입니다. 
예를 들면, 주택 가격, 기존, 판매량 등과 같은 연속된 값을 예측하는 문제들을 들 수 있습니다. 이는 결과 값이 이미지 분류와 같이 과일의 종류를 예측하는 이산적인 구분문제와는 다릅니다. 

![](https://keepinmindsh.github.io/lines/assets/img/deeplearning_01.jpeg){: .align-center} 
 

# 로지스틱 회귀 

# K-Means

# k-Nearet Neighbor 

# Recurrent Neural Network - 순환신경망 

# 다중회귀

# 다중선형회귀 

### 최소 자승법

### 최대우도추정법

# RNN-LSTM : 시계열 데이터 처리에 강점 

# Convolutional Neural Network - 합성곱신경망

# 역전파 알고리즘 

# RMSE ( Root Mean Square Error )

# NSE ( Nash-Sutcliffe Efficiency )

***

# 알아야할 기본 용어 

### Example과 Sample

머신러닝을 위해서 수집한 데이터는 일반적으로 과거의 경험입니다. 과거의 경험을 통해서 미지의 패턴과 경향을 인식하는 지식을 습득한 후, 새로운 데이터의 결과를 예측하는 것을 목표로 합니다. 이러한 의미에서 수집한 데이터셋은 과거의 경험입니다. 이런 배경으로 볼 때 Example 및 Sample에 적합한 한글 용어는 사례와 견본이라고 할 수 있습니다. 이 중에서 사례가 더 적당한 용어라고 생각합니다.

### instance 

데이터별로 여러 개의 속성을 가지며, 간 단위 데이터의 속성은 하나의 객체 혹은 인스턴스로 표현할 수 있습니다. 각 사례를 개별적인 특성을 갖는 단위로 묶을 수 있다는 의미에서 인스턴스라고 표현하기도 합니다.

### Data Point 

다차원 공간에 위치로 표현되는 벡터라는 의미에서 Data Point