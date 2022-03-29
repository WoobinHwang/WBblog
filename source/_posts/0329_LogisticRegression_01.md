---
title: "로지스틱 회귀 01"
author: "woobin"
date: '2022-03-29'
---

# 로지스틱 회귀(Logistic Regression)

# 데이터 불러오기


```python
import pandas as pd
import numpy

fish = pd.read_csv('https://bit.ly/fish_csv_data')
fish.head()
```





  <div id="df-933200f5-54dd-47e4-9481-6ae409c19d41">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Species</th>
      <th>Weight</th>
      <th>Length</th>
      <th>Diagonal</th>
      <th>Height</th>
      <th>Width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bream</td>
      <td>242.0</td>
      <td>25.4</td>
      <td>30.0</td>
      <td>11.5200</td>
      <td>4.0200</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bream</td>
      <td>290.0</td>
      <td>26.3</td>
      <td>31.2</td>
      <td>12.4800</td>
      <td>4.3056</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bream</td>
      <td>340.0</td>
      <td>26.5</td>
      <td>31.1</td>
      <td>12.3778</td>
      <td>4.6961</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bream</td>
      <td>363.0</td>
      <td>29.0</td>
      <td>33.5</td>
      <td>12.7300</td>
      <td>4.4555</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bream</td>
      <td>430.0</td>
      <td>29.0</td>
      <td>34.0</td>
      <td>12.4440</td>
      <td>5.1340</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-933200f5-54dd-47e4-9481-6ae409c19d41')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-933200f5-54dd-47e4-9481-6ae409c19d41 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-933200f5-54dd-47e4-9481-6ae409c19d41');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>




# 데이터 변환
- 배열로 변환
- to_numpy(): 이용하여 넘파이 배열로 새로 저장


```python
fish_input = fish[["Weight", "Length", "Diagonal", "Height", "Width"]].to_numpy()  # 특성별로 구분해서 저장
print(fish_input[:5])
fish_input.shape
```

    [[242.      25.4     30.      11.52     4.02  ]
     [290.      26.3     31.2     12.48     4.3056]
     [340.      26.5     31.1     12.3778   4.6961]
     [363.      29.      33.5     12.73     4.4555]
     [430.      29.      34.      12.444    5.134 ]]
    




    (159, 5)



- 종류를 target 배열로 변환
- 종속변수


```python
fish_target= fish["Species"].to_numpy()
# fish_target.shape
```

# 훈련 데이터와 테스트 데이터


```python
# from sklearn.model_selection import train_test_split
# train_input, test_input, train_target, test_target = train_test_split(fish_input, fish_target, random_state= 42)
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(
    fish_input, fish_target, random_state= 42
)
```

- 표준화 전처리


```python
from sklearn.preprocessing import StandardScaler

ss = StandardScaler()
ss.fit(train_input)  # 변환 전 훈련
train_scaled = ss.transform(train_input)  # 변환
test_scaled = ss.transform(test_input)  # 변환
test_scaled[:5]
```




    array([[-0.88741352, -0.91804565, -1.03098914, -0.90464451, -0.80762518],
           [-1.06924656, -1.50842035, -1.54345461, -1.58849582, -1.93803151],
           [-0.54401367,  0.35641402,  0.30663259, -0.8135697 , -0.65388895],
           [-0.34698097, -0.23396068, -0.22320459, -0.11905019, -0.12233464],
           [-0.68475132, -0.51509149, -0.58801052, -0.8998784 , -0.50124996]])



k최근접 이웃 분류기 확률 예측


```python
from sklearn.neighbors import KNeighborsClassifier

kn = KNeighborsClassifier(n_neighbors=3)  # 3개의 이웃 데이터의 평균으로 유추
kn.fit(train_scaled, train_target)  # 흔랸

print(kn.score(train_scaled, train_target))  # 훈련 후 점수 확인
print(kn.score(test_scaled, test_target))
```

    0.8907563025210085
    0.85
    

182p


```python
import numpy as np
proba = kn.predict_proba(test_scaled[:5])
print(np.round(proba, decimals= 4))
print(kn.classes_)
```

    [[0.     0.     1.     0.     0.     0.     0.    ]
     [0.     0.     0.     0.     0.     1.     0.    ]
     [0.     0.     0.     1.     0.     0.     0.    ]
     [0.     0.     0.6667 0.     0.3333 0.     0.    ]
     [0.     0.     0.6667 0.     0.3333 0.     0.    ]]
    ['Bream' 'Parkki' 'Perch' 'Pike' 'Roach' 'Smelt' 'Whitefish']
    

 # 로지스틱 회귀 (분류모델)
 - 중요도: 최상
 - *** 로지스틱 회귀의 이론이 나온 이유: ***
  - 범주형( True, False ) 의 경우 선형함수로 표현하기엔 오류가 발생하여 굴곡이 있는 모델을 제시함.
 - 유튜브 시청 필요
 - https://youtu.be/zASrGSHoqL4
  - 개념 재복습 필수
  - 중요한 이유:
    - 기초통계로도 활용(의학통계)
    - 머신러닝 분류모형의 기초 모형인데, 성능이 나쁘지 않음.
      - 데이터셋, 수치 데이터 기반
    - 딥러닝: 초기모형에 해당됨.
 - 시그모이드 함수:
  - z = phi = 1 / (1 + np.exp(-z))


```python
# 원본
# import numpy as np
# import matplotlib.pyplot as plt
# z = np.arange(-5, 5, 0.1)
# phi = 1 / (1 + np.exp(-z))
# # phi
# plt.plot(z, phi)
# plt.xlabel("z")
# plt.ylabel("phi")
# plt.show()
```


```python
import numpy as np
import matplotlib.pyplot as plt
z = np.arange(-5, 5, 0.1)  # -5와 5 사이에서 0.1간격으로 배열
phi = 1 / (1 + np.exp(-z))  # 지수함수 계산은 np.exp() 함수를 이용
fig, ax = plt.subplots()
ax.plot(z, phi)
ax.set_xlabel("z")
ax.set_ylabel("phi")
plt.show
```




    <function matplotlib.pyplot.show>




    
![png](output_17_1.png)
    


        시그모이드 함수의 출력이 0.5보다 크면 양성 클래스, 0.5보다 작으면 음성 클래스로 판단
        0.5일때는 라이브러리마다 다르지만 사이킷런은 음성클래스로 판단.

# 로지스틱 회귀로 이진 분류 수행하기


```python
char_arr = np.array(["A", "B", "C", "D", "E"])
print(char_arr[[True, False, True, False, False]])  # True, False로 원하는 데이터만 추출
```

    ['A' 'C']
    


```python
bream_smelt_indexes = (train_target == 'Bream') | (train_target == 'Smelt')  # | : OR연산자
train_bream_smelt = train_scaled[bream_smelt_indexes]
target_bream_smelt = train_target[bream_smelt_indexes]
target_bream_smelt
```




    array(['Bream', 'Smelt', 'Bream', 'Bream', 'Bream', 'Smelt', 'Bream',
           'Bream', 'Bream', 'Bream', 'Bream', 'Bream', 'Bream', 'Smelt',
           'Bream', 'Smelt', 'Smelt', 'Bream', 'Bream', 'Bream', 'Bream',
           'Bream', 'Bream', 'Bream', 'Bream', 'Smelt', 'Bream', 'Smelt',
           'Smelt', 'Bream', 'Smelt', 'Bream', 'Bream'], dtype=object)



- 186p
- 모형 만들고 예측하기!


```python
from sklearn.linear_model import LogisticRegression
lr = LogisticRegression()
#             독립변수          종속변수
lr.fit(train_bream_smelt, target_bream_smelt)
```




    LogisticRegression()




```python
# 예측하기
# 클래스로 분류
# 확률값 -> 0.5

print(lr.predict(train_bream_smelt[:5]))
```

    ['Bream' 'Smelt' 'Bream' 'Bream' 'Bream']
    


```python
print(lr.predict_proba(train_bream_smelt[:5]))
print(lr.classes_)  # smelt: 양성 클래스 , bream: 음성 클래스
```

    [[0.99759855 0.00240145]
     [0.02735183 0.97264817]
     [0.99486072 0.00513928]
     [0.98584202 0.01415798]
     [0.99767269 0.00232731]]
    ['Bream' 'Smelt']
    

- 방정식의 각 기울기와 상수를 구하는 코드


```python
# print(lr.coef_, lr.intercept_)
print(lr.coef_)
print(lr.intercept_)
```

    [[-0.4037798  -0.57620209 -0.66280298 -1.01290277 -0.73168947]]
    [-2.16155132]
    

- 결론 z식
        z = -0.4037798 * weight - 0.57620209 *length - -0.66280298 * diagonal -1.01290277 * height -0.73168947 * width - 2.16155132
        라는 식이 도출됨
- z값을 출력하자
- predict_proba()


```python
decisions = lr.decision_function(train_bream_smelt[:5])
print(decisions)
```

    [-6.02927744  3.57123907 -5.26568906 -4.24321775 -6.0607117 ]
    


```python
from scipy.special import expit
print(expit(decisions))
```

    [0.00240145 0.97264817 0.00513928 0.01415798 0.00232731]
    

# 마무리 정리
- 키워드
  - 로지스틱 회귀: 선형 방정식을 사용한 분류 알고리즘 (시그모이드를 사용하여 클래스 학률을 출력할수 있다.)
  - 다중분류
    - 타깃 클래스가 2개 이상인 분류 문제
  - 시그모이드:
    0~1사이의 값으로 

- Scikit-learn 패키지
  - LogisticRegression:
  선형분류의 알고리즘인 로지스틱 회귀를 위한 클래스
    - 매개변수 solver: 사용할 알고리즘을 선택 [default: lbfgs]
    - 매개변수 penalty: L2규제(릿지 방식)와 L1규제(라쏘 방식)를 선택 [default: l2]
    - 매개변수 C: 규제의 강도를 제어. 작을수록 규제가 강함 [default: 1.0]
  - predict_proba(): 예측 확률을 반환
  - decision_function(): 모델이 학습한 선형 방정식의 출력을 반환
