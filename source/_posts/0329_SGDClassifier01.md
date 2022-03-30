---
title: "확률적 경사 하강법 01"
author: "woobin"
date: '2022-03-29'
---

# 경사 하강법이 쓰인 여러 알고리즘
        - (이미지, 택스트) 딥러닝 기초 알고리즘
        - 트리 알고리즘 + 경사하강법 융합 = 부스팅계열
          - LightGBM, Xgboost, Catboost
            ex 1등으로 자주 쓰인 알고리즘: LightGBM, Xgboost
            - 하이퍼 파라미터의 개수가 80개가 넘음

# SGDClassifier
- 확률적 경사하강법 분류기
- 1회에 1개의 데이터씩 뽑기: 확률적 경사 하강법
- 1회에 여러개의 데이터씩 뽑기: 미니배치 경사 하강법
- 1회에 모든 데이터를 뽑기: 배치 경사 하강법

- 에포크: 훈련세트를 한번 모두 사용하는 과정


```python
import pandas as pd
fish = pd.read_csv("https://bit.ly/fish_csv_data")
fish.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 159 entries, 0 to 158
    Data columns (total 6 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Species   159 non-null    object 
     1   Weight    159 non-null    float64
     2   Length    159 non-null    float64
     3   Diagonal  159 non-null    float64
     4   Height    159 non-null    float64
     5   Width     159 non-null    float64
    dtypes: float64(5), object(1)
    memory usage: 7.6+ KB
    

- 배열로 변환하는 코드
  - 독립변수 = fish_input
  - 종속변수 = fish_target


```python
fish_input = fish[["Weight", "Length", "Diagonal", "Height", "Width"]].to_numpy()
fish_target= fish["Species"].to_numpy()
```

- 훈련세트와 테스트세트로 분리


```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(
    fish_input, fish_target, random_state= 42
)

train_input.shape, test_input.shape, train_target.shape, test_target.shape,
```




    ((119, 5), (40, 5), (119,), (40,))



- 표준화 처리
  - 다시 한번 강조하지만 꼭 훈련 세트로 학습한 통계값으로 테스트 세트도 변환한다.
  - 키워드: Data Leakage 방지 (나중에 찾아볼것)


```python
from sklearn.preprocessing import StandardScaler
ss = StandardScaler()
ss.fit(train_input)  # 변환 전 훈련세트 학습

# ss훈련 데이터만 활용해서 학습이 끝난 상태
# 표준화 처리를 훈련데이터와 테스트데이터에 동시 적용
train_scaled = ss.transform(train_input)  # 변환
test_scaled = ss.transform(test_input)  # 변환
```

# 모델 학습
- 2개의 매개 변수 지정
- loss= "log"=  로지스틱 손실 함수로 지정
- max_iter = 에포크 횟수 지정


```python
from sklearn.linear_model import SGDClassifier

# 매개변수 지정
# 하이퍼파라미터 설정
# 매개변수 값을 dictionary 형태로 추가하는 코드 작성 가능
# 입문자들에게는 비추천
sc = SGDClassifier(loss="log", max_iter= 100, random_state= 42)
sc.fit(train_scaled, train_target)
print(sc.score(train_scaled, train_target))
print(sc.score(test_scaled, test_target))
```

    0.8403361344537815
    0.8
    

- 적절한 에포크 숫자 찾기


```python
import numpy as np
sc = SGDClassifier(loss= "log", max_iter=100, tol=None, random_state= 42)
train_score=[]
test_score= []
classes = np.unique(train_target)

for _ in range(0,300):  # 에포크값 0 ~ 300  , # "_"도 변수가 될 수 있음
  sc.partial_fit(train_scaled, train_target, classes= classes)  # 훈련
  train_score.append(sc.score(train_scaled, train_target))  # 점수로 나오는 값들을 append()
  test_score.append(sc.score(test_scaled, test_target))

print(train_score[:5])
print(test_score[:5])
```

    [0.5294117647058824, 0.6218487394957983, 0.6386554621848739, 0.7310924369747899, 0.7226890756302521]
    [0.65, 0.55, 0.575, 0.7, 0.7]
    

- 모형학습 시각화


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.plot(train_score)
ax.plot(test_score)
ax.set_xlabel("Epoch")
ax.set_ylabel("accuracy")
plt.show()
```


    
![png](/Images/0329_SGDClassifier01/output_14_0.png)
    



```python
sc = SGDClassifier(loss="log", max_iter=100, tol=None, random_state=42)  # 반복횟수 100 , # tol(): 반복을 멈추는 기준
sc.fit(train_scaled, train_target)
print(sc.score(train_scaled, train_target))
print(sc.score(test_scaled, test_target))
```

    0.957983193277311
    0.925
    

# 마무리 정리
- 키워드
  - 확률적 경사 하강법: 훈련세트에서 샘플 하나씩 꺼내 손실 함수의 경사를 따라 최적의 모델링을 찾는 알고리즘
  - 손실함수: 확률적 경사 하강법이 최적화 할 대상
    - ex) 이진분류에는 로지스틱 회귀 손실함수를 사용
  - 에포크: 확률적 경사 하강법에서 전체 샘플을 모두 사용하는 한 번 반복을 의미
- Scikit-Learn 패키지
  - SGDClassifier: 확률적 경사 하강법을 사용한 분류모델을 만듬
    - 매개변수 loss: 최적화 할 손실함수를 지정. 로지스틱 회귀를 위해서는 log로 지정. [default: hinge](서포트 벡터 머신)
    - 매개변수 max_iter: 에포크 횟수를 지정 [default: 1000]
    - 매개변수 tol: 반복을 멈출 조건 [default: 0.001]
  - SGDRegressor: 확률적 경사 하강법을 사용한 회귀모델을 만듬
    - 매개변수 loss: 손실함수를 지정 [default: squared_loss](제곱오차를 나타냄)
    - 매개변수 max_iter: 에포크 횟수를 지정 [default: 1000]
    - 매개변수 tol: 반복을 멈출 조건 [default: 0.001]
