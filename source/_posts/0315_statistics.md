---
title: "분석방법"
author: "Woobin"
date: '2022-03-18'
output: 
  html_document:
    keep_md: true
---



### 기술통계
- 평균, 최솟값, 최댓값, 중앙값과같이 데이터의 특징을 알려주는 값.
- 기술통계(descriptive statistics)라고 함.

### 추론통계
- 변수간의 관계를 파악, 변수간의 인과관계 또는 새로운 사실 밝혀냄
- 평균 차이 검정, 교차분석, 상관관계분석, 회귀분석
- 귀무가설: 두 집단의 평균 차이가 없다. (현상황을 유지 할 필요가 있음)
- 대립가설: 두 집단의 평균 차이가 있다. (현상황을 바꿀 필요가 있음)

## 평균차이검정
- 집단별로 평균의 차이가 실제로 있는가 검정
- 독립표본 t 검정
- 방법1 : t.test(data= 데이터세트, 종속변수(비교값)~독립변수(비교대상))
- 방법2 : t.test(데이터세트$종속변수(비교값)~데이터세트$독립변수(비교대상))


```r
mpg1 = read.csv("source_2021/1_day_eda/solution/data/public_dataset/mpg1.csv", stringsAsFactors = F)

t.test(data = mpg1, cty~ trans)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  cty by trans
## t = -4.5375, df = 132.32, p-value = 1.263e-05
## alternative hypothesis: true difference in means between group auto and group manual is not equal to 0
## 95 percent confidence interval:
##  -3.887311 -1.527033
## sample estimates:
##   mean in group auto mean in group manual 
##             15.96815             18.67532
```
-> p-value = 1.263e-05로 0.05보다 작으므로 평균차이가 있는 대립가설을 채택

## 교차분석
- 범주형 변수로 구성된 집단들의 관련성을 검정하는 통계 분석.


```r
mpg1 = read.csv("source_2021/1_day_eda/solution/data/public_dataset/mpg1.csv", stringsAsFactors = F)
table(mpg1$trans, mpg1$drv)  #trans, drv의 교차분석
```

```
##         
##           4  f  r
##   auto   75 65 17
##   manual 28 41  8
```

```r
prop.table(table(mpg1$trans, mpg1$drv), 1)  #auto, manual의 비율분석
```

```
##         
##                  4         f         r
##   auto   0.4777070 0.4140127 0.1082803
##   manual 0.3636364 0.5324675 0.1038961
```


```r
mpg1 = read.csv("source_2021/1_day_eda/solution/data/public_dataset/mpg1.csv", stringsAsFactors = F)
chisq.test(mpg1$trans, mpg1$drv)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  mpg1$trans and mpg1$drv
## X-squared = 3.1368, df = 2, p-value = 0.2084
```

```r
# == chisq.test(table(mpg1$trans, mpg1$drv))
# == summary(table(mpg1$trans, mpg1$drv))  
```
-> p-value = 0.2084로 0.05보다 크므로 귀무가설을 채택

## 상관관계분석
- 변수간의 상관관계를 알아보는것
- 방법 : cor.test(데이터세트$ 비교변수1, 데이터세트$비교변수2)


```r
mpg1 = read.csv("source_2021/1_day_eda/solution/data/public_dataset/mpg1.csv", stringsAsFactors = F)
cor.test(mpg1$cty, mpg1$hwy)
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  mpg1$cty and mpg1$hwy
## t = 49.585, df = 232, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.9433129 0.9657663
## sample estimates:
##       cor 
## 0.9559159
```
p-value < 2.2e-16로 0.05보다 작으므로 대립가설을 채택

## 회귀분석
- 한 변수가 다른 변수에 영향을 주는것

# 단순회귀분석
- 독립변수가 1개, 종속변수가 1개
- 변수는 독립변수와 종속변수가 모두 등간척도 또는 비율척도이어야합니다.
- 방법 1 : lm(dat= 데이터세트, 종속변수 ~ 독립변수)
- 방법 2 : lm(종속변수 ~ 독립변수, data = 데이터세트)
- 방법 3 : lm(데이터세트$종속변수~ 데이터세트$독립변수)


```r
lm(data= mtcars, mpg~disp)
```

```
## 
## Call:
## lm(formula = mpg ~ disp, data = mtcars)
## 
## Coefficients:
## (Intercept)         disp  
##    29.59985     -0.04122
```
-> 29.59985 - 0.04122 * disp 라는 식이 도출됨.


```r
RA = lm(data= mtcars, mpg~disp)

summary(RA)
```

```
## 
## Call:
## lm(formula = mpg ~ disp, data = mtcars)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.8922 -2.2022 -0.9631  1.6272  7.2305 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 29.599855   1.229720  24.070  < 2e-16 ***
## disp        -0.041215   0.004712  -8.747 9.38e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.251 on 30 degrees of freedom
## Multiple R-squared:  0.7183,	Adjusted R-squared:  0.709 
## F-statistic: 76.51 on 1 and 30 DF,  p-value: 9.38e-10
```
-> p-value: 9.38e-10로 0.05보다 작으므로 회귀모형이 적합
-> 회귀식으로 mpg= -0.04122 * disp + 29.59985 라는 식이 도출됨.

# 다중회귀분석
- 종속변수에 영향을 주는 독립변수가 복수일때 분석하는 방식
- 방법 1 : lm(data = 데이터세트, 종속변수~ 독립변수1 + 독립변수2 + ...)
- 방법 2 : lm(종족변수~ 독립변수1+ 독립변수2+ ... , data = 데이터세트)
- 방법 3 : lm(데이터세트$종속변수~ 데이터세트$독립변수1+ 데이터세트$독립변수2+ ...)


```r
lm(data= mtcars, mpg~ disp+ hp+ wt)
```

```
## 
## Call:
## lm(formula = mpg ~ disp + hp + wt, data = mtcars)
## 
## Coefficients:
## (Intercept)         disp           hp           wt  
##   37.105505    -0.000937    -0.031157    -3.800891
```

```r
# == lm(mpg~ disp+ hp+ wt, data= mtcars)
# ==lm(mtcars$mpg~ mtcars$disp+ mtcars$hp+ mtcars$wt)
```
-> mpg = 37.105505 - (0.000937 * disp) - (0.031157 * hp) - (3.800891 * wt) 라는 다중 회귀식이 도출됨.
-> 하지만, summary() 함수로 유의수준을 확인 할 필요가 있음


```r
RA = lm(data= mtcars, mpg~ disp+ hp+ wt)

summary(RA)
```

```
## 
## Call:
## lm(formula = mpg ~ disp + hp + wt, data = mtcars)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -3.891 -1.640 -0.172  1.061  5.861 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 37.105505   2.110815  17.579  < 2e-16 ***
## disp        -0.000937   0.010350  -0.091  0.92851    
## hp          -0.031157   0.011436  -2.724  0.01097 *  
## wt          -3.800891   1.066191  -3.565  0.00133 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.639 on 28 degrees of freedom
## Multiple R-squared:  0.8268,	Adjusted R-squared:  0.8083 
## F-statistic: 44.57 on 3 and 28 DF,  p-value: 8.65e-11
```
-> p-value: 8.65e-11로 0.001보다 작으므로 회귀모형이 적합
-> disp의 Pr(>|t|)값이 0.92851로 0.05보다 높으므로 *유의수준 부적합*
-> mpg는 disp에 영향을 받지 않고 hp, wt의 영향을 받음.
