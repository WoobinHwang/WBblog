---
title: "k-최근접 이웃 회귀 02"
author: "woobin"
date: '2022-03-28'
---

# 선형회귀

# 데이터 불러오기


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
    

# 모델만들기


```python
from sklearn.neighbors import KNeighborsRegressor

# 클래스 불러오기
knr = KNeighborsRegressor(n_neighbors=3)

#  모형학습
knr.fit(train_input, train_target)
```




    KNeighborsRegressor(n_neighbors=3)




```python
print(knr.predict([[50]]))
```

    [1033.33333333]
    

# 시각화


```python
# # 원본
# import matplotlib.pyplot as plt

# # 50cm 농어의 이웃을 구하기
# distances, indexes= knr.kneighbors([[50]])

# # 훈련세트의 산점도를 그리기
# plt.scatter(train_input, train_target)

# # 훈련 세트 중에서 이웃 샘플만 다시 그립니다.
# plt.scatter(train_input[indexes], train_target[indexes], marker= "D")

# # 50cm 농어 데이터
# plt.scatter(50, 1033, marker= "^")
# plt.xlabel("length")
# plt.show()
```


```python
import matplotlib.pyplot as plt

# 50cm 농어의 이웃을 구하기
distances, indexes= knr.kneighbors([[50]])

# 훈련세트의 산점도를 그리기
# plt.scatter(train_input, train_target)
fig, ax = plt.subplots()
ax.scatter(train_input, train_target)

# 훈련 세트 중에서 이웃 샘플만 다시 그립니다.
ax.scatter(train_input[indexes], train_target[indexes], marker= "D")

# 50cm 농어 데이터
ax.scatter(50, 1033, marker= "^")
ax.set_xlabel("length")
ax.set_ylabel("weight")
plt.show()
```


    
![png](/Images/0328_Regression_02/output_12_0.png)
    


- 머신러닝 모델은 주기적으로 훈련해야 합니다.
   - MLOps(Machine Learning & Operations)
   - 최근에 각광받는 데이터 관련 직업 필수
   - 입사와 함께 공부시작 (데이터 분석가, 머신러닝 엔지니어, 데이터 싸이언티스트 희망자)

# 선형회귀 (머신러닝)
- 평가지표 확이이 더 중요 - R2점수, MAE, MSE, 등등

# 선형회귀 (통계)
- 5가지 가정들
  - 장차의 정규성, 등분상성, 다중공선성, 등등...
  - 종속변수 ~ 독립변수간의 "인과관계"를 찾는 과정


```python
from sklearn.linear_model import LinearRegression

lr = LinearRegression()

# 선형 회귀 모델 훈련
lr.fit(train_input, train_target)

print(lr.predict([[50]]))
```

    [1241.83860323]
    


```python
import matplotlib.pyplot as plt
fig, ax= plt.subplots()
ax.scatter(train_input, train_target)
ax.scatter(train_input[indexes], train_target[indexes], marker= "D")
ax.scatter(50, 1241, marker= "^")
ax.set_xlabel("length")
ax.set_ylabel("weight")
plt.show()
```


    
![png](/Images/0328_Regression_02/output_16_0.png)
    


    - 농어무게 = 기울기(a) * 농어 길이 + 절편(b)

# 회귀식 찾기
- 기울기
- 절편


```python
# 기울기, 절편
print(lr.coef_, lr.intercept_)
```

    [39.01714496] -709.0186449535477
    

    - 기울기 a: 39.01714496
    - 절편 b: 709.0186449535477

- **기울기**: 계수 = 가중치 (딥러닝)


```python
import matplotlib.pyplot as plt

fig, ax= plt.subplots()
ax.scatter(train_input, train_target)

# 15~50까지의 1차 방정식 그래프
ax.plot([15,50],
        [15 * lr.coef_ + lr.intercept_,
         50 * lr.coef_ + lr.intercept_],)

plt.scatter(50, 1241.8, marker= "^")

plt.show()
```


    
![png](/Images/0328_Regression_02/output_22_0.png)
    


- 모형 평가
  - 과대적합 발생

# 다항회귀
- ax^2 + by + c
- if) 치어 1cm
  - -670g이 나옴


```python
print(lr.predict([[1]]))
```

    [-670.00149999]
    

- 1차 방정식을 2차 방정식으로 만드는 과정 필요
- 넘파이 브로드캐스팅
  - 배열의 크기가 동일하면 상관없음
  - 배열의 크기가 다른데 연산을 할 때 브로드캐스팅 원리가 적용됨.


```python
train_poly = np.column_stack((train_input **2, train_input))
test_poly = np.column_stack((test_input **2, test_input))

print(train_poly.shape, test_poly.shape)
```

    (42, 2) (14, 2)
    


```python
lr = LinearRegression()

lr.fit(train_poly, train_target)

print(lr.predict([[50**2,50]]))
```

    [1573.98423528]
    


```python
print(lr.coef_, lr.intercept_)
```

    [  1.01433211 -21.55792498] 116.0502107827827
    

-knn의 문제점
  - 농어의 길이가 커져도 무게가 동일함
  - 현실성없음
- 단순 선형회귀(1차방정식)의 문제점
  - 치어(1cm)의 무게가 음수로 나옴
  - 현실성없음
- 다항회귀(2차 방정식)로 변경
  - 현실성 있음

# 마무리 정리
- 키워드
  - 선형회귀: 특성과 타깃 사이의 관계를 가장 잘 나타내는 선형 방정식을 찾음. 특성이 하나면 직선 방정식이 됨.
  - 모델 파라미터: 선형회귀가 찾은 가중치처럼 머신러닝 모델이 특성에서 학습한 파라미터
  - 다항 회귀: 다항식을 사용하여 특성과 타깃 사이의 관계를 나타냄
- Scikit-learn 패키지
  - LinearRegression: 선형 회귀 클래스
