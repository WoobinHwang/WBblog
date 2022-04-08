---
title: "심층 신경망"
author: "woobin"
date: '2022-04-04'
---

# 심증 신경망


```python
from tensorflow import keras

(train_input, train_target), (test_input, test_target) = keras.datasets.fashion_mnist.load_data()
```


```python
from sklearn.model_selection import train_test_split

train_scaled = train_input / 255.0  # 0 ~ 255의 픽셀값을 0~1로 변환
train_scaled = train_scaled.reshape(-1, 28*28)  # 2차원 배열을 1차원 배열로 변환

train_scaled, val_scaled, train_target, val_target = train_test_split(
    train_scaled, train_target, test_size=0.2, random_state=42)
```

# 심층 신경망 만들기

- 입력값 및 출력값 층 만들기


```python
dense1 = keras.layers.Dense(100, activation='sigmoid', input_shape=(784,))
dense2 = keras.layers.Dense(10, activation='softmax')
```


```python
model = keras.Sequential([dense1, dense2])
model.summary()
```

    Model: "sequential_7"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_14 (Dense)            (None, 100)               78500     
                                                                     
     dense_15 (Dense)            (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

# 층을 추가하는 다른 방법


```python
# 직관적이게 한 줄로 적는 방법
model = keras.Sequential([
    keras.layers.Dense(100, activation='sigmoid', input_shape=(784,), name='hidden'),
    keras.layers.Dense(10, activation='softmax', name='output')
], name='패션 MNIST 모델')
```


```python
model.summary()
```

    Model: "패션 MNIST 모델"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     hidden (Dense)              (None, 100)               78500     
                                                                     
     output (Dense)              (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    


```python
# add()함수를 이용해 층을 추가하는 방법
model = keras.Sequential()
model.add(keras.layers.Dense(100, activation='sigmoid', input_shape=(784,)))
model.add(keras.layers.Dense(10, activation='softmax'))
```


```python
model.summary()
```

    Model: "sequential_8"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_16 (Dense)            (None, 100)               78500     
                                                                     
     dense_17 (Dense)            (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

    - 은닉층의 layer 수: 785 * 100
    - 결과층의 layer 수: (100 * 10) + 10 으로 추정됨.


```python
model.compile(loss='sparse_categorical_crossentropy', metrics='accuracy')

model.fit(train_scaled, train_target, epochs=5)
```

    Epoch 1/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.5608 - accuracy: 0.8098
    Epoch 2/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.4056 - accuracy: 0.8525
    Epoch 3/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3716 - accuracy: 0.8661
    Epoch 4/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3491 - accuracy: 0.8735
    Epoch 5/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3319 - accuracy: 0.8800
    




    <keras.callbacks.History at 0x7f47a719add0>



- 층이 많은 심층 신경망일수록 학습을 어렵게 만들어 relu함수를 이용함.
  - 도출되는 확률값이 0이거나 음수이면 0을 출력.

- Flatten 클래스: 배치 차원을 제외하고 나머지 입력 차원을 모두 일렬로 펼치는 역할. Flatten층을 생성


```python
model = keras.Sequential()
model.add(keras.layers.Flatten(input_shape=(28, 28)))
model.add(keras.layers.Dense(100, activation='relu'))
model.add(keras.layers.Dense(10, activation='softmax'))
```


```python
model.summary()
```

    Model: "sequential_9"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     flatten_1 (Flatten)         (None, 784)               0         
                                                                     
     dense_18 (Dense)            (None, 100)               78500     
                                                                     
     dense_19 (Dense)            (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

# 옵티마이저
- 대체로 Adam을 사용하면 됨.
- 이유: 스텝 방향 & 스템 사이즈를 모두 고려한 옵티마이저
  - 스텝 방향만 고려: GD, SGD, Momentum, NAG
  - 스템 사이즈만 고려: GD, SGD, Adagrad, RMSProp

- 오류 방지용 base


```python
(train_input, train_target), (test_input, test_target) = keras.datasets.fashion_mnist.load_data()

train_scaled = train_input / 255.0

train_scaled, val_scaled, train_target, val_target = train_test_split(
    train_scaled, train_target, test_size=0.2, random_state=42)
```


```python
model.compile(loss='sparse_categorical_crossentropy', metrics='accuracy')
model.fit(train_scaled, train_target, epochs=5)
```

    Epoch 1/5
    1500/1500 [==============================] - 5s 3ms/step - loss: 0.5277 - accuracy: 0.8145
    Epoch 2/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3914 - accuracy: 0.8606
    Epoch 3/5
    1500/1500 [==============================] - 7s 5ms/step - loss: 0.3551 - accuracy: 0.8724
    Epoch 4/5
    1500/1500 [==============================] - 5s 3ms/step - loss: 0.3351 - accuracy: 0.8807
    Epoch 5/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3198 - accuracy: 0.8857
    




    <keras.callbacks.History at 0x7f47a7af6e10>




```python
model.evaluate(val_scaled, val_target)
```

    375/375 [==============================] - 1s 1ms/step - loss: 0.3423 - accuracy: 0.8812
    




    [0.3422880470752716, 0.8811666369438171]



- 옵티마이저 실습


```python
model.compile(optimizer='sgd', loss='sparse_categorical_crossentropy', metrics='accuracy')
```


```python
sgd = keras.optimizers.SGD()
model.compile(optimizer=sgd, loss='sparse_categorical_crossentropy', metrics='accuracy')
```


```python
# 학습률 지정
sgd = keras.optimizers.SGD(learning_rate= 0.1)
# 모멘텀 최적화: 0보다 큰 값으로 지정하면 경사하강법을 가속도처럼 사용
sgd = keras.optimizers.SGD(momentum= 0.9, nesterov= True)
```


```python
# 적응적 학습률: 모델이 최적점에 가까이 갈수록 학습률을 낯추어, 안정적으로 최적점에 수렴 할 가능성이 높음.
# 'adagrad' 를 이용
adagrad =keras.optimizers.Adagrad()
model.compile(optimizer= adagrad, loss= 'sparse_categoriacl_crossentropy',
              metrics= 'accuracy')
```


```python
# 적응적 학습률
# 'RMSprop' 을 이용
rmsprop =keras.optimizers.RMSprop()
model.compile(optimizer= rmsprop, loss= 'sparse_categoriacl_crossentropy',
              metrics= 'accuracy')
```


```python
# 모멘텀 최적화와 RMSprop의 장점을 접목한것이 Adam
model = keras.Sequential()
model.add(keras.layers.Flatten(input_shape= (28, 28)))
model.add(keras.layers.Dense(100, activation= 'relu'))
model.add(keras.layers.Dense(10, activation= 'softmax'))


model.compile(optimizer= 'adam', loss='sparse_categorical_crossentropy', metrics='accuracy')
model.fit(train_scaled, train_target, epochs=5)
```

    Epoch 1/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.5213 - accuracy: 0.8182
    Epoch 2/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3931 - accuracy: 0.8588
    Epoch 3/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3477 - accuracy: 0.8730
    Epoch 4/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3233 - accuracy: 0.8807
    Epoch 5/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3053 - accuracy: 0.8876
    




    <keras.callbacks.History at 0x7f47a7af2a10>




```python
model.evaluate(val_scaled, val_target)
```

    375/375 [==============================] - 1s 1ms/step - loss: 0.3386 - accuracy: 0.8772
    




    [0.33857372403144836, 0.8771666884422302]



# 마무리 정리
- 키워드
  - **심층 신경망**: 2개 이상의 층을 포함한 신경망. 종종 다층 인공 신경망, 심층 신경망, 딥러닝을 같은 의미로 사용
  - **은닉층**: 입력층과 출력층 사이에 있는 모든 층
  - **렐루 함수**: 이미지 분류 모델의 은닉층을 많이 사용하는 활성화 함수
  - **옵티마이저**: 신경망의 가중치와 절편을 학습하기 위한 알고리즘 또는 방법
- **TensorFlow 패키지**
  - **add()**: 케라스 모델에 층을 추가하는 메서드
  - **summary()**: 케라스 모델의 정보를 출력
  - **SGD**: 기본 경사 하강법 옵티마이저 클래스
  - **Adagrad**: Adagrad 옵티마이저 클래스
  - **RMSprop**: RMSprop옵티마이저 클래스
  - **Adam**: Adam옵티마이저 클래스
