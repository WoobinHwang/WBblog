---
title: "LSTM과 GRU셀"
author: "woobin"
date: '2022-04-08'
---

# LSTM과 GPU셀

# LSTM 신경망 훈련하기
- RNN은 실무에서 안씀.
- LSTM이 나온 배경
  - RNN -> 문장이 길면, 학습 능력이 떨어지게됨
  - Long Short-Term Memory
- 단기 기억을 오래 기억하기 위해 고안됨.
  - 메모장에 적듯이 업무 처리함.

# 데이터 불러오기


```python
from tensorflow.keras.datasets import imdb
from sklearn.model_selection import train_test_split

(train_input, train_target), (test_input, test_target) = imdb.load_data(
    num_words=500)

train_input, val_input, train_target, val_target = train_test_split(
    train_input, train_target, test_size=0.2, random_state=42)
```

    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/imdb.npz
    17465344/17464789 [==============================] - 0s 0us/step
    17473536/17464789 [==============================] - 0s 0us/step
    

## padding


```python
from tensorflow.keras.preprocessing.sequence import pad_sequences

train_seq = pad_sequences(train_input, maxlen=100)
val_seq = pad_sequences(val_input, maxlen=100)
```

## 모형 만들기


```python
from tensorflow import keras
model = keras.Sequential()
model.add(keras.layers.Embedding(500, 16, input_length= 100))
model.add(keras.layers.LSTM(8))
model.add(keras.layers.Dense(1, activation= 'sigmoid'))
```


```python
model.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     embedding (Embedding)       (None, 100, 16)           8000      
                                                                     
     lstm (LSTM)                 (None, 8)                 800       
                                                                     
     dense (Dense)               (None, 1)                 9         
                                                                     
    =================================================================
    Total params: 8,809
    Trainable params: 8,809
    Non-trainable params: 0
    _________________________________________________________________
    


```python
rmsprop = keras.optimizers.RMSprop(learning_rate=1e-4)
model.compile(optimizer=rmsprop, loss='binary_crossentropy', 
              metrics=['accuracy'])

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-lstm-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=3,
                                                  restore_best_weights=True)

# 원본 epochs = 100
history = model.fit(train_seq, train_target, epochs=10, batch_size=64,
                    validation_data=(val_seq, val_target),
                    callbacks=[checkpoint_cb, early_stopping_cb])
```

    Epoch 1/10
    313/313 [==============================] - 20s 57ms/step - loss: 0.6929 - accuracy: 0.5167 - val_loss: 0.6923 - val_accuracy: 0.5706
    Epoch 2/10
    313/313 [==============================] - 15s 46ms/step - loss: 0.6914 - accuracy: 0.6021 - val_loss: 0.6902 - val_accuracy: 0.6312
    Epoch 3/10
    313/313 [==============================] - 16s 51ms/step - loss: 0.6869 - accuracy: 0.6435 - val_loss: 0.6816 - val_accuracy: 0.5628
    Epoch 4/10
    313/313 [==============================] - 14s 45ms/step - loss: 0.6530 - accuracy: 0.6578 - val_loss: 0.6313 - val_accuracy: 0.7128
    Epoch 5/10
    313/313 [==============================] - 13s 42ms/step - loss: 0.6149 - accuracy: 0.7222 - val_loss: 0.6105 - val_accuracy: 0.7282
    Epoch 6/10
    313/313 [==============================] - 13s 42ms/step - loss: 0.5949 - accuracy: 0.7358 - val_loss: 0.5939 - val_accuracy: 0.7382
    Epoch 7/10
    313/313 [==============================] - 13s 42ms/step - loss: 0.5780 - accuracy: 0.7503 - val_loss: 0.5786 - val_accuracy: 0.7452
    Epoch 8/10
    313/313 [==============================] - 13s 43ms/step - loss: 0.5623 - accuracy: 0.7577 - val_loss: 0.5654 - val_accuracy: 0.7466
    Epoch 9/10
    313/313 [==============================] - 13s 42ms/step - loss: 0.5480 - accuracy: 0.7663 - val_loss: 0.5516 - val_accuracy: 0.7574
    Epoch 10/10
    313/313 [==============================] - 13s 42ms/step - loss: 0.5343 - accuracy: 0.7711 - val_loss: 0.5400 - val_accuracy: 0.7602
    

## 손실 곡선 추가


```python
import matplotlib.pyplot as plt

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```


    
![png](/Images/0408_LSTM,GRU_Cell/output_11_0.png)
    


## 순환층에 드롭아웃 적용하기


```python
model2 = keras.Sequential()

model2.add(keras.layers.Embedding(500, 16, input_length=100))
# 드롭아웃 추가
model2.add(keras.layers.LSTM(8, dropout=0.3))
model2.add(keras.layers.Dense(1, activation='sigmoid'))

rmsprop = keras.optimizers.RMSprop(learning_rate=1e-4)
model2.compile(optimizer=rmsprop, loss='binary_crossentropy', 
               metrics=['accuracy'])

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-dropout-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=3,
                                                  restore_best_weights=True)

# epcohs = 100
history = model2.fit(train_seq, train_target, epochs=5, batch_size=64,
                     validation_data=(val_seq, val_target),
                     callbacks=[checkpoint_cb, early_stopping_cb])

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```

    Epoch 1/5
    313/313 [==============================] - 26s 74ms/step - loss: 0.6925 - accuracy: 0.5351 - val_loss: 0.6917 - val_accuracy: 0.5744
    Epoch 2/5
    313/313 [==============================] - 14s 44ms/step - loss: 0.6902 - accuracy: 0.5934 - val_loss: 0.6881 - val_accuracy: 0.6306
    Epoch 3/5
    313/313 [==============================] - 14s 45ms/step - loss: 0.6817 - accuracy: 0.6486 - val_loss: 0.6711 - val_accuracy: 0.6736
    Epoch 4/5
    313/313 [==============================] - 14s 44ms/step - loss: 0.6357 - accuracy: 0.6889 - val_loss: 0.6024 - val_accuracy: 0.6984
    Epoch 5/5
    313/313 [==============================] - 14s 45ms/step - loss: 0.5847 - accuracy: 0.7155 - val_loss: 0.5686 - val_accuracy: 0.7286
    


    
![png](/Images/0408_LSTM,GRU_Cell/output_13_1.png)
    


# 마무리 정리
- 키워드
  - **LSTM**: 타임스텝이 긴 데이터를 효과적으로 학습하기 위해 고안된 순환층.
  - **셀 상태**: LSTM셀은 은닉 상태 외에 셀상태를 출력. 셀 상태는 다음 층으로 전달되지 않으며 현재 셀에서만 순환됨.
  - **GPU**: LSTM셀의 간소화 버전으로 생각 할 수 있지만 LSTM셀에 못지않는 성능을 냅니다.
- **TensorFlow 패키지**
  - **LSTM**: LSTM셀을 사용한 순환층 클래스
  - **GRU**: GRU셀을 사용한 순환층 클래스
