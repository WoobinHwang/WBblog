---
title: "Chapter6,7 Test"
author: "woobin"
date: '2022-05-03'
---

# 0503_Chapter06,07_test

## 06장

### 1) 101번 사원을 조회하는 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled.png)

 ——> 마지막에 조인 해주는것이 중요

### 2) 쿼리 오류의 원인 찾기

![Untitled](/Images/0503_Chapter06,07_test/Untitled%201.png)

 ——> 외부조인시 (+) 명령어는 같은 테이블에서 써주는것이 좋다

- 올바른 예시

![Untitled](/Images/0503_Chapter06,07_test/Untitled%202.png)

 ——> (+)연산자를 위치를 바꿔도 가능

### 3) 외부 조인을 할 때 (+) 연산자를 같이 사용 할 수 없는데, IN절에 사용하는 값이 한 개이면 사용 할 수 있다. 그 이유는??

![Untitled](/Images/0503_Chapter06,07_test/Untitled%203.png)

### 4) ANSI 문법으로 변경

![Untitled](/Images/0503_Chapter06,07_test/Untitled%204.png)

### 5) 연관성 있는 서브쿼리에서 연관성 없는 서브 쿼리로 변경

![Untitled](/Images/0503_Chapter06,07_test/Untitled%205.png)

### 6) 연도별 이탈리아 최대 매출액과 사원을 작성하는 쿼리를 학습했다.
이 쿼리를 기준으로 최대 매출액 뿐만 아니라 최소매출액과 해당 사원을 조회하는 쿼리 작성

- 1차적으로 연도별 + 사번 기준으로 총 판매량을 구하는 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%206.png)

- 2차적으로 연도별 최소 매출액 구하는 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%207.png)

- 쿼리 합치기

![Untitled](/Images/0503_Chapter06,07_test/Untitled%208.png)

![Untitled](/Images/0503_Chapter06,07_test/Untitled%209.png)

## 7장

### 1) LISTAGG 함수 대신 계층형 쿼리, 분석 함수를 사용해서, 동일한 결과를 산출하는 쿼리를 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2010.png)

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2011.png)

### 2) 아래의 쿼리는 사원테이블에서 JOB_ID가 'SH_CLerk'인 사원을 조회하는 쿼리이다. 아래의 hire_date로 바꾸는 쿼리를 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2012.png)

- LEAD를 이용하기

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2013.png)

### 3) 2001년 12월 판매 데이터 중 현재일자를 기준으로 고객의 나이를 계산해서 연령대별 매출금액을 보여주는 쿼리 작성

- 첫번째 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2014.png)

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2015.png)

- 앞서 알아둔 코드들로 첫번째 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2016.png)

- 두번째 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2017.png)

- WITH절로 합쳐서 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2018.png)

### 4) 월별로 판매금액이 가장 하위에 속하는 대률 목록
대륙 목록은 countries테이블의 country_region에 있음
country_id컬럼으로 customers 테이블과 조인

- 첫번쨰 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2019.png)

- 두번째 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2020.png)

- WITH절로 쿼리 합치기

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2021.png)

### 5) 지역별, 대출종류별, 월별 대출잔액과 지역별 파티션을 만들어
대출종류별로 대출잔액의 퍼센트를 구하는 쿼리 작성

- 첫번쨰 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2022.png)

- 두번째 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2023.png)

- 쿼리 합쳐서 작성

![Untitled](/Images/0503_Chapter06,07_test/Untitled%2024.png)