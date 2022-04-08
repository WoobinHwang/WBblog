---
title: "합성곱 신경망_02"
author: "woobin"
date: '2022-04-07'
---

# 합성곱 신경망을 사용한 이미지 분류

# 패션 MNIST 데이터 불러오기
- 데이터 스케일 0 ~ 255의 데이터를 0 ~ 1로 표준화

- 훈련 데이터 / 검증 데이터 분류

- 합성곱 신경망은 2차원 이미지를 그대로 사용하기 때문에 일렬로 펼치지 않아도 됨

- 완전 연결 신경망 (Fully Connected Layer)
  - 2차원 배열을 1차원 배열로 바꿔야함


```python
from tensorflow import keras
from sklearn.model_selection import train_test_split

(train_input, train_target), (test_input, test_target) = \
    keras.datasets.fashion_mnist.load_data()

train_scaled = train_input.reshape(-1, 28, 28, 1) / 255.0

train_scaled, val_scaled, train_target, val_target = train_test_split(
    train_scaled, train_target, test_size=0.2, random_state=42)

print(train_input.shape, train_scaled.shape)
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
    (60000, 28, 28) (48000, 28, 28, 1)
    

    - (60000, 28, 28)크기였던 train_input이 (48000, 28, 28, 1)크기의 train_scaled가 됨.

# 합성곱 신경망 만들기
- 합성곱 신경망의 전형적인 구조
  - 합성곱 층으로 이미지에서 특징을 감지 한 후 밀집층으로 클래스에 따른 분류 확률을 계산


```python
# 실험 코드
model = keras.Sequential()

# 합성곱 층
model.add(keras.layers.Conv2D(32, kernel_size = 3, activation = 'relu',
                              padding = 'same', input_shape = (28, 28, 1)))

# 풀링층
# 평균 풀링을 사용, 풀링 크기는 (3, 3)로 지정
# # 최대 풀링의 함수: MaxPooling2D()
# # 평균 풀링의 함수: AveragePooling2D()
model.add(keras.layers.AveragePooling2D(3))  # 1/3으로 줄이겠다는 의미

# 합성곱 층
model.add(keras.layers.Conv2D(32, kernel_size=(3,3), activation='relu', 
                              padding='same'))

# 풀링층
model.add(keras.layers.AveragePooling2D(3))

# 완전연결층 (밀집층 = Fully Connected Layer)
model.add(keras.layers.Flatten())
model.add(keras.layers.Dense(100, activation='relu'))
model.add(keras.layers.Dropout(0.4))
model.add(keras.layers.Dense(10, activation='softmax'))

model.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     conv2d (Conv2D)             (None, 28, 28, 32)        320       
                                                                     
     average_pooling2d (AverageP  (None, 9, 9, 32)         0         
     ooling2D)                                                       
                                                                     
     conv2d_1 (Conv2D)           (None, 9, 9, 32)          9248      
                                                                     
     average_pooling2d_1 (Averag  (None, 3, 3, 32)         0         
     ePooling2D)                                                     
                                                                     
     flatten (Flatten)           (None, 288)               0         
                                                                     
     dense (Dense)               (None, 100)               28900     
                                                                     
     dropout (Dropout)           (None, 100)               0         
                                                                     
     dense_1 (Dense)             (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 39,478
    Trainable params: 39,478
    Non-trainable params: 0
    _________________________________________________________________
    

                                            


```python
model = keras.Sequential()

# 합성곱 층
model.add(keras.layers.Conv2D(32, kernel_size = 3, activation = 'relu',
                              padding = 'same', input_shape = (28, 28, 1)))

# 풀링층
# 최대 풀링을 사용, 전형적인 풀링 크기인 (2,2)로 지정
# # 최대 풀링의 함수: MaxPooling2D()
# # 평균 풀링의 함수: AveragePooling2D()
model.add(keras.layers.MaxPooling2D(2))  # 절반으로 줄이겠다는 의미

# 합성곱 층
model.add(keras.layers.Conv2D(64, kernel_size=(3,3), activation='relu', 
                              padding='same'))

# 풀링층
model.add(keras.layers.MaxPooling2D(2))

# 완전연결층 (밀집층 = Fully Connected Layer)
model.add(keras.layers.Flatten())
model.add(keras.layers.Dense(100, activation='relu'))
model.add(keras.layers.Dropout(0.4))
model.add(keras.layers.Dense(10, activation='softmax'))

model.summary()
```

    Model: "sequential_1"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     conv2d_2 (Conv2D)           (None, 28, 28, 32)        320       
                                                                     
     max_pooling2d (MaxPooling2D  (None, 14, 14, 32)       0         
     )                                                               
                                                                     
     conv2d_3 (Conv2D)           (None, 14, 14, 64)        18496     
                                                                     
     max_pooling2d_1 (MaxPooling  (None, 7, 7, 64)         0         
     2D)                                                             
                                                                     
     flatten_1 (Flatten)         (None, 3136)              0         
                                                                     
     dense_2 (Dense)             (None, 100)               313700    
                                                                     
     dropout_1 (Dropout)         (None, 100)               0         
                                                                     
     dense_3 (Dense)             (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 333,526
    Trainable params: 333,526
    Non-trainable params: 0
    _________________________________________________________________
    

- 층의 구조를 그림으로 표현
  - plot_model()함수


```python
keras.utils.plot_model(model)
```




    
![png](/Images/0407_Convolution_Neural_Network_02/output_9_0.png)
    




```python
# 실험 코드
keras.utils.plot_model(model, show_shapes=True,
                       dpi= 50)

# dpi로 크기를 조절 할 수 있음 [default: 96]
```




    
![png](/Images/0407_Convolution_Neural_Network_02/output_10_0.png)
    



- 매개변수 show_shapes를 적용하여 입력과 출력의 크기를 표시


```python
keras.utils.plot_model(model, show_shapes=True)
```




    
![png](/Images/0407_Convolution_Neural_Network_02/output_12_0.png)
    



    - 지금까지 한 것은 모델 정의

- 모델 컴파일 후, 훈련


```python
# import tensorflow as tf

model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', 
              metrics='accuracy')

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-cnn-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=2,
                                                  restore_best_weights=True)

# with tf.device('/device:GPU:0'):

history = model.fit(train_scaled, train_target, epochs=10,
                    validation_data=(val_scaled, val_target),
                    callbacks=[checkpoint_cb, early_stopping_cb])
```

    Epoch 1/10
    1500/1500 [==============================] - 22s 7ms/step - loss: 0.5226 - accuracy: 0.8148 - val_loss: 0.3283 - val_accuracy: 0.8823
    Epoch 2/10
    1500/1500 [==============================] - 12s 8ms/step - loss: 0.3447 - accuracy: 0.8770 - val_loss: 0.2821 - val_accuracy: 0.8950
    Epoch 3/10
    1500/1500 [==============================] - 11s 7ms/step - loss: 0.2924 - accuracy: 0.8943 - val_loss: 0.2594 - val_accuracy: 0.8985
    Epoch 4/10
    1500/1500 [==============================] - 13s 8ms/step - loss: 0.2618 - accuracy: 0.9046 - val_loss: 0.2387 - val_accuracy: 0.9109
    Epoch 5/10
    1500/1500 [==============================] - 12s 8ms/step - loss: 0.2392 - accuracy: 0.9132 - val_loss: 0.2278 - val_accuracy: 0.9163
    Epoch 6/10
    1500/1500 [==============================] - 10s 7ms/step - loss: 0.2190 - accuracy: 0.9192 - val_loss: 0.2297 - val_accuracy: 0.9172
    Epoch 7/10
    1500/1500 [==============================] - 10s 7ms/step - loss: 0.2006 - accuracy: 0.9255 - val_loss: 0.2286 - val_accuracy: 0.9179
    


```python
# import tensorflow as tf
# model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', 
#               metrics='accuracy')

# checkpoint_cb = keras.callbacks.ModelCheckpoint('best-cnn-model.h5', 
#                                                 save_best_only=True)
# early_stopping_cb = keras.callbacks.EarlyStopping(patience=2,
#                                                   restore_best_weights=True)

# with tf.device('/device:GPU:0'):
#   history = model.fit(train_scaled, train_target, epochs=10,
#                       validation_data=(val_scaled, val_target),
#                       callbacks=[checkpoint_cb, early_stopping_cb])
```

- 모델 학습 곡선 그리기


```python
# # 연습 코드: plotly로 그려보기
# # 아래와 비슷한 결과를 얻었지만 블로그에 올리기 위해 주석 처리
# import plotly.express as px
# from plotly.subplots import make_subplots
# import plotly.graph_objects as go
# import matplotlib.pyplot as plt

# fig = make_subplots()

# fig.add_trace(go.Bar(y= history.history['loss']))
# fig.add_trace(go.Scatter(y= history.history['val_loss']))

# fig.show()
```


```python
import matplotlib.pyplot as plt
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.legend(['train', 'val'])
plt.show()
```


    
![png](/Images/0407_Convolution_Neural_Network_02/output_19_0.png)
    



```python
# 그래픽 카드로 돌리는지 확인하는 방법
import tensorflow as tf

device_name = tf.test.gpu_device_name()
if device_name != '/device:GPU:0':
  raise SystemError('GPU device not found')
print('Found GPU at: {}'.format(device_name))
```

    Found GPU at: /device:GPU:0
    

# 합성곱 신경망 시각화

- model이 어떤 가중치를 학습했는지 확인


```python
from tensorflow import keras
model = keras.models.load_model('best-cnn-model.h5')

# keras.utils.plot_model(model2, show_shapes=True)

model.layers  # 리스트 형태로 저장되어 있음
```




    [<keras.layers.convolutional.Conv2D at 0x7fc7d81a7790>,
     <keras.layers.pooling.MaxPooling2D at 0x7fc8e00bfc10>,
     <keras.layers.convolutional.Conv2D at 0x7fc7f2f88490>,
     <keras.layers.pooling.MaxPooling2D at 0x7fc7d710d5d0>,
     <keras.layers.core.flatten.Flatten at 0x7fc8e023bb10>,
     <keras.layers.core.dense.Dense at 0x7fc7f3fb5350>,
     <keras.layers.core.dropout.Dropout at 0x7fc86a7a7050>,
     <keras.layers.core.dense.Dense at 0x7fc86a793750>]



    - 2번의 합성곱과 풀링을 하고 Flatten층, Dense층, Dropout층, Dense층이 보임

- 합성곱층의 가중치를 확인 가능


```python
conv = model.layers[0]
print(conv.weights[0].shape, conv.weights[1].shape)
```

    (3, 3, 1, 32) (32,)
    

    - 깊이가 1이므로 실제 커널 크기: (3, 3, 1)
    - 필터 개수가 32개이므로 weights의 첫번째 원소인 가중치의 크기: (3, 3, 1, 32)
    - weights의 두번째 원소는 절편의 개수를 나타냄
    - 필터마다 1개의 절편이 있으므로 (32,)크기가 됨.


```python
conv_weights = conv.weights[0].numpy()
print(conv_weights.mean(), conv_weights.std())  # 평균(mean), 표준편차(std)
```

    -0.025856383 0.21591602
    


```python
plt.hist(conv_weights.reshape(-1, 1))
plt.xlabel('weight')
plt.ylabel('count')
plt.show()
```


    
![png](/Images/0407_Convolution_Neural_Network_02/output_29_0.png)
    


    - 0을 중심으로 종 모양 분포를 띄고 있음


```python
fig, axs = plt.subplots(2, 16, figsize=(15,2))

for i in range(2):
    for j in range(16):
        axs[i, j].imshow(conv_weights[:,:,0,i*16 + j], vmin=-0.5, vmax=0.5)
        axs[i, j].axis('off')

plt.show()
```


    
![png](/Images/0407_Convolution_Neural_Network_02/output_31_0.png)
    


- 훈련하지 않은 빈 합성곱 신경망을 작성


```python
no_training_model = keras.Sequential()

no_training_model.add(keras.layers.Conv2D(32, kernel_size=3, activation='relu', 
                                          padding='same', input_shape=(28,28,1)))
```


```python
no_training_conv = no_training_model.layers[0]
print(no_training_conv.weights[0].shape)
```

    (3, 3, 1, 32)
    


```python
no_training_weights = no_training_conv.weights[0].numpy()
print(no_training_weights.mean(), no_training_weights.std())
```

    0.0065383185 0.081879705
    


```python
plt.hist(no_training_weights.reshape(-1, 1))
plt.xlabel('weight')
plt.ylabel('count')
plt.show()
```


    
![png](/Images/0407_Convolution_Neural_Network_02/output_36_0.png)
    



```python
fig, axs = plt.subplots(2, 16, figsize=(15,2))

for i in range(2):
    for j in range(16):
        axs[i, j].imshow(no_training_weights[:,:,0,i*16 + j], vmin=-0.5, vmax=0.5)
        axs[i, j].axis('off')

plt.show()
```


    
![png](/Images/0407_Convolution_Neural_Network_02/output_37_0.png)
    


    - 히스토그램에서 보앗듯이 전체적으로 가중치가 밋밋하게 초기화됨.
    - 특징이 뚜렷하지 않음.

# 함수형 API
- 딥러닝에는 더 복잡한 모델이 많이 있음.
  - 입력이 2개일수도 있고 출력이 2개일수도 있는데 이런 경우엔 함수형API를 사용

# 특성 맵 시각화


```python
print(model.input)
conv_acti = keras.Model(model.input, model.layers[0].output)
(train_input, train_target), (test_input, test_target) = keras.datasets.fashion_mnist.load_data()
plt.imshow(train_input[0], cmap='gray_r')
plt.show()
```

    KerasTensor(type_spec=TensorSpec(shape=(None, 28, 28, 1), dtype=tf.float32, name='conv2d_2_input'), name='conv2d_2_input', description="created by layer 'conv2d_2_input'")
    


    
![png](/Images/0407_Convolution_Neural_Network_02/output_41_1.png)
    



```python
inputs = train_input[0:1].reshape(-1, 28, 28, 1)/255.0
feature_maps = conv_acti.predict(inputs)
print(feature_maps.shape)
```

    (1, 28, 28, 32)
    

- 32개의 필터로 인해 입력 이미지에서 강하게 활성화 된 부분을 보여줌.


```python
fig, axs = plt.subplots(4, 8, figsize=(15,8))

for i in range(4):
    for j in range(8):
        axs[i, j].imshow(feature_maps[0,:,:,i*8 + j])
        axs[i, j].axis('off')

plt.show()
```


    
![png](/Images/0407_Convolution_Neural_Network_02/output_44_0.png)
    



```python
conv2_acti = keras.Model(model.input, model.layers[2].output)
feature_maps = conv2_acti.predict(train_input[0:1].reshape(-1, 28, 28, 1)/255.0)
print(feature_maps.shape)

fig, axs = plt.subplots(8, 8, figsize=(12,12))

for i in range(8):
    for j in range(8):
        axs[i, j].imshow(feature_maps[0,:,:,i*8 + j])
        axs[i, j].axis('off')

plt.show()
```

    (1, 14, 14, 64)
    


    
![png](/Images/0407_Convolution_Neural_Network_02/output_45_1.png)
    


-  - 합성곱 신경망의 앞부분에 있는 합성곱 층은 이미지의 시각적인 정보를 감지하고 뒤쪽에 있는 합성곱 층은 앞쪽에서 감지한 시각적인 정보를 바탕으로 추상적인 정보를 학습한다는 추론이 가능.

# 마무리
- 키워드
  - **가중치 시각화**: 합성곱 층의 가중치를 이미지로 출력하는것
  - **특성 맵 시각화**: 합성곱 층의 활성화 출력을 이미지로 그리는 것
  - **함수형 API**: 케라스에서 신경망 모델을 만드는 방법 중 하나. 전형적으로 입력은 Input()함수를 사용하고, 출력은 마지막 층의 출력으로 정의


- **TensolFlow** 패키지
  - **Conv2D**: 입력의 너비와 높이 방향의 합성곱 연산을 구현한 클래스
  - **MaxPooling2D**: 입력의 너비와 높이를 줄이는 풀링 연산을 구현한 클래스
  - **plot_model()**: 케라스 모델 구조를 주피터 노트북에 그리거나 파일로 저장.
  - **Model**: 케라스 모델을 만드는 클래스

- **matplotlib** 패키지
  - **bar()**: 막대그래프를 출력
