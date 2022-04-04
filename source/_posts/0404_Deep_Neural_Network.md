---
title: "심층_신경망"
author: "woobin"
date: '2022-04-04'
---

# 심증 신경망


```python
from tensorflow import keras

(train_input, train_target), (test_input, test_target) = keras.datasets.fashion_mnist.load_data()
```

    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-labels-idx1-ubyte.gz
    32768/29515 [=================================] - 0s 0us/step
    40960/29515 [=========================================] - 0s 0us/step
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/train-images-idx3-ubyte.gz
    26427392/26421880 [==============================] - 0s 0us/step
    26435584/26421880 [==============================] - 0s 0us/step
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-labels-idx1-ubyte.gz
    16384/5148 [===============================================================================================] - 0s 0us/step
    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/t10k-images-idx3-ubyte.gz
    4423680/4422102 [==============================] - 0s 0us/step
    4431872/4422102 [==============================] - 0s 0us/step
    


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

# 심층 신경망


```python
model = keras.Sequential([dense1, dense2])
model.summary()
```

    Model: "sequential_2"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense (Dense)               (None, 100)               78500     
                                                                     
     dense_1 (Dense)             (None, 10)                1010      
                                                                     
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

    Model: "sequential_3"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     dense_2 (Dense)             (None, 100)               78500     
                                                                     
     dense_3 (Dense)             (None, 10)                1010      
                                                                     
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
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.5633 - accuracy: 0.8074
    Epoch 2/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.4069 - accuracy: 0.8523
    Epoch 3/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3724 - accuracy: 0.8647
    Epoch 4/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3506 - accuracy: 0.8730
    Epoch 5/5
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3327 - accuracy: 0.8788
    




    <keras.callbacks.History at 0x7eff51aa6550>




```python
model = keras.Sequential()
model.add(keras.layers.Flatten(input_shape=(28, 28)))
model.add(keras.layers.Dense(100, activation='relu'))
model.add(keras.layers.Dense(10, activation='softmax'))
```


```python
model.summary()
```

    Model: "sequential_5"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     flatten_1 (Flatten)         (None, 784)               0         
                                                                     
     dense_4 (Dense)             (None, 100)               78500     
                                                                     
     dense_5 (Dense)             (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

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
