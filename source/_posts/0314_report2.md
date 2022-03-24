---
title: "마크다운 연습"
author: "woobin"
date: '2022-03-14'
output: 
  html_document:
    keep_md: true
---



## 4. 데이터 전처리
### 1) 분석 파일을 R로 불러오기


```r
library(dplyr)
```

```
## 
## 다음의 패키지를 부착합니다: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(ggplot2)
```

- 메뉴얼 : https://bookdown.org/yihui/rmarkdown/

### 2) 시각화 코드

- 데이터를 불러옵니다

```r
ggplot(iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point()
```

![](/Images/report2_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

