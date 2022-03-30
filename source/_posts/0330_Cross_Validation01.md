---
title: "교차검증과_랜덤서치_01"
author: "woobin"
date: '2022-03-30'
---

# 교차 검증과 그리드 서치
- 키워드: 하이퍼 파라미터
- 데이터가 작을 때, 주로 사용
- 하이퍼 파라미터
  - max_depth: 3일때, 정확도가 84%
- 결론
  - 모르면 default만 쓸것
  - 가성비 (시간 대비 성능 보장 안됨)

# 검증 세트
- 테스트 세트 (1회성)
- 훈련 데이터를 훈련데이터 + 검증 데이터로 재분할

## 현실
- 테스트 데이터가 별도로 존재안함
- ex) 전체 데이터 = 훈련(6) : 검증(2) : 테스트(2)
  - 테스트 데이터는 모르는 데이터로 간주


```python
import pandas as pd
wine = pd.read_csv("https://bit.ly/wine_csv_data")

data = wine[['alcohol', 'sugar', 'pH']].to_numpy()
target= wine['class'].to_numpy()
wine.head()
```





  <div id="df-7bbb17c4-c2ff-4a49-b897-7cc39b6d3337">
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
      <th>alcohol</th>
      <th>sugar</th>
      <th>pH</th>
      <th>class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9.4</td>
      <td>1.9</td>
      <td>3.51</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.8</td>
      <td>2.6</td>
      <td>3.20</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.8</td>
      <td>2.3</td>
      <td>3.26</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9.8</td>
      <td>1.9</td>
      <td>3.16</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9.4</td>
      <td>1.9</td>
      <td>3.51</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-7bbb17c4-c2ff-4a49-b897-7cc39b6d3337')"
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
          document.querySelector('#df-7bbb17c4-c2ff-4a49-b897-7cc39b6d3337 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-7bbb17c4-c2ff-4a49-b897-7cc39b6d3337');
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





```python
from sklearn.model_selection import train_test_split
train_input, test_input, train_target, test_target = train_test_split(
    data, target, test_size= 0.2, random_state= 42
)
print(train_input.shape, test_input.shape)
```

    (5197, 3) (1300, 3)
    


```python
# train 데이터의 20%를 val 데이터로 분류
sub_input, val_input, sub_target, val_target = train_test_split(
    train_input, train_target, test_size= 0.2, random_state= 42  # random_state: 추출값 고정이 목적
)
print(sub_input.shape, val_input.shape, val_target.shape)
```

    (4157, 3) (1040, 3) (1040,)
    


```python
from sklearn.tree import DecisionTreeClassifier
dt = DecisionTreeClassifier(random_state=42)
dt.fit(sub_input, sub_target)
print(dt.score(sub_input, sub_target))  # 0.9971133028626413
print(dt.score(val_input, val_target))  # 0.864423076923077
```

    0.9971133028626413
    0.864423076923077
    

# 교차검증
- 교차검증의 목적: 좋은 모델이 만들어진다!
  - 좋은 모델 = 과대적합이 아닌 모델 = 모형의 오차가 적은 모델 = 안정적인 모델
  - 성능 좋은 모델이 좋은 모델이란게 아님
- 교재 245p
  - 모델평가1: 90% (소요시간: 1시간)
  - 모델평가2: 85%
  - 모델평가3: 80%
- 단점: 시간이 오래 걸림

# 교차검증 함수


```python
from sklearn.model_selection import cross_validate
scores = cross_validate(dt, train_input, train_target)
print(scores)
```

    {'fit_time': array([0.0083735 , 0.00927544, 0.00848556, 0.00817442, 0.00816131]), 'score_time': array([0.00126314, 0.00097775, 0.00073361, 0.00077486, 0.00080872]), 'test_score': array([0.86923077, 0.84615385, 0.87680462, 0.84889317, 0.83541867])}
    

- 최종점수 평균 구하기


```python
import numpy as np
print(np.mean(scores['test_score']))
```

    0.855300214703487
    

- 훈련 세트 섞은 후, 10-폴드 교차검증


```python
from sklearn.model_selection import StratifiedKFold
splitter = StratifiedKFold(n_splits = 10, shuffle = True, random_state = 42)
scores = cross_validate(dt, train_input, train_target, cv= splitter)
print(scores['test_score'])
print(np.mean(scores['test_score']))
```

    [0.84807692 0.89423077 0.87115385 0.85576923 0.86346154 0.87884615
     0.87692308 0.86319846 0.87668593 0.87475915]
    0.8703105083740921
    

# 하이퍼파라미터 튜닝
- 그리드 서치 vs 랜덤 서치
- 꼭 사용하고 싶다면 -> 랜덤 서치 사용
- 자동으로 잡아주는 라이브러리 등장
  - hyperopt 등등...


```python
from pandas.core.common import random_state
from sklearn.model_selection import GridSearchCV
from sklearn.tree import DecisionTreeClassifier

params = {'min_impurity_decrease': [0.0001, 0.0002, 0.0003, 0.0004, 0.0005],
          # 'max_depth': [3, 4, 5, 6, 7]
}

# dt = DecisionTreeClassifier(random_state= 42)
gs = GridSearchCV(DecisionTreeClassifier(random_state= 42),params, n_jobs= -1)
gs.fit(train_input, train_target)
```




    GridSearchCV(estimator=DecisionTreeClassifier(random_state=42), n_jobs=-1,
                 param_grid={'min_impurity_decrease': [0.0001, 0.0002, 0.0003,
                                                       0.0004, 0.0005]})




```python
dt= gs.best_estimator_
print(dt)
print(dt.score(train_input, train_target))
print(gs.best_params_)
```

    DecisionTreeClassifier(min_impurity_decrease=0.0001, random_state=42)
    0.9615162593804117
    {'min_impurity_decrease': 0.0001}
    


```python
print(gs.cv_results_['mean_test_score'])
```

    [0.86819297 0.86453617 0.86492226 0.86780891 0.86761605]
    

# 랜덤서치
- 매개변수 값의 목록을 전달하는것이 아니라 매개변수를 샘플링 할 수 있도록 확률 분포 객체를 전달


```python
# scipy라이브러리: 적분, 보간, 선형대수, 확률 등을 포함한 수치 계산 전용으로 파이썬의 핵심 과학 라이브러리
from scipy.stats import uniform, randint  # randint: 정수값을 뽑음,  uniform: 실수값을 뽑음.
rgen = randint(0,10)
rgen.rvs(10)
```




    array([0, 4, 9, 5, 9, 9, 1, 0, 2, 1])




```python
np.unique(rgen.rvs(1000), return_counts= True)  # 각각 추출된 숫자의 갯수
```




    (array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]),
     array([ 99, 116,  90,  99,  87, 118, 104, 101,  81, 105]))




```python
ugen= uniform(0, 1)
ugen.rvs(10)
```




    array([0.4111007 , 0.57571785, 0.55554775, 0.58827729, 0.92876842,
           0.98869391, 0.87016065, 0.51721186, 0.54686086, 0.97925777])




```python
from sklearn.model_selection import RandomizedSearchCV

params = {
    'min_impurity_decrease': uniform(0.0001, 0.001),
    'max_depth': randint(20, 50),
    'min_samples_split': randint(2, 25),
    'min_samples_leaf': randint(1, 25),}
gs = RandomizedSearchCV(DecisionTreeClassifier(random_state= 42), params,  # n_iter: 파라미터 검색 횟수
                        n_iter= 100, n_jobs= -1, random_state= 42)  # n_jobs: cpu코어 수

gs.fit(train_input, train_target)
```




    RandomizedSearchCV(estimator=DecisionTreeClassifier(random_state=42),
                       n_iter=100, n_jobs=-1,
                       param_distributions={'max_depth': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7f6ec0604a90>,
                                            'min_impurity_decrease': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7f6ec0604910>,
                                            'min_samples_leaf': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7f6ec0604e90>,
                                            'min_samples_split': <scipy.stats._distn_infrastructure.rv_frozen object at 0x7f6ec0604a50>},
                       random_state=42)




```python
print(gs.best_params_)  # 최고의 교차검증 점수
```

    {'max_depth': 39, 'min_impurity_decrease': 0.00034102546602601173, 'min_samples_leaf': 7, 'min_samples_split': 13}
    


```python
# 검증 점수
print(np.max(gs.cv_results_['mean_test_score']))
```

    0.8695428296438884
    


```python
# 테스트 세트로 성능 확인
dt = gs.best_estimator_
print(dt.score(test_input, test_target))
```

    0.86
    

# 마무리 정리
- 키워드
  - 검증세트: 하이퍼파라미터 튜닝을 위해 모델을 평가 할 때, 테스트 세트를 사용하지 않기 위해 훈련세트에서 다시 떼어 낸 데이터 세트
  - 교차검증: 훈련세트를 여러 폴드로 나눈 다음 한 폴드가 검증 세트의 역할을 하고 나머지 폴드에서는 모델을 훈련
  - 그리드 서치: 하이퍼파라미터 탐색을 자동화해주는 도구. 탐색할 매개변수를 나열하면 교차 거증을 수행하여 가장 좋은 검증 점수의 매개변수 조합을 선택. 이 매개변수 조합으로 최종 모델을 훈련
  - 랜덤 서치: 연속된 매개변수 값을 탐색할 때 유용함. 탐색할 값을 직접 나열하는것이 아니고 탐색값을 샘플링 할 수 있는 확률 분포 객체를 전달

- Scikit-learn 패키지
  - crpss_va;odate(): 교차 검증을 수행하는 함수
  - GridSerchCV: 교차검증으로 하이퍼파라미터 탐색을 수행
  - RandomizedSearchCV: 교차검증으로 랜덤한 하이퍼파라미터 탐색을 수행
