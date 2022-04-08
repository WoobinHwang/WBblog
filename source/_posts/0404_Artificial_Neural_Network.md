---
title: "인공 신경망"
author: "woobin"
date: '2022-04-04'
---

# 인공 신경망
## 딥러닝 라이브러리
- 텐서플로 : https://www.tensorflow.org/
  - 2016년 텐서플로 1 버전 vs 텐서플로 2 버전
  - 문법적으로 매우 다름
  - 산업용
- 파이토치 : https://pytorch.org/
  - 연구용

# 패션 MNIST 데이터 이용


# 데이터 불러오기


```python
import tensorflow
print(tensorflow.__version__)
```

    2.8.0
    


```python
from tensorflow import keras
(train_input, train_target), (test_input, test_target) = keras.datasets.fashion_mnist.load_data()
```

- 데이터 확인
  - 60000개 이미지, 이미지 크기는 28 * 28


```python
print(train_input.shape, train_target.shape)  # input= 사진 # target= 0~9의 타겟값
print(test_input.shape, test_target.shape)  
```

    (60000, 28, 28) (60000,)
    (10000, 28, 28) (10000,)
    

- 이미지 시각화


```python
import matplotlib.pyplot as plt
fig, axs= plt.subplots(1, 10, figsize=(10,10))
for i in range(10):
  axs[i].imshow(train_input[i], cmap='gray_r')
  axs[i].axis('off')
plt.show()
```


    
![png](/Images/0404_Artificial_Neural_Network/output_8_0.png)
    


- 타겟값 리스트


```python
print([train_target[i] for i in range(10)])
```

    [9, 0, 0, 3, 0, 2, 7, 2, 5, 5]
    

- 실제 타겟값의 값을 확인


```python
import numpy as np
print(np.unique(train_target, return_counts= True))
```

    (array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], dtype=uint8), array([6000, 6000, 6000, 6000, 6000, 6000, 6000, 6000, 6000, 6000]))
    

# 로지스틱 회귀로 패션 아이템 분류하기
- 경사하강법(기울기)
- 전제조건: 각 컬럼의 데이터셋 동일 (표준화)
- why 255? : 각 픽셀의 값 0 ~ 255 사이의 정숫값을 가진다.
  - 0 ~ 1 사이의 값으로 정규화 시킴


```python
train_scaled = train_input/ 255.0

# 1차원 배열로 만들기
train_scaled = train_scaled.reshape(-1, 28* 28)
print(train_scaled.shape)
```

    (60000, 784)
    


```python
from sklearn.model_selection import cross_validate
from sklearn.linear_model import SGDClassifier

sc = SGDClassifier(loss='log', max_iter=5, random_state=42)

scores = cross_validate(sc, train_scaled, train_target, n_jobs=-1)
print(np.mean(scores['test_score']))
```

    0.8195666666666668
    

## 모델 만들기
- 비정형데이터에 선형모델 또는 비선형 모델을 적용시키는것이 합리적인가??
  - 결론: 아니다!
  - 다른 대안은 있는가??: 인공신경망!
- 정형데이터에 인공신경망 및 딥러닝 모델을 적용시키는 것이 합리적인가
  - 결론: 아니다!

## 인공신경망 모델 적용


```python
import tensorflow as tf
from tensorflow import keras

from sklearn.model_selection import train_test_split

train_scaled, val_scaled, train_target, val_target = train_test_split(
    train_scaled, train_target, test_size=0.2, random_state=42)

print(train_scaled.shape, train_target.shape)
print(val_scaled.shape, val_target.shape)
```

    (48000, 784) (48000,)
    (12000, 784) (12000,)
    

- 다중분류의 상황이기에 소프트맥스 활성화 함수를 사용
  - 이진분류에는 시그모이드 함수(로지스틱 회귀)


```python
# 모델 정의
dense = keras.layers.Dense(10, activation= 'softmax', input_shape=(784, ))
model = keras.Sequential(dense)
model.compile(loss= 'sparse_categorical_crossentropy', metrics= 'accuracy')

# train데이터로 모델 훈련
model.fit(train_scaled, train_target, epochs= 5)
```

    Epoch 1/5
    1500/1500 [==============================] - 2s 1ms/step - loss: 0.6102 - accuracy: 0.7926
    Epoch 2/5
    1500/1500 [==============================] - 2s 1ms/step - loss: 0.4791 - accuracy: 0.8392
    Epoch 3/5
    1500/1500 [==============================] - 2s 2ms/step - loss: 0.4554 - accuracy: 0.8481
    Epoch 4/5
    1500/1500 [==============================] - 2s 1ms/step - loss: 0.4453 - accuracy: 0.8519
    Epoch 5/5
    1500/1500 [==============================] - 2s 2ms/step - loss: 0.4363 - accuracy: 0.8558
    




    <keras.callbacks.History at 0x7f04ab0dcc50>




```python
# val데이터로 모델 훈련
# evaluate()메서드도 fit()메서드와 비슷한 출력을 보여줌.
model.evaluate(val_scaled, val_target)
```

    375/375 [==============================] - 1s 3ms/step - loss: 0.4559 - accuracy: 0.8462
    




    [0.45591267943382263, 0.8462499976158142]



# 마무리 정리
- 키워드
  - **인공신경망(=딥러닝)**: 생물학적 뉴런에서 영감을 받아 만든 머신러닝 알고리즘
  - **텐서플로**: 구글이 만든 딥러닝 라이브러리. 케라스를 사용하면 간단한 모델에서 아주 복잡한 모델까지 손쉽게 만들 수 있다.
  - **케라스**: 텐서플로에서 제공하는 고수준 API. 2015년에 개발된 딥러닝 라이브러리
  - **입력층**: 처음 입력되는 모든 input데이터
  - **밀집층**: 가장 간단한 인공 신경망의 층
  - **출력층**: 신경망의 최종 값을 만드는 층
  - **완전 연결층(Fully Connected Layer)** : 양쪽의 뉴런이 모두 연결하고 있는 층
  - **뉴런(=유닛)**: 값을 계산하는 단위
  - **원-핫 인코딩**: 정숫값을 배열에서 해당 정수 위치의 원소만 1이고 나머지는 모두 0으로 변환
- **TensorFlow 패키지**
  - **Dense**: 신경망에서 가장 기본 층인 밀집층을 만드는 클래스
    - 매개변수 units: 첫번째 매개변수로 뉴런의 개수를 지정
    - 매개변수 activation: 사용할 활성화 함수를 지정. 이진분류일때는 'sigmooid', 다중분류일때는 'softmax'나 'relu'를 사용
  - **Sequential**: 케라스에서 신경망 모델을 만드는 클래스
  - **Compile()**: 모델 객체를 만든 후 훈련하기 전에 사용할 손실 함수와 측정 지표 등을 지정하는 메서드
  - **fit()**: 모델을 훈련하는 메서드
  - **evaluate()**: 모델 성능을 평가하는 메서드
  
