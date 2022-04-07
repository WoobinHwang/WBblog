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

    Model: "sequential_5"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     conv2d_9 (Conv2D)           (None, 28, 28, 32)        320       
                                                                     
     average_pooling2d_4 (Averag  (None, 9, 9, 32)         0         
     ePooling2D)                                                     
                                                                     
     conv2d_10 (Conv2D)          (None, 9, 9, 32)          9248      
                                                                     
     average_pooling2d_5 (Averag  (None, 3, 3, 32)         0         
     ePooling2D)                                                     
                                                                     
     flatten_4 (Flatten)         (None, 288)               0         
                                                                     
     dense_8 (Dense)             (None, 100)               28900     
                                                                     
     dropout_4 (Dropout)         (None, 100)               0         
                                                                     
     dense_9 (Dense)             (None, 10)                1010      
                                                                     
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

    Model: "sequential_6"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     conv2d_11 (Conv2D)          (None, 28, 28, 32)        320       
                                                                     
     max_pooling2d_4 (MaxPooling  (None, 14, 14, 32)       0         
     2D)                                                             
                                                                     
     conv2d_12 (Conv2D)          (None, 14, 14, 64)        18496     
                                                                     
     max_pooling2d_5 (MaxPooling  (None, 7, 7, 64)         0         
     2D)                                                             
                                                                     
     flatten_5 (Flatten)         (None, 3136)              0         
                                                                     
     dense_10 (Dense)            (None, 100)               313700    
                                                                     
     dropout_5 (Dropout)         (None, 100)               0         
                                                                     
     dense_11 (Dense)            (None, 10)                1010      
                                                                     
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




    
![png](output_9_0.png)
    




```python
# 실험 코드
keras.utils.plot_model(model, show_shapes=True,
                       dpi= 50)

# dpi로 크기를 조절 할 수 있음 [default: 96]
```




    
![png](output_10_0.png)
    



- 매개변수 show_shapes를 적용하여 입력과 출력의 크기를 표시


```python
keras.utils.plot_model(model, show_shapes=True)
```




    
![png](output_12_0.png)
    



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

    Epoch 1/10
    1500/1500 [==============================] - 7s 4ms/step - loss: 0.5228 - accuracy: 0.8134 - val_loss: 0.3351 - val_accuracy: 0.8783
    Epoch 2/10
    1500/1500 [==============================] - 5s 3ms/step - loss: 0.3461 - accuracy: 0.8764 - val_loss: 0.2777 - val_accuracy: 0.8954
    Epoch 3/10
    1500/1500 [==============================] - 5s 3ms/step - loss: 0.2951 - accuracy: 0.8924 - val_loss: 0.2575 - val_accuracy: 0.9026
    Epoch 4/10
    1500/1500 [==============================] - 5s 4ms/step - loss: 0.2621 - accuracy: 0.9051 - val_loss: 0.2330 - val_accuracy: 0.9120
    Epoch 5/10
    1500/1500 [==============================] - 6s 4ms/step - loss: 0.2377 - accuracy: 0.9129 - val_loss: 0.2234 - val_accuracy: 0.9152
    Epoch 6/10
    1500/1500 [==============================] - 6s 4ms/step - loss: 0.2186 - accuracy: 0.9200 - val_loss: 0.2219 - val_accuracy: 0.9162
    Epoch 7/10
    1500/1500 [==============================] - 6s 4ms/step - loss: 0.2023 - accuracy: 0.9264 - val_loss: 0.2159 - val_accuracy: 0.9214
    Epoch 8/10
    1500/1500 [==============================] - 6s 4ms/step - loss: 0.1872 - accuracy: 0.9312 - val_loss: 0.2125 - val_accuracy: 0.9229
    Epoch 9/10
    1500/1500 [==============================] - 6s 4ms/step - loss: 0.1722 - accuracy: 0.9358 - val_loss: 0.2129 - val_accuracy: 0.9258
    Epoch 10/10
    1500/1500 [==============================] - 6s 4ms/step - loss: 0.1611 - accuracy: 0.9376 - val_loss: 0.2283 - val_accuracy: 0.9199
    

- 모델 학습 곡선 그리기


```python
# 연습 코드: plotly로 그려보기
import plotly.express as px
from plotly.subplots import make_subplots
import plotly.graph_objects as go
import matplotlib.pyplot as plt

fig = make_subplots()

fig.add_trace(go.Bar(y= history.history['loss']))
fig.add_trace(go.Scatter(y= history.history['val_loss']))

fig.show()
```


[//]: # (<html>)

[//]: # (<head><meta charset="utf-8" /></head>)

[//]: # (<body>)

[//]: # (    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if &#40;window.MathJax&#41; {MathJax.Hub.Config&#40;{SVG: {font: "STIX-Web"}}&#41;;}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>)

[//]: # (        <script src="https://cdn.plot.ly/plotly-2.8.3.min.js"></script>                <div id="c8924ee6-55c6-46f0-8086-9059699fa8f5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if &#40;document.getElementById&#40;"c8924ee6-55c6-46f0-8086-9059699fa8f5"&#41;&#41; {                    Plotly.newPlot&#40;                        "c8924ee6-55c6-46f0-8086-9059699fa8f5",                        [{"y":[0.5227905511856079,0.34605586528778076,0.29508572816848755,0.262106716632843,0.2377263754606247,0.21863968670368195,0.20233646035194397,0.18718752264976501,0.1721685230731964,0.1610814332962036],"type":"bar"},{"y":[0.3351101279258728,0.27766725420951843,0.25754213333129883,0.23295046389102936,0.22336450219154358,0.22187326848506927,0.21594305336475372,0.21249684691429138,0.21287128329277039,0.22832433879375458],"type":"scatter"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0]},"yaxis":{"anchor":"x","domain":[0.0,1.0]}},                        {"responsive": true}                    &#41;.then&#40;function&#40;&#41;{)

[//]: # ()
[//]: # (var gd = document.getElementById&#40;'c8924ee6-55c6-46f0-8086-9059699fa8f5'&#41;;)

[//]: # (var x = new MutationObserver&#40;function &#40;mutations, observer&#41; {{)

[//]: # (        var display = window.getComputedStyle&#40;gd&#41;.display;)

[//]: # (        if &#40;!display || display === 'none'&#41; {{)

[//]: # (            console.log&#40;[gd, 'removed!']&#41;;)

[//]: # (            Plotly.purge&#40;gd&#41;;)

[//]: # (            observer.disconnect&#40;&#41;;)

[//]: # (        }})

[//]: # (}}&#41;;)

[//]: # ()
[//]: # (// Listen for the removal of the full notebook cells)

[//]: # (var notebookContainer = gd.closest&#40;'#notebook-container'&#41;;)

[//]: # (if &#40;notebookContainer&#41; {{)

[//]: # (    x.observe&#40;notebookContainer, {childList: true}&#41;;)

[//]: # (}})

[//]: # ()
[//]: # (// Listen for the clearing of the current output cell)

[//]: # (var outputEl = gd.closest&#40;'.output'&#41;;)

[//]: # (if &#40;outputEl&#41; {{)

[//]: # (    x.observe&#40;outputEl, {childList: true}&#41;;)

[//]: # (}})

[//]: # ()
[//]: # (                        }&#41;                };                            </script>        </div>)

[//]: # (</body>)

[//]: # (</html>)



```python
import matplotlib.pyplot as plt
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.legend(['train', 'val'])
plt.show()
```


    
![png](output_19_0.png)
    



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




    [<keras.layers.convolutional.Conv2D at 0x7f72cec86650>,
     <keras.layers.pooling.MaxPooling2D at 0x7f72cec86b90>,
     <keras.layers.convolutional.Conv2D at 0x7f72cec7ded0>,
     <keras.layers.pooling.MaxPooling2D at 0x7f7394985a90>,
     <keras.layers.core.flatten.Flatten at 0x7f72cec8d250>,
     <keras.layers.core.dense.Dense at 0x7f72cec86a10>,
     <keras.layers.core.dropout.Dropout at 0x7f73949e5cd0>,
     <keras.layers.core.dense.Dense at 0x7f73949cff50>]



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

    -0.054838914 0.28367323
    


```python
plt.hist(conv_weights.reshape(-1, 1))
plt.xlabel('weight')
plt.ylabel('count')
plt.show()
```


    
![png](output_29_0.png)
    


    - 0을 중심으로 종 모양 분포를 띄고 있음


```python
fig, axs = plt.subplots(2, 16, figsize=(15,2))

for i in range(2):
    for j in range(16):
        axs[i, j].imshow(conv_weights[:,:,0,i*16 + j], vmin=-0.5, vmax=0.5)
        axs[i, j].axis('off')

plt.show()
```


    
![png](output_31_0.png)
    


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

    0.0015572025 0.082305014
    


```python
plt.hist(no_training_weights.reshape(-1, 1))
plt.xlabel('weight')
plt.ylabel('count')
plt.show()
```


    
![png](output_36_0.png)
    



```python
fig, axs = plt.subplots(2, 16, figsize=(15,2))

for i in range(2):
    for j in range(16):
        axs[i, j].imshow(no_training_weights[:,:,0,i*16 + j], vmin=-0.5, vmax=0.5)
        axs[i, j].axis('off')

plt.show()
```


    
![png](output_37_0.png)
    


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

    KerasTensor(type_spec=TensorSpec(shape=(None, 28, 28, 1), dtype=tf.float32, name='conv2d_11_input'), name='conv2d_11_input', description="created by layer 'conv2d_11_input'")
    


    
![png](output_41_1.png)
    



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


    
![png](output_44_0.png)
    



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
    


    
![png](output_45_1.png)
    


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


```python

```
