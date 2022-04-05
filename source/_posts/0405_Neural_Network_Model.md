---
title: "신경망_모델_훈련"
author: "woobin"
date: '2022-04-05'
---

# 신경망 모델 훈련

# 손실곡선


```python
from tensorflow import keras
from sklearn.model_selection import train_test_split

(train_input, train_target), (test_input, test_target) = \
    keras.datasets.fashion_mnist.load_data()

train_scaled = train_input / 255.0

train_scaled, val_scaled, train_target, val_target = train_test_split(
    train_scaled, train_target, test_size=0.2, random_state=42)
```

- 사용자 정의 함수를 작성


```python
def model_fn(a_layer=None):
  model = keras.Sequential()
  model.add(keras.layers.Flatten(input_shape=(28, 28)))
  model.add(keras.layers.Dense(100, activation='relu'))
  if a_layer:
    model.add(a_layer)
  model.add(keras.layers.Dense(10, activation='softmax'))
  return model

model = model_fn()
model.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     flatten (Flatten)           (None, 784)               0         
                                                                     
     dense (Dense)               (None, 100)               78500     
                                                                     
     dense_1 (Dense)             (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    

- 모델 정의 후, 학습


```python
model.compile(loss='sparse_categorical_crossentropy', metrics= 'accuracy')
history = model.fit(train_scaled, train_target, epochs=5, verbose= 1)
```

    Epoch 1/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3084 - accuracy: 0.8907
    Epoch 2/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2963 - accuracy: 0.8947
    Epoch 3/5
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2866 - accuracy: 0.8985
    Epoch 4/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.2826 - accuracy: 0.9015
    Epoch 5/5
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.2754 - accuracy: 0.9037
    

- history 객체 무슨 값이 있나??


```python
print(history.history.keys())
```

    dict_keys(['loss', 'accuracy'])
    

- 그래프 작성


```python
import matplotlib.pyplot as plt

plt.plot(history.history['loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.show()
```


    
![png](/Images/0405_Neural_Network_Model/output_10_0.png)
    


- 정확도 출력


```python
plt.plot(history.history['accuracy'])
plt.xlabel('epoch')
plt.ylabel('accuracy')
plt.show()
```


    
![png](/Images/0405_Neural_Network_Model/output_12_0.png)
    


- 손실그래프


```python
model = model_fn()
model.compile(loss='sparse_categorical_crossentropy', metrics='accuracy')

history = model.fit(train_scaled, train_target, epochs=20, verbose=0)
plt.plot(history.history['loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.show()
```


    
![png](/Images/0405_Neural_Network_Model/output_14_0.png)
    


- 검증 손실


```python
# # 중요
# history = model.fit(train_scaled, train_target, epochs=10, verbose=1, 
#                     validation_data=(val_scaled, val_target))

# plt.plot(history.history['loss'])
# plt.plot(history.history['val_loss'])
# plt.xlabel('epoch')
# plt.ylabel('loss')
# plt.legend(['train', 'val'])
# plt.show()
```


```python
model = model_fn()
model.compile(loss='sparse_categorical_crossentropy', metrics='accuracy')

history = model.fit(train_scaled, train_target, epochs=10, verbose=1, 
                    validation_data=(val_scaled, val_target))

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```

    Epoch 1/10
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.5272 - accuracy: 0.8130 - val_loss: 0.4003 - val_accuracy: 0.8572
    Epoch 2/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3944 - accuracy: 0.8576 - val_loss: 0.3871 - val_accuracy: 0.8617
    Epoch 3/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3568 - accuracy: 0.8729 - val_loss: 0.3542 - val_accuracy: 0.8726
    Epoch 4/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3324 - accuracy: 0.8805 - val_loss: 0.3576 - val_accuracy: 0.8765
    Epoch 5/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3198 - accuracy: 0.8861 - val_loss: 0.3562 - val_accuracy: 0.8795
    Epoch 6/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3072 - accuracy: 0.8910 - val_loss: 0.3875 - val_accuracy: 0.8758
    Epoch 7/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2966 - accuracy: 0.8956 - val_loss: 0.3722 - val_accuracy: 0.8764
    Epoch 8/10
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.2872 - accuracy: 0.8982 - val_loss: 0.3690 - val_accuracy: 0.8783
    Epoch 9/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2840 - accuracy: 0.8990 - val_loss: 0.3649 - val_accuracy: 0.8838
    Epoch 10/10
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2745 - accuracy: 0.9041 - val_loss: 0.4154 - val_accuracy: 0.8708
    


    
![png](/Images/0405_Neural_Network_Model/output_17_1.png)
    



```python
# 에포크 20으로 늘림
model = model_fn()
model.compile(loss='sparse_categorical_crossentropy', metrics='accuracy')

history = model.fit(train_scaled, train_target, epochs=20, verbose=1, 
                    validation_data=(val_scaled, val_target))

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```

    Epoch 1/20
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.5319 - accuracy: 0.8131 - val_loss: 0.4284 - val_accuracy: 0.8492
    Epoch 2/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3936 - accuracy: 0.8571 - val_loss: 0.3844 - val_accuracy: 0.8645
    Epoch 3/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3560 - accuracy: 0.8704 - val_loss: 0.3669 - val_accuracy: 0.8715
    Epoch 4/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3353 - accuracy: 0.8792 - val_loss: 0.3503 - val_accuracy: 0.8792
    Epoch 5/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3200 - accuracy: 0.8857 - val_loss: 0.3798 - val_accuracy: 0.8707
    Epoch 6/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3089 - accuracy: 0.8902 - val_loss: 0.3681 - val_accuracy: 0.8759
    Epoch 7/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2984 - accuracy: 0.8945 - val_loss: 0.4130 - val_accuracy: 0.8653
    Epoch 8/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2925 - accuracy: 0.8955 - val_loss: 0.3637 - val_accuracy: 0.8847
    Epoch 9/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2822 - accuracy: 0.9012 - val_loss: 0.3923 - val_accuracy: 0.8762
    Epoch 10/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2762 - accuracy: 0.9031 - val_loss: 0.3755 - val_accuracy: 0.8808
    Epoch 11/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2726 - accuracy: 0.9040 - val_loss: 0.3748 - val_accuracy: 0.8794
    Epoch 12/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2631 - accuracy: 0.9085 - val_loss: 0.3988 - val_accuracy: 0.8809
    Epoch 13/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2600 - accuracy: 0.9087 - val_loss: 0.4310 - val_accuracy: 0.8813
    Epoch 14/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2549 - accuracy: 0.9116 - val_loss: 0.4131 - val_accuracy: 0.8844
    Epoch 15/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2533 - accuracy: 0.9139 - val_loss: 0.4343 - val_accuracy: 0.8820
    Epoch 16/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2479 - accuracy: 0.9151 - val_loss: 0.4254 - val_accuracy: 0.8862
    Epoch 17/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2406 - accuracy: 0.9166 - val_loss: 0.4298 - val_accuracy: 0.8858
    Epoch 18/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2369 - accuracy: 0.9176 - val_loss: 0.4401 - val_accuracy: 0.8852
    Epoch 19/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2347 - accuracy: 0.9192 - val_loss: 0.4332 - val_accuracy: 0.8883
    Epoch 20/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2316 - accuracy: 0.9211 - val_loss: 0.4471 - val_accuracy: 0.8796
    


    
![png](/Images/0405_Neural_Network_Model/output_18_1.png)
    



```python
# 옵티마이저 적용
model = model_fn()
model.compile(optimizer= 'adam', loss='sparse_categorical_crossentropy', metrics='accuracy')

history = model.fit(train_scaled, train_target, epochs=20, verbose=1, 
                    validation_data=(val_scaled, val_target))

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```

    Epoch 1/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.5265 - accuracy: 0.8185 - val_loss: 0.4242 - val_accuracy: 0.8491
    Epoch 2/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3969 - accuracy: 0.8575 - val_loss: 0.3747 - val_accuracy: 0.8667
    Epoch 3/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3550 - accuracy: 0.8715 - val_loss: 0.3502 - val_accuracy: 0.8747
    Epoch 4/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3292 - accuracy: 0.8795 - val_loss: 0.3671 - val_accuracy: 0.8695
    Epoch 5/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3079 - accuracy: 0.8876 - val_loss: 0.3563 - val_accuracy: 0.8748
    Epoch 6/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2941 - accuracy: 0.8918 - val_loss: 0.3482 - val_accuracy: 0.8759
    Epoch 7/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2816 - accuracy: 0.8972 - val_loss: 0.3568 - val_accuracy: 0.8693
    Epoch 8/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2702 - accuracy: 0.9002 - val_loss: 0.3165 - val_accuracy: 0.8883
    Epoch 9/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2575 - accuracy: 0.9045 - val_loss: 0.3286 - val_accuracy: 0.8808
    Epoch 10/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2501 - accuracy: 0.9072 - val_loss: 0.3677 - val_accuracy: 0.8698
    Epoch 11/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2387 - accuracy: 0.9115 - val_loss: 0.3232 - val_accuracy: 0.8850
    Epoch 12/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2338 - accuracy: 0.9126 - val_loss: 0.3256 - val_accuracy: 0.8865
    Epoch 13/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2259 - accuracy: 0.9145 - val_loss: 0.3364 - val_accuracy: 0.8852
    Epoch 14/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2178 - accuracy: 0.9184 - val_loss: 0.3426 - val_accuracy: 0.8797
    Epoch 15/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2121 - accuracy: 0.9204 - val_loss: 0.3343 - val_accuracy: 0.8855
    Epoch 16/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2076 - accuracy: 0.9230 - val_loss: 0.3224 - val_accuracy: 0.8903
    Epoch 17/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2025 - accuracy: 0.9222 - val_loss: 0.3432 - val_accuracy: 0.8807
    Epoch 18/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.1963 - accuracy: 0.9260 - val_loss: 0.3421 - val_accuracy: 0.8889
    Epoch 19/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.1904 - accuracy: 0.9277 - val_loss: 0.3387 - val_accuracy: 0.8859
    Epoch 20/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.1869 - accuracy: 0.9308 - val_loss: 0.3481 - val_accuracy: 0.8863
    


    
![png](/Images/0405_Neural_Network_Model/output_19_1.png)
    


# 드롭아웃
- 제프리 힌턴
- 기본적으로 모든 파라미터를 연산하는것이 원칙
  - but. 일부 뉴런에서 출력이 없는 뉴런 발생
  - --> 기존 일부 뉴런은 계산에서 제외 시킴
- 인공신경망 (뇌과학)
  - 값이 쏠림 현상이 발생 %= 뇌에 피가 고인 현상 %= 뇌출혈
  


```python
model = model_fn(keras.layers.Dropout(0.3))
model.summary()
```

    Model: "sequential_6"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     flatten_6 (Flatten)         (None, 784)               0         
                                                                     
     dense_12 (Dense)            (None, 100)               78500     
                                                                     
     dropout (Dropout)           (None, 100)               0         
                                                                     
     dense_13 (Dense)            (None, 10)                1010      
                                                                     
    =================================================================
    Total params: 79,510
    Trainable params: 79,510
    Non-trainable params: 0
    _________________________________________________________________
    


```python
# 드롭아웃 적용
model.compile(optimizer= 'adam', loss='sparse_categorical_crossentropy', metrics='accuracy')

history = model.fit(train_scaled, train_target, epochs=20, verbose=1, 
                    validation_data=(val_scaled, val_target))

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```

    Epoch 1/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.5938 - accuracy: 0.7927 - val_loss: 0.4280 - val_accuracy: 0.8453
    Epoch 2/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.4419 - accuracy: 0.8411 - val_loss: 0.4036 - val_accuracy: 0.8522
    Epoch 3/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.4047 - accuracy: 0.8537 - val_loss: 0.3683 - val_accuracy: 0.8670
    Epoch 4/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3828 - accuracy: 0.8608 - val_loss: 0.3504 - val_accuracy: 0.8707
    Epoch 5/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3714 - accuracy: 0.8635 - val_loss: 0.3404 - val_accuracy: 0.8767
    Epoch 6/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3539 - accuracy: 0.8703 - val_loss: 0.3410 - val_accuracy: 0.8760
    Epoch 7/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3483 - accuracy: 0.8721 - val_loss: 0.3377 - val_accuracy: 0.8776
    Epoch 8/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3357 - accuracy: 0.8763 - val_loss: 0.3315 - val_accuracy: 0.8807
    Epoch 9/20
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3314 - accuracy: 0.8773 - val_loss: 0.3301 - val_accuracy: 0.8771
    Epoch 10/20
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3214 - accuracy: 0.8812 - val_loss: 0.3274 - val_accuracy: 0.8787
    Epoch 11/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3180 - accuracy: 0.8831 - val_loss: 0.3325 - val_accuracy: 0.8813
    Epoch 12/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.3121 - accuracy: 0.8852 - val_loss: 0.3194 - val_accuracy: 0.8821
    Epoch 13/20
    1500/1500 [==============================] - 4s 2ms/step - loss: 0.3078 - accuracy: 0.8860 - val_loss: 0.3408 - val_accuracy: 0.8773
    Epoch 14/20
    1500/1500 [==============================] - 4s 3ms/step - loss: 0.3026 - accuracy: 0.8865 - val_loss: 0.3316 - val_accuracy: 0.8817
    Epoch 15/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2940 - accuracy: 0.8890 - val_loss: 0.3322 - val_accuracy: 0.8831
    Epoch 16/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2955 - accuracy: 0.8900 - val_loss: 0.3196 - val_accuracy: 0.8870
    Epoch 17/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2919 - accuracy: 0.8908 - val_loss: 0.3243 - val_accuracy: 0.8853
    Epoch 18/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2835 - accuracy: 0.8925 - val_loss: 0.3362 - val_accuracy: 0.8830
    Epoch 19/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2844 - accuracy: 0.8926 - val_loss: 0.3243 - val_accuracy: 0.8819
    Epoch 20/20
    1500/1500 [==============================] - 3s 2ms/step - loss: 0.2812 - accuracy: 0.8943 - val_loss: 0.3184 - val_accuracy: 0.8881
    


    
![png](/Images/0405_Neural_Network_Model/output_22_1.png)
    


    - 드롭아웃을 적용했더니 과대적합 되던 모형이 많이 완화됨.

# 모댈 저장과 복원
- 개발자: 정확도는 중요하지 않음
  - 딥러닝 모델 활용해서 웹앱을 개발하는게 목적
- 분석가 & 머신러닝 엔지니어: 캐글 대회 (정확도 검증 필수)


```python
model.save_weights('model-weights.h5')
model.save('model-whole.h5')
```

- 모델 불러오기


```python
model = model_fn(keras.layers.Dropout(0.3))
model.load_weights('model-weights.h5')
```


```python
import numpy as np

val_labels = np.argmax(model.predict(val_scaled), axis=-1)
print(np.mean(val_labels == val_target))
```

    0.8810833333333333
    

# 콜백
- model.checkpoint(): 체크포인트를 기록


```python
model = model_fn(keras.layers.Dropout(0.3))
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', 
              metrics='accuracy')

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-model.h5', 
                                                save_best_only=True)

model.fit(train_scaled, train_target, epochs=20, verbose=0, 
          validation_data=(val_scaled, val_target),
          callbacks=[checkpoint_cb])
```




    <keras.callbacks.History at 0x7f673a848ad0>




```python
model = keras.models.load_model('best-model.h5')
model.evaluate(val_scaled, val_target)
```

    375/375 [==============================] - 1s 1ms/step - loss: 0.3137 - accuracy: 0.8893
    




    [0.31372642517089844, 0.8893333077430725]



- Early Stopping
  - 조기 종료
  - 에포크를 많이 주면 많이 줄수록 성능이 좋아야 하는 것이 원리 (가중치 업데이트 / 기울기가 계속 미분)
  - if) 에포크 100 / 50 에포크 시점과 90에포크 시점 성능 차이 없음


```python
model = model_fn(keras.layers.Dropout(0.3))
model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', 
              metrics='accuracy')

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=2,
                                                  restore_best_weights=True)

history = model.fit(train_scaled, train_target, epochs=20, verbose=0, 
                    validation_data=(val_scaled, val_target),
                    callbacks=[checkpoint_cb, early_stopping_cb])
```


```python
print(early_stopping_cb.stopped_epoch)
```

    14
    


```python
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```


    
![png](/Images/0405_Neural_Network_Model/output_35_0.png)
    


# 마무리 정리
- 키워드:
  - 드롭아웃: 은닉층에 있는 뉴런의 출력을 랜덤하게 꺼서 과대적합을 막는 기법
  - 콜백: 케라스 모델을 훈련하는 도중에 어떤 작업을 수행 할 수 있도록 도와주는 도구
  - 조기종료: 검증점수가 더 이상 감소하지 않고 상승하여 과대적합이 일어나면 훈련을 계속 진행하지 않고 멈추는 기법

- TensorFlow 패키지
  - Dropout: 드롭아웃 층
  - save_weights(): 모든 층의 가중치와 절편을 파일에 저장
  - load_weights(): 모든 층의 가중치와 절편을 파일에 읽음
  - save(): 모델 구조와 모든 가중치와 절편을 파일에 저장
  - load_model(): model.save()로 저장된 모델을 로드
  - ModelCheckpoint: 케라스 모델과 가중치를 일정 간격으로 저장
  - EarlyStopping: 관심지표가 더이상 향상하지 않으면 훈련을 중지
- NumPy 패키지
  - argmax: 배열에서 축을 따라 최댓값의 인덱스 반환
    - 매개변수 axis: 어떤축을 따라 최댓값을 찾을지 지정 [default: None] (전체 배열에서 최댓값)
