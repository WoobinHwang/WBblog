---
title: "앙상블_학습_01"
author: "woobin"
date: '2022-03-30'
---

# 트리의 앙상블
- LightGBM ( 중요 )
  - GBM --> XGBoost --> LightGBM
  - 참고1. 모델 개발 속도가 빨라졌나?
  - 참고2. 모델의 성능이 좋아졌나?
- TabNet (2019)
  - 딥러닝  컨셉

## 랜덤 포레스트(Forest)
- 결정 트리 나무를 500갸 심기
- 최종적인 결정은 투표 방식
  - 나무 1 : 양성
  - 나무 2 : 음성
  - 나무 3 : 양성
  - ...
  - 나무 n : 양성
  - 최종 판단 : 양성입니다!


```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

wine = pd.read_csv('https://bit.ly/wine_csv_data')

data = wine[['alcohol', 'sugar', 'pH']].to_numpy()
target = wine['class'].to_numpy()

train_input, test_input, train_target, test_target = train_test_split(data, 
                                                                      target, 
                                                                      test_size=0.2, 
                                                                      random_state=42)
```

- 267p
  - cross_validate() 교차 검증 수행


```python
from sklearn.model_selection import cross_validate
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(n_jobs= -1, random_state=42)

scores= cross_validate(rf, train_input, train_target,
                      return_train_score= True, n_jobs= -1)  # return_train_score: 훈련세트의 점수도 같이 반환됨.

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.9973541965122431 0.8905151032797809
    


```python
rf.fit(train_input, train_target)
print(rf.feature_importances_)
# feature_importances_: 하나의 특성에 과도하게 집중하지 않고 좀 더 많은 특성이 훈련에 기여
```

    [0.23167441 0.50039841 0.26792718]
    


```python
rf= RandomForestClassifier(oob_score= True, n_jobs= -1, random_state= 42)
rf.fit(train_input, train_target)
print(rf.oob_score_)
# oob샘플: 훈련세트에 중복을 허용하여 푸트스트랩 샘플을 만들때 포함되지 않고 남는 샘플 (=검증 세트의 역할)
# oob_score_: 검증 점수와 비슷한 값을 얻을 수 있음
```

    0.8934000384837406
    

# 그레이디언트 부스팅
- 이전트리의 오차를 보완하는 방식으로 사용
- 깊이가 얕은 트리를 사용
- 학습률 매개변수로 속도를 조절
- 단점: 속도가 느림.


```python
from sklearn.ensemble import GradientBoostingClassifier # 깊이가 얕은 결정 트리를 사용
gb = GradientBoostingClassifier(random_state= 42)
scores= cross_validate(gb, train_input, train_target,
                      return_train_score= True, n_jobs= -1)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.8881086892152563 0.8720430147331015
    


```python
gb = GradientBoostingClassifier(n_estimators= 500, learning_rate= 0.2, random_state= 42)
scores= cross_validate(gb, train_input, train_target,
                      return_train_score= True, n_jobs= -1)

print(np.mean(scores['train_score']), np.mean(scores['test_score']))
```

    0.9464595437171814 0.8780082549788999
    


```python
gb.fit(train_input, train_target)
print(gb.feature_importances_)
#  랜덤포레스트보다 일부 특성(당도)에 더 집중하는게 보임.
```

    [0.15872278 0.68010884 0.16116839]
    

- 흐름
  - 데이터 전처리/ 시각화
  - 기본 모형으로 전체 흐름을 설계
  - 여러모형으로 비교 대조
  - 교차 검증, 하이퍼 파라미터 성능 비교
  - ...
  - 1등하는 그날까지...??

# 마무리 정리
- 키워드
  - 앙상블 학습: 더 좋은 예측 결과를 만들기 위해 여러 개의 모델을 훈련하는 머신러닝 알고리즘을 말함.
  - 랜덤 포레스트: 대표적인 결정트리ㅇ 기반의 앙상블 학습 방법으로 부트스트랩 샘플을 사용하고 랜덤하게 일부 특성을 선택하여 트리를 만드는것이 특징
  - 엑스트라 트리: 랜덤포레스트와 비슷하게 결정 트리를 사용하여 앙상블 모델을 만들지만 부트스트랩 샘플을 사용하지 않음.
  - 그레이디언트 부스팅: 결정트리를 연속적으로 추가하여 손실 함수를 최소화 하는 앙상블 방법
  - 히스토그램 기반 그레이디언트 부스팅: 그레이디언트 부스팅의 속도를 개선했으ㅜ며, 안정적인 결과와 높은 성능으로 매우 인기가 높음.
- Scikit-learn
  - RandomForestClassifier: 랜덤 포레스트 분류 클래스
    - 매개변수 n_estimators: 앙상블을 구성할 트리의 개수를 지정 [defalt: 100]
    - 매개변수 criterion: 불순도를 지정 [defalt: gini]
    - max_depth: 트리가 성장할 최대 깊이를 지정 [defalt: None]
    - min_samples_split: 노드를 나누기 위한 최소 샘플 개수 [defalt: 2]
    - 메게변수 max_features: 최적의 분할을 위해 탐색할 특성의 개수를 지정 [defalt: auto](특성 개수의 제곱근)
    - 매개변수 bootstrap: 부트스트랩 샘플을 사용할지 지정 [defalt: True]
    - oob_score: OOB샘플을 사용하여 훈련한 모델을 평가할지 지정 [defalt: False]
    - 매개변수 n_jobs: 병렬 실행에 사용할 CPU코어 수를 지정 [defalt: 1](-1로 지정하면 시스템에 있는 모든 코어를 사용)
    - GrandientBoostingClassifier: 그레이디언트 부스팅 분류 클래스
    - 매개변수 loss: 손실 함수를 지정 [defalt: deviance]
    - 매개변수 learning_rate: 트리가 앙상블에 기여하는 정도를 조절 [defalt: 0.1]
    - 매개변수 n_estimators: 부스팅 단계를 수행하는 트리의 개수
    - 매개변수 subsample: 사용할 훈련 세트의 샘플 비율을 지정 [defalt: 1]
    - 매개변수 max_depth: 개별 회귀 트리의 최대 깊이 [defalt: 3]
  - HistGradientBoostingClassifier: 히스토그램 기반 그레이디언트 부스팅 분류 클래스
    - 매개변수 learning_rate: 학습률 또는 감쇠율 [defalt: 0.1]
    - max_iter: 부스팅 단계를 수행하는 트리의 개수 [defalt: 100]
    - max_bins: 입력 데이터를 나눌 구간의 개수. [defalt: 255]
