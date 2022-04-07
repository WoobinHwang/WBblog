---
title: "Kaggle 프로젝트"
author: "woobin"
date: '2022-04-06'
---

# 프로젝트 개요
- 강의명 : 2022년 K-디지털 직업훈련(Training) 사업 - AI데이터플랫폼을 활용한 빅데이터 분석전문가 과정
- 교과목명 : 빅데이터 분석 및 시각화, AI개발 기초, 인공지능 프로그래밍
- 프로젝트 주제 : Spaceship Titanic 데이터를 활용한 탑승유무 분류모형 개발
- 프로젝트 마감일 : 2022년 4월 12일 화요일
- 강사명 : 정지훈 강사
- 수강생명 : 황우빈


```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)


# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session
```

# Step.1 라이브러리 및 데이터 불러오기
- 라이브러리 버전 확인


```python
import numpy as np
import pandas as pd
import plotly.express as px
from plotly.subplots import make_subplots
import plotly.graph_objects as go
import matplotlib.pyplot as plt

from sklearn.preprocessing import OneHotEncoder
from sklearn.compose import make_column_transformer
from sklearn.model_selection import train_test_split
from sklearn.model_selection import StratifiedKFold

from scipy.stats import uniform, randint
from sklearn.model_selection import RandomizedSearchCV
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import cross_validate
from sklearn.model_selection import GridSearchCV

from lightgbm import LGBMClassifier
from sklearn.metrics import accuracy_score

print("success")
```


```python
# 데이터 불러오기
    # PassengerId: 승객 번호, Transported: 전송
sample = pd.read_csv("/kaggle/input/spaceship-titanic/sample_submission.csv")
# print(sample.head())
test = pd.read_csv("/kaggle/input/spaceship-titanic/test.csv")
print(test.head())
train = pd.read_csv("/kaggle/input/spaceship-titanic/train.csv")
print(train.head())
```

## 칼럼 별 설명:
- PassengerId: 각 승객의 고유 ID
- HomePlanet: 승객이 출발한 행성
- CryoSleep: 승객이 항해 기간 동안 애니메이션을 일시 중단하도록 선택했는지 여부를 나타냄
- Cabin: 승객이 머물고 있는 객실 번호입니다.
- Destination: 승객이 출발할 행성입니다.
- Age: 승객의 나이
- VIP: 승객이 항해 중 특별 VIP 서비스 비용을 지불했는지 여부.
- RoomService: 승객이 룸서비스에 대해 청구한 금액입니다.
- Name: 승객의 이름
- Transported: 승객이 다른 차원으로 운송되었는지 여부. 예측하려는 대상인 열

- train데이터 결측치 유무, 통계 확인


```python
train.info()
train.describe()
```

- test데이터 결측치 유무, 통계 확인


```python
# 결측치 확인
test.info()
test.describe()
```

# Step.2 탐색적 자료 분석(EDA)
- 데이터 시각화
- 산점도, 막대 그래프 등
- 그래프 해석해서 설명을 달아야 함.
- 약간의 데이터 전처리

- 시각화하기 위한 함수 작성.


```python
# text = "Age"
# train_series = train[text].value_counts()
# train_df = pd.DataFrame(train_series).sort_index()

# test_series = test[text].value_counts()
# test_df = pd.DataFrame(test_series).sort_index()

# fig = make_subplots(rows= 1,
#                    cols= 1,
#                    column_titles= ["Target Values"],
#                    y_title= "Values")

# fig.add_trace(go.Scatter(x= df.index, y= train_df[text],
#                     orientation="v",
#                         name= "train"),
#                     1, 1)

# fig.add_trace(go.Scatter(x= df.index, y= test_df[text],
#                     orientation="v"),
#                     1, 1)          
# fig.update_layout()
# fig.show()
```


```python
def visual(train, test, text):
    
    train_series = train[text].value_counts()
    train_df = pd.DataFrame(train_series).sort_index()
    
    test_series = test[text].value_counts()
    test_df = pd.DataFrame(test_series).sort_index()

    fig = make_subplots(rows= 1,
                       cols= 1,
                       column_titles= ["Target Values"],
                       y_title= "Values")

    fig.add_trace(go.Scatter(x= train_df.index, y= train_df[text],
                        orientation="v",
                            name= "train_data"),
                        1, 1)

    fig.add_trace(go.Scatter(x= test_df.index, y= test_df[text],
                        orientation="v",
                            name= "test_data"),
                        1, 1)          
    fig.update_layout()
    fig.show()
```


```python
visual(train, test, "Age")
```

    - 10대 후반에서 30대 후반의 비율이 높은것을 확인 할 수 있음.


```python
visual(train, test, "RoomService")
```


```python
visual(train, test, "FoodCourt")
```


```python
visual(train, test, "ShoppingMall")
```


```python
visual(train, test, "Spa")
```


```python
visual(train, test, "VRDeck")
```

    - RoomService, FoodCourt, ShoppingMall, Spa, VRDeck의 데이터중 10 미만의 값들이 압도적으로 많은것을 확인 할 수 있다.

- 결측값들의 개수 찾기


```python
train_null = pd.DataFrame(train.isna().sum())
train_null= train_null.sort_values(by= 0)
print(train_null)

print()

test_null = pd.DataFrame(test.isna().sum())
test_null= test_null.sort_values(by= 0)
print(test_null)
```


```python
# 결측치데이터 - 시각화
fig = make_subplots(rows= 1,
                   cols= 2,
                   column_titles= ["train data", "test data"],
                   x_title= "Missing Values")

fig.add_trace(go.Bar(x= train_null[0], y= train_null.index,
                    orientation="h",),
                    1, 1)
              
fig.add_trace(go.Bar(x= test_null[0], y= test_null.index,
                    orientation="h"),
                    1, 2)          

fig.update_layout()
fig.show()
```

    - Transported와 PassengerId를 제외하고는 결측치가 존재하는걸 확인 할 수 있음.

# Step.3 데이터 전처리
- Feature Engineering
- 머신러닝(ML) 모형을 돌리기 위해 표준화 등 / 원핫-인코딩
- 파생변수(도출변수) 만들기
    - 왜 이 변수를 만들었는지에 대한 설명 필요

- 필요한 데이터를 제외하고는 제외


```python
train.head()
```


```python
target = train['Transported']
target
```


```python
print(train['PassengerId'].value_counts())  # x
# train['HomePlanet'].value_counts()  # o
# train['CryoSleep'].value_counts()  # o
print(train['Cabin'].value_counts())  # x
# train['Destination'].value_counts()  # o
# train['Age'].value_counts()  # o
# train['VIP'].value_counts()  # o
# train['RoomService'].value_counts()  # o
# train['FoodCourt'].value_counts()  # o
# train['ShoppingMall'].value_counts()  # o
# train['Spa'].value_counts()  # o
# train['VRDeck'].value_counts()  # o
print(train['Name'].value_counts())  # x
```

    - 확인 해 본 결과 PassengerId, Cabin, Name의 특성은 예측에 영향을 안줄것으로 추정하여 제외하도록 함.


```python
# train_data = train.drop(['PassengerId', 'Name'], axis= 1)
# test_data = test.drop(['PassengerId', 'Name'], axis= 1)

train_data = train.drop(['PassengerId', 'Cabin', 'Name'], axis= 1)
test_data = test.drop(['PassengerId', 'Cabin', 'Name'], axis= 1)
print(train_data.shape)
print(test_data.shape)
```

- 결측치를 float 타입은 mean()으로 object 타입은 최빈값으로 채우기 위해 각각의 데이터를 확인


```python
train_data.info()
```


```python
# float
print(train_data["Age"].mean())
print(train_data["RoomService"].mean())
print(train_data["FoodCourt"].mean())
print(train_data["ShoppingMall"].mean())
print(train_data["Spa"].mean())
print(train_data["VRDeck"].mean())
# object
print(train_data["HomePlanet"].mode())
print(train_data["CryoSleep"].mode())
print(train_data["Destination"].mode())
print(train_data["VIP"].mode())
```

- train데이터 결측값 대체하기


```python
# float
train_data['Age'].replace(np.nan, train_data['Age'].mean(), inplace= True)
train_data['RoomService'].replace(np.nan, train_data['RoomService'].mean(), inplace= True)
train_data['FoodCourt'].replace(np.nan, train_data['FoodCourt'].mean(), inplace= True)
train_data['ShoppingMall'].replace(np.nan, train_data['ShoppingMall'].mean(), inplace= True)
train_data['Spa'].replace(np.nan, train_data['Spa'].mean(), inplace= True)
train_data['VRDeck'].replace(np.nan, train_data['VRDeck'].mean(), inplace= True)
# object
train_data['HomePlanet'].replace(np.nan, 'Earth', inplace= True)
train_data['Destination'].replace(np.nan, 'TRAPPIST-1e', inplace= True)
train_data['CryoSleep'].replace(np.nan, False, inplace= True)
train_data['VIP'].replace(np.nan, False, inplace= True)

train_data.isna().sum()
```


```python
test_data.info()
```


```python
# float
print(test_data["Age"].mean())
print(test_data["RoomService"].mean())
print(test_data["FoodCourt"].mean())
print(test_data["ShoppingMall"].mean())
print(test_data["Spa"].mean())
print(test_data["VRDeck"].mean())
# object
print(test_data["HomePlanet"].mode())
print(test_data["CryoSleep"].mode())
print(test_data["Destination"].mode())
print(test_data["VIP"].mode())
```

- test데이터 결측값 대체하기


```python
# float
test_data['Age'].replace(np.nan, test_data['Age'].mean(), inplace= True)
test_data['RoomService'].replace(np.nan, test_data['RoomService'].mean(), inplace= True)
test_data['FoodCourt'].replace(np.nan, test_data['FoodCourt'].mean(), inplace= True)
test_data['ShoppingMall'].replace(np.nan, test_data['ShoppingMall'].mean(), inplace= True)
test_data['Spa'].replace(np.nan, test_data['Spa'].mean(), inplace= True)
test_data['VRDeck'].replace(np.nan, test_data['VRDeck'].mean(), inplace= True)
# object
test_data['HomePlanet'].replace(np.nan, 'Earth', inplace= True)
test_data['Destination'].replace(np.nan, 'TRAPPIST-1e', inplace= True)
test_data['CryoSleep'].replace(np.nan, False, inplace= True)
test_data['VIP'].replace(np.nan, False, inplace= True)

test_data.isna().sum()
```

     -결측값들 성공적으로 제거!

## 원핫인코딩


```python
# Transported의 True는 1로, False는 0으로 대체
train_data['Transported'] = train_data['Transported'].map({True: 1, False: 0})
categorical = ['HomePlanet', 'Destination', 'CryoSleep', 'VIP']

transformer = make_column_transformer(
(OneHotEncoder(), categorical),
remainder = 'passthrough')


train_transformed = transformer.fit_transform(train_data[categorical])
train_transformed_df = pd.DataFrame(train_transformed, columns= transformer.get_feature_names_out())
train_data = pd.concat([train_data, train_transformed_df], axis= 1)
train_data = train_data.drop(categorical, axis= 1)

test_transformed = transformer.fit_transform(test_data[categorical])
test_transformed_df = pd.DataFrame(test_transformed, columns= transformer.get_feature_names_out())
test_data = pd.concat([test_data, test_transformed_df], axis= 1)
test_data = test_data.drop(categorical, axis= 1)

print("success")
```

# Step.4 머신러닝 모형 개발
- 모형에 대한 설명 필요
- 모형 1~2개 사용 권장
- 교차 검증
- 하이퍼 파라미터 튜닝
    - 랜덤서치(매개변수(max_depth))
    - 그래드서치

- 독립변수와 종속변수를 구분
    - 독립변수: x
    - 종속변수: Transported == y


```python
x_cols = test_data.columns
x = train_data[x_cols].to_numpy()
y = train_data['Transported'].to_numpy()
```

- 훈련데이터를 검증데이터로 분리
    - 검증데이터: val


```python
x_train, x_val, y_train, y_val = train_test_split(x, y, test_size= 0.3, random_state= 42)
x_train.shape,x_val.shape,y_train.shape,y_val.shape
```

## 랜덤서치


```python
params = {
    'min_impurity_decrease': uniform(0.0001, 0.001),
    'max_depth': randint(20, 50),
    'min_samples_split': randint(2, 25),
    'min_samples_leaf': randint(1, 25),}
gs = RandomizedSearchCV(DecisionTreeClassifier(random_state= 42), params,  # n_iter: 파라미터 검색 횟수
                        n_iter= 100, n_jobs= -1, random_state= 42)  # n_jobs: cpu코어 수

gs.fit(x_train, y_train)
```


```python
print(gs.best_params_)
```


```python
print(np.max(gs.cv_results_['mean_test_score']))
```


```python
dt = gs.best_estimator_
print(dt.score(x_val, y_val))
print("success")
```

## 교차검증


```python
scores = cross_validate(dt, x_train, y_train)
scores
```


```python
print(np.mean(scores['test_score']))
# 평균 점수 0.7903040262941661
```

- StratifiedKFold()로 Fold 교차검증을 높여봄.


```python
scores = cross_validate(dt, x_train, y_train, cv= StratifiedKFold())
print(np.mean(scores['test_score']))
```


```python
splitter = StratifiedKFold(n_splits=200, shuffle= True, random_state= 42)
scores = cross_validate(dt, x_train, y_train, cv= splitter)
print(np.mean(scores['test_score']))
```

GridSearchCV()의 하이퍼 파라미터를 이용해봄.


```python
params = {'min_impurity_decrease': [0.0001, 0.0002, 0.0003, 0.0004, 0.0005]}
```


```python
gs = GridSearchCV(DecisionTreeClassifier(random_state= 42),params, n_jobs= -1)
gs.fit(x_train, y_train)
```

- gs로 x,y train데이터 훈련


```python
dt= gs.best_estimator_
print(dt)
print(dt.score(x_train, y_train))
print(gs.best_params_)
```


```python
print(gs.cv_results_['mean_test_score'])
```

- gs로 x,y val데이터 훈련


```python
gs = GridSearchCV(DecisionTreeClassifier(random_state= 42),params, n_jobs= -1)
gs.fit(x_val, y_val)

dt= gs.best_estimator_
print(dt)
print(dt.score(x_val, y_val))
print(gs.best_params_)
```


```python
params = {'min_impurity_decrease': [0.0001, 0.0002, 0.0003, 0.0004, 0.0005],
          'max_depth': range(5, 20, 1),
         'min_samples_split': range(2, 100, 10)}
```


```python
print(gs.best_params_)
```


```python
print(np.max(gs.cv_results_['mean_test_score']))
```

- LightGBM 사용


```python
lgb = LGBMClassifier(random_state= 42)
lgb
```

# Step.5 모형 평가
- 훈련데이터를 쪼개어 훈련데이터 + 검증데이터 분리
- 정확도 비교
- 혼동행렬(confusion Matrix) 설명

- cross_validate() 활용


```python
# # 1st challenge
splitter = StratifiedKFold(n_splits = 5, shuffle = True, random_state=42)
scores = cross_validate(lgb, x_train, y_train, return_train_score = True, cv=splitter)

print("train Acc.", np.mean(scores['train_score']))
print("test Acc.", np.mean(scores['test_score']))
```

- 검증데이터를 활용하여 정확도를 예상해본다.


```python
# 1st challenge

lgb.fit(x_train, y_train)
y_pred = lgb.predict(x_val)
print("Acc.", accuracy_score(y_val, y_pred))
# Acc. 0.7864263803680982
```

## 혼동행렬 - 오분류 비용

- **나무위키 정의**: 어떤 개인이나 모델, 검사도구, 알고리즘의 '진단', '분류', '판별', '예측'능력을 평가하기 위해 고안된 표. 오류행렬(error matrix)이라고도 하며, 국내에는 오분류표라고 번역되기도 한다.

- 표로 나타내기.


```python
def confusion_matrix():
    col = ["실제로 맞았다  ", "  실제로 틀렸다"]
    ind = ["맞을것이다", "틀릴것이다"]
    con = [["예측 성공", "예측 실패"], ["예측 실패", "예측 성공"]]
    matrix = pd.DataFrame(con, columns=col, index=ind)
    print(matrix)
```


```python
confusion_matrix()
```

    - True, False 기준에서 작성했기에 2 by 2의 모습
    - 기준을 어떻게 잡느냐에 따라서 3 by 3 이상의 다등급 분류를 나타내는 혼동행렬로 나타낼 수 있다.

# Step.6 제출
- 제출 양식 샘플은 만들어줌.


```python
# 1st challenge
test_preds = lgb.predict(test_data.to_numpy())
sample['Transported'] = test_preds.astype("bool")
sample.to_csv("submission.csv",index=False)
sample.head()
```

# 참고
- 다른 사람의 code 설명을 쭉 따라치는경우
    - 노트북 표절 방지 위해서, 참조한 코드는 반드시 링크 걸어둘것
    - 저자 이름, 글 제목, 링크 주소

# 마감일
- 4월 12일 17시 40분
- 제출 형태
    - Leaderboard 랭킹 사진 캡처
    - 고용노동부 보고 양식 (다음주에 확인하고 알려주실 예정)

# My_Note
- value_counts(): 값 별 개수 세기
- Pandas라이브러리
    - fillna(): 결측값을 지정한 값으로 채움
