---
title: "데이터 분석 4일차 - 기본 문법 공부"
excerpt: "기본적인 선형 회귀 분석"

categories:
  - Deep Learning
tags:
  - Deep Learning
classes: wide
last_modified_at: 2021-07-29T07:31:00-05:00
---

> 조금씩, 천천히

***

# pandas <https://pandas.pydata.org/>

pandas를 사용하기 위해서 PyCharm을 이용해서 pandas package를 설치하면 아래와 같이 import에서 사용 가능하다. 

```python

import pandas as pd

```

### pandas에서의 datatable에 대한 표현 

```python

import pandas as pd

df = pd.DataFrame(
    {
        "Name": [
            "Braund, Mr. Owen Harris",
            "Allen, Mr. William Henry",
            "Bonnell, Miss. Elizabeth",
        ],
        "Age": [22, 35, 58],
        "Sex": ["male", "male", "female"],
    }
)

print(df)

```

- DataFrame는 컬러명을 가지고 있는 2차원 데이터 구조 
- DateFrame의 각각의 컬럼은 Series 

```shell

D:\GIT\sample\sample\python\python_deeplearning\venv\Scripts\python.exe D:/GIT/sample/sample/python/python_deeplearning/dataanalysis_20210819_pandas.py
                       Name  Age     Sex
0   Braund, Mr. Owen Harris   22    male
1  Allen, Mr. William Henry   35    male
2  Bonnell, Miss. Elizabeth   58  female

Process finished with exit code 0

```

- 그래서 만약 내가 특정 컬럼에 관심이 있다면 아래와 같이 코딩이 가능하고, 

```python

print(df["Age"])

```

```shell

0    22
1    35
2    58
Name: Age, dtype: int64

```

- max 값을 구하거나 , 데이터의 basic statistics 에 관심이 있다면 

```python

print(df["Age"].max())

print(df.describe())

```

```shell

# print(df["Age"].max()) 의 결과 

58

# print(df.describe()) 의 결과 

             Age
count   3.000000
mean   38.333333
std    18.230012
min    22.000000
25%    28.500000
50%    35.000000
75%    46.500000
max    58.000000

```