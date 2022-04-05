---
title: "심층_신경망"
author: "woobin"
date: '2022-04-05'
---

# 심증 신경망


```python
from tensorflow import keras

(train_input, train_target), (test_input, test_target) = keras.datasets.fashion_mnist.load_data()
```


```python
from sklearn.model_selection import train_test_split

train_scaled = train_input / 255.0
train_scaled = train_scaled.reshape(-1, 28*28)

train_scaled, val_scaled, train_target, val_target = train_test_split(
    train_scaled, train_target, test_size=0.2, random_state=42)
```

- 입력값 및 출력값 층 만들기


```python
dense1 = keras.layers.Dense(100, activation='sigmoid', input_shape=(784,))
dense2 = keras.layers.Dense(10, activation='softmax')
```

# 심층 신경망 만들기


```python
model = keras.Sequential([dense1, dense2])
model.summary()
```

    Model: "sequential_4"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_8 (Dense)             (None, 100)               78500     
                                                                     
     dense_9 (Dense)             (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

# 층을 추가하는 다른 방법


```python
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
model = keras.Sequential()
model.add(keras.layers.Dense(100, activation='sigmoid', input_shape=(784,)))
model.add(keras.layers.Dense(10, activation='softmax'))
```


```python
model.summary()
```

    Model: "sequential_5"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_10 (Dense)            (None, 100)               78500     
                                                                     
     dense_11 (Dense)            (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    


```python
model.compile(loss='sparse_categorical_crossentropy', metrics='accuracy')

model.fit(train_scaled, train_target, epochs=5)
```

    Epoch 1/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.5603 - accuracy: 0.8106
    Epoch 2/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.4079 - accuracy: 0.8532
    Epoch 3/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3727 - accuracy: 0.8659
    Epoch 4/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3509 - accuracy: 0.8736
    Epoch 5/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3342 - accuracy: 0.8785
    




    <keras.callbacks.History at 0x7f0fbfcf9050>




```python
model = keras.Sequential()
model.add(keras.layers.Flatten(input_shape=(28, 28)))
model.add(keras.layers.Dense(100, activation='relu'))
model.add(keras.layers.Dense(10, activation='softmax'))
```


```python
model.summary()
```

    Model: "sequential_6"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     flatten_2 (Flatten)         (None, 784)               0         
                                                                     
     dense_12 (Dense)            (None, 100)               78500     
                                                                     
     dense_13 (Dense)            (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

# 옵티마이저
- 대체적으로 Adam을 사용하면 됨.
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
    1500/1500 [==============================] - 5s 3ms/step - loss: 0.5357 - accuracy: 0.8103
    Epoch 2/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3911 - accuracy: 0.8590
    Epoch 3/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3529 - accuracy: 0.8730
    Epoch 4/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3329 - accuracy: 0.8804
    Epoch 5/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3182 - accuracy: 0.8872
    




    <keras.callbacks.History at 0x7f0fbfba4950>




```python
model.evaluate(val_scaled, val_target)
```

    375/375 [==============================] - 1s 1ms/step - loss: 0.3537 - accuracy: 0.8813
    




    [0.35373446345329285, 0.8812500238418579]



- 옵티마이저 실습


```python
model.compile(optimizer='sgd', loss='sparse_categorical_crossentropy', metrics='accuracy')
```


```python
sgd = keras.optimizers.SGD()
model.compile(optimizer=sgd, loss='sparse_categorical_crossentropy', metrics='accuracy')
```

- learning_rate = 0.1
  - 랜덤서치, 그리드서치
  - --> 딥러닝에서도 하이퍼파라미터 튜닝


```python
sgd = keras.optimizers.SGD(learning_rate= 0.1)
sgd = keras.optimizers.SGD(momentum= 0.9, nesterov= True)
```

# 마무리 정리
- 키워드
  - 심층 신경망: 2개 이상의 층을 포함한 신경망. 종종 다층 인공 신경망, 심층 신경망, 딥러닝을 같은 의미로 사용
  - 렐루 함수: 이미지 분류 모데의 은닉층을 많이 사용하는 활성화 함수
  - 옵티마이저: 신경망의 가중치와 절편을 학습하기 위한 알고리즘 또는 방법
- TensorFlow 패키지
  - add(): 케라스 모델에 층을 추가하는 메서드
  - summary(): 케라스 모델의 정보를 출력
  - SGD: 기본 경사 하강법 옵티마이저 클래스
  - Adagrad: Adagrad 옵티마이저 클래스
  - RMSprop: RMSprop옵티마이저 클래스
  - Adam: Adam옵티마이저 클래스
