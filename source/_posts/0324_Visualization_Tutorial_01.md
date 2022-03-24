---
title: "시각화 연습1"
author: "woobin"
date: '2022-03-24'
---

# 라이브러리 불러오기


```python
import matplotlib
import seaborn as sns
print(matplotlib.__version__)
print(sns.__version__)
```

    3.2.2
    0.11.2
    

# 시각화 그려보기


```python
import matplotlib.pyplot as plt

dates = [
    '2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04', '2021-01-05',
    '2021-01-06', '2021-01-07', '2021-01-08', '2021-01-09', '2021-01-10'
]
min_temperature = [20.7, 17.9, 18.8, 14.6, 15.8, 15.8, 15.8, 17.4, 21.8, 20.0]
max_temperature = [34.7, 28.9, 31.8, 25.6, 28.8, 21.8, 22.8, 28.4, 30.8, 32.0]

# 앞으로 아래와 같이 코드를 작성하면 됩니다.
fig, ax = plt.subplots(nrows = 1, ncols = 1, figsize = (10, 6))
ax.plot(dates, min_temperature, label = "Min Temp.")
ax.plot(dates, max_temperature, label = "Max Temp.")
ax.legend()
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_3_0.png)
    


# 주식 데이터 다운로드 받기


```python
!pip install yfinance --upgrade --no-cache-dir
```

    Collecting yfinance
      Downloading yfinance-0.1.70-py2.py3-none-any.whl (26 kB)
    Requirement already satisfied: numpy>=1.15 in /usr/local/lib/python3.7/dist-packages (from yfinance) (1.21.5)
    Requirement already satisfied: pandas>=0.24.0 in /usr/local/lib/python3.7/dist-packages (from yfinance) (1.3.5)
    Collecting requests>=2.26
      Downloading requests-2.27.1-py2.py3-none-any.whl (63 kB)
    [K     |████████████████████████████████| 63 kB 6.4 MB/s 
    [?25hCollecting lxml>=4.5.1
      Downloading lxml-4.8.0-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl (6.4 MB)
    [K     |████████████████████████████████| 6.4 MB 11.0 MB/s 
    [?25hRequirement already satisfied: multitasking>=0.0.7 in /usr/local/lib/python3.7/dist-packages (from yfinance) (0.0.10)
    Requirement already satisfied: pytz>=2017.3 in /usr/local/lib/python3.7/dist-packages (from pandas>=0.24.0->yfinance) (2018.9)
    Requirement already satisfied: python-dateutil>=2.7.3 in /usr/local/lib/python3.7/dist-packages (from pandas>=0.24.0->yfinance) (2.8.2)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.7/dist-packages (from python-dateutil>=2.7.3->pandas>=0.24.0->yfinance) (1.15.0)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.7/dist-packages (from requests>=2.26->yfinance) (2021.10.8)
    Requirement already satisfied: urllib3<1.27,>=1.21.1 in /usr/local/lib/python3.7/dist-packages (from requests>=2.26->yfinance) (1.24.3)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.7/dist-packages (from requests>=2.26->yfinance) (2.10)
    Requirement already satisfied: charset-normalizer~=2.0.0 in /usr/local/lib/python3.7/dist-packages (from requests>=2.26->yfinance) (2.0.12)
    Installing collected packages: requests, lxml, yfinance
      Attempting uninstall: requests
        Found existing installation: requests 2.23.0
        Uninstalling requests-2.23.0:
          Successfully uninstalled requests-2.23.0
      Attempting uninstall: lxml
        Found existing installation: lxml 4.2.6
        Uninstalling lxml-4.2.6:
          Successfully uninstalled lxml-4.2.6
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    google-colab 1.0.0 requires requests~=2.23.0, but you have requests 2.27.1 which is incompatible.
    datascience 0.10.6 requires folium==0.2.1, but you have folium 0.8.3 which is incompatible.[0m
    Successfully installed lxml-4.8.0 requests-2.27.1 yfinance-0.1.70
    


```python
import yfinance as yf
data = yf.download("AAPL", start= "2019-08-01", end="2022-03-23")
ts = data["Open"]
print(ts.head())
print(type(ts))
```

    [*********************100%***********************]  1 of 1 completed
    Date
    2019-08-01    53.474998
    2019-08-02    51.382500
    2019-08-05    49.497501
    2019-08-06    49.077499
    2019-08-07    48.852501
    Name: Open, dtype: float64
    <class 'pandas.core.series.Series'>
    

# pyplot 형태


```python
import matplotlib.pyplot as plt
plt.plot(ts)
plt.title("Stokc Market of AAPL")
plt.xlabel("Date")
plt.ylabel("Open Price")
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_8_0.png)
    


# 객체지향으로 그리기


```python
# 필수  fig ax = plt.subplots()
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.plot(ts)
ax.set_title("Stokc Market of AAPL")
ax.set_xlabel("Date")
ax.set_ylabel("Open Price")
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_10_0.png)
    


# 막대 그래프


```python
import matplotlib.pyplot as plt
import numpy as np
import calendar

month_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
sold_list = [300, 400, 550, 900, 600, 960, 900, 910, 800, 700, 550, 450]

# print(month_list)
# print(sold_list)

fig, ax= plt.subplots(figsize = (10, 6))
barplots = ax.bar(month_list, sold_list)

print("barplots: ", barplots)

for plot in barplots:
  print(plot)
  # print(plot.get_height())
  # print(plot.get_x())
  # print(plot.get_y())
  # print(plot.get_width())
  height = plot.get_height()
  ax.text(plot.get_x() + plot.get_width()/2, height, height, ha = "center", va = "bottom")

plt.xticks(month_list, calendar.month_name[1:13])
plt.show()
```

    barplots:  <BarContainer object of 12 artists>
    Rectangle(xy=(0.6, 0), width=0.8, height=300, angle=0)
    Rectangle(xy=(1.6, 0), width=0.8, height=400, angle=0)
    Rectangle(xy=(2.6, 0), width=0.8, height=550, angle=0)
    Rectangle(xy=(3.6, 0), width=0.8, height=900, angle=0)
    Rectangle(xy=(4.6, 0), width=0.8, height=600, angle=0)
    Rectangle(xy=(5.6, 0), width=0.8, height=960, angle=0)
    Rectangle(xy=(6.6, 0), width=0.8, height=900, angle=0)
    Rectangle(xy=(7.6, 0), width=0.8, height=910, angle=0)
    Rectangle(xy=(8.6, 0), width=0.8, height=800, angle=0)
    Rectangle(xy=(9.6, 0), width=0.8, height=700, angle=0)
    Rectangle(xy=(10.6, 0), width=0.8, height=550, angle=0)
    Rectangle(xy=(11.6, 0), width=0.8, height=450, angle=0)
    


    
![png](/Images/visualization_tutorial_01/output_12_1.png)
    



```python
from IPython.core.pylabtools import figsize
import seaborn as sns

tips = sns.load_dataset("tips")
x= tips["total_bill"]
y= tips["tip"]

# 산점도
fig, ax= plt.subplots(figsize= (10, 6))   # figsize=(가로길이, 세로길이)
ax.scatter(x, y)
ax.set_xlabel("Total_Bill")
ax.set_ylabel("Tip")
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_13_0.png)
    



```python
label, data = tips.groupby("sex")
#print(label)
#print(data)

tips["sex_color"] = tips["sex"].map({"Male": "#2521F6", "Female": "#EB4036"})
# print(tips.head())
fig, ax = plt.subplots(figsize= (10, 6))
for label, data in tips.groupby("sex"):
  # print(label)
  # print(data)
  ax.scatter(data["total_bill"], data["tip"], label = label, color = data["sex_color"], alpha = 0.5)
  ax.set_xlabel("Total_Bill")
  ax.set_ylabel("Tip")

ax.legend()  # 범례
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_14_0.png)
    


## seaborn


```python
import matplotlib.pyplot as plt
import seaborn as sns

tips = sns.load_dataset("tips")

fig, ax = plt.subplots(figsize=(10, 6))
sns.scatterplot(x= "total_bill", y ="tip", hue = "sex", data = tips)
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_16_0.png)
    



```python
# 두개의 그래프를 동시에 표현
fig, ax = plt.subplots(nrows= 1 , ncols=2, figsize=(15, 5))
sns.regplot(x= "total_bill", y ="tip", data = tips, ax = ax[1], fit_reg = True)
ax[1].set_title("with linear regression line")

sns.regplot(x= "total_bill", y ="tip", data = tips, ax = ax[0], fit_reg = False)  # ax = ax[0] 선 있음
ax[0].set_title("without linear regression line")
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_17_0.png)
    


- 막대 그래프 그리기 seaborn 방식


```python
sns.countplot(x= "day", data = tips)
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_19_0.png)
    



```python
print(tips["day"].value_counts().index)
print(tips["day"].value_counts().values)
print(tips["day"].value_counts(ascending= True))  # 오름차순
```

    CategoricalIndex(['Sat', 'Sun', 'Thur', 'Fri'], categories=['Thur', 'Fri', 'Sat', 'Sun'], ordered=False, dtype='category')
    [87 76 62 19]
    Fri     19
    Thur    62
    Sun     76
    Sat     87
    Name: day, dtype: int64
    


```python
fig, ax = plt.subplots()
ax = sns.countplot(x = "day", data= tips, order= tips["day"].value_counts().index)

for plot in ax.patches:  # matplotlib
  # print(plot)
  height = plot.get_height()
  ax.text(plot.get_x() + plot.get_width()/2, height, height, ha = "center", va = "bottom")

ax.set_ylim(-5, 100)  # y축을 100까지 확장
plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_21_0.png)
    


## 어려운 시각화 그래프


```python
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from matplotlib.ticker import (MultipleLocator, AutoMinorLocator, FuncFormatter)  # ticker : 증권 시세 표시기

def major_formatter(x, pos):
  return "%.2f$" % x

formatter = FuncFormatter(major_formatter)

tips = sns.load_dataset("tips")
fig, ax = plt.subplots(nrows = 1, ncols= 2, figsize= (16, 6))

ax0 = sns.barplot(x= "day", y= "total_bill", data= tips,
                  ci = None, color= "lightgray", alpha= 0.85, zorder= 2,
                  ax= ax[0])

# groupby
group_mean = tips.groupby(["day"])["total_bill"].agg("mean")
# print(group_mean)

h_day = group_mean.sort_values(ascending= False).index[0]
# print(h_day)
h_mean= group_mean.sort_values(ascending= False).values[0]
# print(h_mean)

# text 추가
for plot in ax0.patches:
  height = np.round(plot.get_height(), 2)
  # print(height)

  # Default
  fontweight = "normal"
  color = "k"
  if h_mean == height:
    fontweight = "bold"
    color = "darkred"
    plot.set_facecolor(color)
    plot.set_edgecolor("black")

  ax0.text(plot.get_x()+ plot.get_width()/2.,
           height + 1, height,
           ha = "center", size= 12, fontweight= fontweight, color = color)

# 축 수정
ax0.set_ylim(-3,30)
ax0.set_title("Bar Graph", size= 16)

# spines 제거
ax0.spines["top"].set_visible(False)
ax0.spines["left"].set_position(("outward", 20))
ax0.spines["left"].set_visible(False)
ax0.spines["right"].set_visible(False)

ax0.yaxis.set_major_locator(MultipleLocator(10))
ax0.yaxis.set_major_formatter(formatter)
ax0.yaxis.set_minor_locator(MultipleLocator(5))

ax0.set_ylabel("Avg. Total Bill($)", fontsize=14)

ax0.grid(axis="y", which="major", color= "lightgray")
ax0.grid(axis="y", which="minor", ls= ":")

ax0.set_xlabel("Weekday", fontsize= 14)

for xtick in ax0.get_xticklabels():
  # print(xtick)
  if xtick.get_text() == h_day:
    xtick.set_color("darkred")
    xtick.set_fontweight("demibold")

ax0.set_xticklabels(["Thursday", "Friday", "Saturday", "Sunday"], size = 12)

plt.show()
```


    
![png](/Images/visualization_tutorial_01/output_23_0.png)
    

