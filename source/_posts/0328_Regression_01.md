---
title: "k-최근접 이웃 회귀 01"
author: "woobin"
date: '2022-03-28'
---

# k-최근접 이웃 회귀(Regression)
- 중요도: 하 (그냥 넘어갈것)

# 데이터 준비


```python
import numpy as np

perch_length = np.array(
    [8.4, 13.7, 15.0, 16.2, 17.4, 18.0, 18.7, 19.0, 19.6, 20.0, 
     21.0, 21.0, 21.0, 21.3, 22.0, 22.0, 22.0, 22.0, 22.0, 22.5, 
     22.5, 22.7, 23.0, 23.5, 24.0, 24.0, 24.6, 25.0, 25.6, 26.5, 
     27.3, 27.5, 27.5, 27.5, 28.0, 28.7, 30.0, 32.8, 34.5, 35.0, 
     36.5, 36.0, 37.0, 37.0, 39.0, 39.0, 39.0, 40.0, 40.0, 40.0, 
     40.0, 42.0, 43.0, 43.0, 43.5, 44.0]
     )
perch_weight = np.array(
    [5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 
     110.0, 115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 
     130.0, 150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 
     197.0, 218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 
     514.0, 556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 
     820.0, 850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 
     1000.0, 1000.0]
     )
```

# 시각화
 - 객체지향으로 변경할것
  - fig, ax = plt.subplots()
  - ax.scatter(Xdata, Ydata)


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.scatter(perch_length, perch_weight)
ax.set_xlabel("length")
ax.set_ylabel("weight")
plt.show()
```


    
![png](/Images/0328_Regression_01/output_4_0.png)
    


# 훈련 데이터 테스트 데이터셋 분리


```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(
    perch_length, perch_weight, random_state = 42
)
train_input.shape, test_input.shape, train_target.shape, test_target.shape
```




    ((42,), (14,), (42,), (14,))



- reshape() 사용하여 2차원 배열로 바꿈


```python
train_input= train_input.reshape(-1, 1)
test_input= test_input.reshape(-1, 1)

print(train_input.shape, test_input.shape)
```

    (42, 1) (14, 1)
    

# 결정계수 R^2
- 모델이 얼마만큼 정확한가
  - 99.3%
- 절댓값은 아님 ==> 상대적인 값임


```python
from sklearn.neighbors import KNeighborsRegressor

# 클래스 불러오기
knr = KNeighborsRegressor()

#  모형학습
knr.fit(train_input, train_target)

# 테스트 점수 확인
knr.score(test_input, test_target)
```




    0.992809406101064



# MAE
- 타깃과 얘측의 절댓값 오차를 평균하여 반환


```python
from sklearn.metrics import mean_absolute_error

# 예측 데이터 만들기
test_prediction = knr.predict(test_input)
test_prediction
```




    array([  60. ,   79.6,  248. ,  122. ,  136. ,  847. ,  311.4,  183.4,
            847. ,  113. , 1010. ,   60. ,  248. ,  248. ])




```python
mae = mean_absolute_error(test_target, test_prediction)
print(mae)
# 평균적으로 19g정도 다름
```

    19.157142857142862
    

# 과대적합 vs 과소적합
- 공통점: 머신러닝 모형이 실제 테스트 시 잘 예측을 못함.
- 차이점:
  - 과대적합: 훈련데이터에는 예측 잘함, 테스트데이터에서는 예측을 잘 못함.
    - 처리하기 곤란.
  - 과소적합: 훈련데이터에서는 예측을 못하고, 테스트데이터에서는 예측을 잘하거나 or 둘 다 예측을 잘 못함.
    - 데이터양이 적거나, 모델을 너무 간단하게 만듬


```python
# 훈련 데이터 점수 확인
knr.score(train_input, train_target)  # 0.97정도 나옴
```




    0.9698823289099254




```python
# Default 5를 3으로 변경
# 머신러닝 모형을 변경
knr.n_neighbors = 3

# 모델을 다시 훈련
knr.fit(train_input, train_target)
print(knr.score(train_input, train_target))  # 0.98정도 나옴
```

    0.9804899950518966
    


```python
print(knr.score(test_input, test_target))  # 0.97
```

    0.9746459963987609
    

- MAE구하기


```python
# 예측 데이터 만들기
test_prediction = knr.predict(test_input)
mae = mean_absolute_error(test_target, test_prediction)
# test_prediction
print(mae)  # 35.4정도 나옴
```

    35.42380952380951
    


```python
# 머신러닝 모형을 변경
knr.n_neighbors = 7

# 모델을 다시 훈련
knr.fit(train_input, train_target)
print(knr.score(train_input, train_target))  # 
print(knr.score(test_input, test_target))  # 
```

    0.9761170732051527
    0.9781383949643516
    


```python
# 예측 데이터 만들기
test_prediction = knr.predict(test_input)
mae = mean_absolute_error(test_target, test_prediction)
print(mae)  # 
```

    32.512244897959185
    

# 결론
-  k그룹을 5로 했을때 R2는 0.98 mae는 19정도 였음
-  k그룹을 3으로 했을때 R2는 0.97 mae는 35.4정도 였음
-  k그룹을 7로 했을때 R2는 0.98 mae는 32.5정도 였음

# 내맘대로 코딩
  - R^2와 mae의 값을 찾기


```python
TEST = np.arange(1, 10)
# float R0 = 0
for i in TEST:
  knr.n_neighbors = i
  knr.fit(train_input, train_target)
  print(knr.score(test_input, test_target))

  R2= knr.score(test_input, test_target)
  test_prediction = knr.predict(test_input)
  mae = mean_absolute_error(test_target, test_prediction)
  print(mae, i)
  
```

    0.991309195814175
    22.685714285714287 1
    0.9725010241788556
    35.292857142857144 2
    0.9746459963987609
    35.42380952380951 3
    0.9840231023848637
    28.38214285714286 4
    0.992809406101064
    19.157142857142862 5
    0.9855001139899048
    28.388095238095243 6
    0.9781383949643516
    32.512244897959185 7
    0.9780541148735824
    34.88214285714286 8
    0.9692647749722698
    43.987301587301594 9
    

# 마무리 정리
- 키워드
  - 회귀: 임의의 수치를 예측하는 문제
  - k-최근접 이웃 회귀: 가장 가까운 이웃 샘플을 찾고 그 샘플들의 평균으로 예측
  - 결정계수(R²)대표적인 회귀문제의 성능 측정 도구. 1에 가까울수록 좋고 0에 가까울수록 나쁜 모델임
  - 과대적합: 훈련데이터에는 예측 잘함, 테스트데이터에서는 예측을 잘 못함
  - 과소적합: 훈련데이터에서는 예측을 못하고, 테스트데이터에서는 예측을 잘하거나 or 둘 다 예측을 잘 못함
- Scikit-learn 패키지
  - KNeighborsRegressor: k-최근접 이웃 회귀 모델을 만드는 사이킷런 클래스. n_neighbors 매개변수를 지정함. [default: 5]
  - mean_absolute_error(A, B): 회귀모델의 평균 절댓값 오차를 계산. A는 타깃, B는 예측값. 타깃과 예측을 뺀 값을 제곱한 다음 전체 샘플에 대해 평균한 값을 반환함.
- Numpy 패키지
  - reshape(): 배열의 크기를 바꾸는 메서드. -1은 적용 가능한 정수를 자동으로 찾아주는 역할
