---
title: "k-최근접 이웃 회귀 03"
author: "woobin"
date: '2022-03-28'
---

# 특성 공학과 규제

- 다중회귀:
  - 여러가지 특성을 사용한 선형회귀
- 특성공학:
  - 기존의 특성을 사용해 새로운 특성을 뽑아내는 작업

# 데이터 준비
- 판다스: 유명한 데이터 분석 라이브러리
  - 데이터프레임: 판다스의 핵심 데이터 구조


```python
import pandas as pd
# pd.__version__
df = pd.read_csv("https://bit.ly/perch_csv_data")
perch_full = df.to_numpy()
# print(perch_full)
```


```python
# https://gist.github.com/rickiepark/2cd82455e985001542047d7d55d50630

import numpy as np
perch_weight = np.array([5.9, 32.0, 40.0, 51.5, 70.0, 100.0, 78.0, 80.0, 85.0, 85.0, 110.0,
       115.0, 125.0, 130.0, 120.0, 120.0, 130.0, 135.0, 110.0, 130.0,
       150.0, 145.0, 150.0, 170.0, 225.0, 145.0, 188.0, 180.0, 197.0,
       218.0, 300.0, 260.0, 265.0, 250.0, 250.0, 300.0, 320.0, 514.0,
       556.0, 840.0, 685.0, 700.0, 700.0, 690.0, 900.0, 650.0, 820.0,
       850.0, 900.0, 1015.0, 820.0, 1100.0, 1000.0, 1100.0, 1000.0,
       1000.0])
```

- perch_full과 perch_weight를 훈련세트와 테스트 세트로 나눔


```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(perch_full, perch_weight, random_state= 42)
print(train_target)
```

    [  85.  135.   78.   70.  700.  180.  850.  820. 1000.  120.   85.  130.
      225.  260. 1100.  900.  145.  115.  265. 1015.  514.  218.  685.   32.
      145.   40.  690.  840.  300.  170.  650.  110.  150.  110. 1000.  150.
       80.  700.  120.  197. 1100.  556.]
    

# 사이킷런의 변환기
- 변환기: 특성을 만들거나 전처리하기 위한 다양한 클래스들
  - fit(), score(), predict(), transform(), 등등...


```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures()
poly.fit([[2,3]])  # 변환 전 학습시키기
print(poly.transform([[2,3]]))  # 2,3의 특성을 변환시킨것
```

    [[1. 2. 3. 4. 6. 9.]]
    

      - 2와 3을 곱한 6, 각각 제곱한 4, 9, 절편인 1이 추가되었다.

- 사이킷런 특성상 절편을 자동으로 추가하므로 1은 필요 없음.
  - include_bias= False로 제거


```python
poly = PolynomialFeatures(include_bias= False)
poly.fit([[2,3]])
print(poly.transform([[2, 3]]))
```

    [[2. 3. 4. 6. 9.]]
    

- train_input에 적용


```python
poly = PolynomialFeatures(include_bias= False)  # 1은 제외
poly.fit(train_input)  # 변환 전 학습시키기
train_poly = poly.transform(train_input)  # 변환
print(train_poly.shape)
# print(train_poly)
```

    (42, 9)
    

- PolynomiaFeatures클래스의 get_feature_names_/Images/0328_Regression_03/out()의 메서드로 9개의 특성이 각각 어떤 입력의 조합으로 만들어졌는지 알 수 있다.


```python
poly.get_feature_names_/Images/0328_Regression_03/out()
```




    array(['x0', 'x1', 'x2', 'x0^2', 'x0 x1', 'x0 x2', 'x1^2', 'x1 x2',
           'x2^2'], dtype=object)



      각각 x0, x1, x2에서 파생된 값들


```python
test_poly= poly.transform(test_input)  # test_input 을 변환
# print(test_poly)
```

      poly: 2,3을 transform한 값
      train_poly: train_input을 transform한 값
      test_poly: test_input을 transform한 값

# 다중회귀 모델 훈련


```python
from sklearn.linear_model import LinearRegression
lr = LinearRegression()
lr.fit(train_poly, train_target)  # fit(): 훈련시키기
print(lr.score(train_poly, train_target))  # 정답률 99퍼센트
```

    0.9903183436982124
    


```python
print(lr.score(test_poly, test_target))  # 정답률 97퍼센트
```

    0.9714559911594134
    


```python
poly = PolynomialFeatures(degree= 5, include_bias= False)  # 5제곱까지 특성을 만들어 출력, 1은 제외
poly.fit(train_input)  # 변환 전 학습시키기
train_poly = poly.transform(train_input)  # 변환시키기
test_poly = poly.transform(test_input)  # 변환시키기
print(train_poly.shape)
```

    (42, 55)
    


```python
lr.fit(train_poly, train_target)  # fit(): 훈련시키기
print(lr.score(train_poly, train_target))  # 정답률 100퍼센트에 가까움
```

    0.9999999999991097
    


```python
print(lr.score(test_poly, test_target))  # 점수가 음수로 나옴
```

    -144.40579242684848
    

      144로 음수가 나와 과대적합이 되었음.
      특성을 줄일 필요가 있음.

# 규제(Regularization)
- 머신러닝 모델이 훈련세트를 너무 과도하게 학습하지 못하도록 훼방하는 것


```python
# 정규화 우선
from sklearn.preprocessing import StandardScaler
ss = StandardScaler()
ss.fit(train_poly)  # 변환 전 학습시키기 
train_scaled = ss.transform(train_poly)  # 정규화
test_scaled = ss.transform(test_poly)  # 정규화
```

# 릿지(Ridge)와 라쏘(Lasso)
- 선형 회귀모델에 규제를 추가한 모델

## 릿지 회귀 (Ridge)


```python
from sklearn.linear_model import Ridge
ridge = Ridge()
ridge.fit(train_scaled, train_target)  # fit()로 학습시키기
print(ridge.score(train_scaled, train_target))  # 0.99의 점수가 나옴
print(ridge.score(test_scaled, test_target))  # 점수가 정상으로 돌아옴
```

    0.9896101671037343
    0.9790693977615397
    

- 릿지와 라쏘 모델을 사용 할 때 alpha매개변수로 규제의 강도를 조절 가능
  -  alpha값이 크면 규제강도가 세지므로 과소적합되도록 유도
  -  alpha값이 작으면 규제강도가 세지므로 과대적합되도록 유도

- 적절한 alpha값을 찾는 방법중 하나로는 alpha값에 대한 R^2값의 그래프를 그려보는것.
  - 훈련세트와 테스트 세트의 점수가 가장 가까운 지점이 최적의 alpha값


```python
train_score = []
test_score = []

# alpha값을 0.001~100까지 10배씩 늘리며 회귀모델을 훈련
alpha_list = [0.001, 0.01, 0.1, 1, 10, 100]
for alpha in alpha_list:
  # 릿지모델을 만듬
  ridge = Ridge(alpha=alpha)

  # 릿지모델을 훈련
  ridge.fit(train_scaled, train_target)

  # 훈련 점수와 테스트 점수를 저장
  train_score.append(ridge.score(train_scaled, train_target))
  test_score.append(ridge.score(test_scaled, test_target))
```


```python
# 로그함수로 나타내어 그래프 그리기
# 객체지향 함수로 진행
import matplotlib.pyplot as plt
fig, ax= plt.subplots()
ax.plot(np.log10(alpha_list), train_score)  # 위에 그려질 그래프
ax.plot(np.log10(alpha_list), test_score)  # 아래에 그려질 그래프
ax.set_xlabel("alpha")  # x축은 지수를 나타냄
ax.set_ylabel("R^2")  # == score의 점수
plt.show()
```


    
![png](/Images/0328_Regression_03/output_34_0.png)
    


      0.1인 -1에서 가장 가깝고 결정계수가 가장 높기때문에 alpha로 선정



```python
ridge = Ridge(alpha=0.1)
ridge.fit(train_scaled, train_target)
print(ridge.score(train_scaled, train_target))
print(ridge.score(test_scaled, test_target))  # 둘이 비슷하게 높은 점수가 나옴.
```

    0.9903815817570366
    0.9827976465386926
    

# 라쏘 (Lasso)


```python
from sklearn.linear_model import Lasso
lasso = Lasso()
lasso.fit(train_scaled, train_target)  # fit()로 학습시키기
print(lasso.score(train_scaled, train_target))  # 0.99의 점수가 나옴
print(lasso.score(test_scaled, test_target))  # 점수가 정상으로 돌아옴
```

    0.989789897208096
    0.9800593698421883
    

- 적절한 alpha 값 찾기


```python
train_score = []
test_score = []

# alpha값을 0.001~100까지 10배씩 늘리며 회귀모델을 훈련
alpha_list = [0.001, 0.01, 0.1, 1, 10, 100]
for alpha in alpha_list:
  # 라쏘모델을 만듬
  lasso = Lasso(alpha=alpha, max_iter=10000)

  # 라쏘모델을 훈련
  lasso.fit(train_scaled, train_target)

  # 훈련 점수와 테스트 점수를 저장
  train_score.append(lasso.score(train_scaled, train_target))
  test_score.append(lasso.score(test_scaled, test_target))
```

    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_coordinate_descent.py:648: ConvergenceWarning: Objective did not converge. You might want to increase the number of iterations, check the scale of the features or consider increasing regularisation. Duality gap: 1.878e+04, tolerance: 5.183e+02
      coef_, l1_reg, l2_reg, X, y, max_iter, tol, rng, random, positive
    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_coordinate_descent.py:648: ConvergenceWarning: Objective did not converge. You might want to increase the number of iterations, check the scale of the features or consider increasing regularisation. Duality gap: 1.297e+04, tolerance: 5.183e+02
      coef_, l1_reg, l2_reg, X, y, max_iter, tol, rng, random, positive
    


```python
# 로그함수로 나타내어 그래프 그리기
# 객체지향 함수로 진행
import matplotlib.pyplot as plt
fig, ax= plt.subplots()
ax.plot(np.log10(alpha_list), train_score)  # 위에 그려질 그래프
ax.plot(np.log10(alpha_list), test_score)  # 아래에 그려질 그래프
ax.set_xlabel("alpha")  # x축은 지수를 나타냄
ax.set_ylabel("R^2")  # == score의 점수
plt.show()
```


    
![png](/Images/0328_Regression_03/output_41_0.png)
    


      alpha가 2인 값은 훈련세트와 테스트세트의 점수가 좁음
      하지만 결정계수가 낮기때문에 10을 의미하는 1이 최적의 alpha값임. 


```python
lasso = Lasso(alpha=0.1)
lasso.fit(train_scaled, train_target)
print(lasso.score(train_scaled, train_target))
print(lasso.score(test_scaled, test_target))  # 라쏘 또한 둘이 비슷하게 높은 점수가 나옴.
```

    0.990137631128448
    0.9819405116249363
    

    /usr/local/lib/python3.7/dist-packages/sklearn/linear_model/_coordinate_descent.py:648: ConvergenceWarning: Objective did not converge. You might want to increase the number of iterations, check the scale of the features or consider increasing regularisation. Duality gap: 8.062e+02, tolerance: 5.183e+02
      coef_, l1_reg, l2_reg, X, y, max_iter, tol, rng, random, positive
    

- 라쏘모델의 계수값을 아예 0으로 만들 수 있음.
- 라쏘 모델의 계수는 coef_ 속성에 저장되어 있음.


```python
print(np.sum(lasso.coef_ == 0))
```

    52
    

# 마무리 정리
- 키워드
  - 다중회귀: 여러개의 특성을 사용하는 회귀모델
  - 특성공학: 주어진 특성을 조합하여 새로운 특성을 만드는 일련의 작업과정
  - 릿지: 규제가 있는 선형 회귀모델중 하나이며 선형 모델의 계수를 작게 만들어 과대적합을 완화시킴
  - 라쏘: 또 다른 규제가 있는 선형회귀 모델. 릿지와 달리 계수값을 0으로 만들 수 있음.
  - 하이퍼파라미터: 머신러닝 알고리즘이 학습하지 않는 파라미터. 사람이 지정해야함. 대표적으로 릿지와 라쏘의 규제 강도 alpha파라미터가 있음.

- Pandas 패키지
  - 유명한 데이터 분석 라이브러리
  - read_csv(): 로컬 컴퓨터나 인터넷에 있는 csv파일을 읽어 판다스 데이터프레임으로 변환하는 함수

- Scikit-learn 패키지
  - PolynomialFeatures: 주어진 특성을 조합하여 새로운 특성을 만듬
    - degree: 최고 차수를 지정함. [default: 2]
    - interactopm_only: True이면 거듭제곱항은 제외되고 특성간의 곱셈항만 추가됨. [default: False]
    - include_bias: False이면 절편을 위한 특성을 추가하지 않음. [default: True]
  - Ridge: 규제가 있는 회귀 알고리즘인 릿지 회귀모델을 훈련함.
    - alpha: 매개변수로 규제의 강도를 조절함. 값이 클수록 규제가 강함. [default: 1]
  - Lasso: 규제가 있는 회귀 알고리즘인 라쏘 회귀모델을 훈련함. 최적의 모델을 찾기 위해 좌표축을 따라 최적화를 수행해가는 좌표 하강법을 사용
    - alpha와 random_state매개변수는 Ridge클래스와 동일
    - max_iter: 알고리즘의 수행 반복 횟수를 지정함. [default: 1000]
