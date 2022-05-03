---
title: "Chapter6,7 Test02"
author: "woobin"
date: '2022-05-03'
---

# 0503_Chapter06,07_test_02

### 데이터 셋 임포트하기

- 사이드바에서 테이블(우클릭) - 임포트

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled.png)

- 찾아보기에서 원하는 데이터 파일을 선택

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%201.png)

- 인코딩은 UTF-8로 설정 > 다음 버튼 누르기

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%202.png)

- 테이블 이름 작성 > 다음 버튼 계속 누르면 끝!

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%203.png)

- 테이블 생성 된 것을 확인 가능

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%204.png)

### 데이터가 제대로 확인 되는지 확인

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%205.png)

## TEST (Sub Query)

### 1) 2015년 평균 기대수명보다 높은 모든 정보를 조회

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%206.png)

### 2) sub_countries 테이블에 있는 capital과 매칭되는 cities 테이블의 정보를 조회

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%207.png)

### 3) 조건 1. economies 테이블에서 country code, inflation rate, unemployment rate를 조회
조건 2. inflation rate 오름차순으로 정렬
조건 3. sub_countries 테이블내 gov_form 컬럼에서 ‘Constitutional’, ‘Monarchy’, ‘Republic’ 들어간 국가는 제외

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%208.png)

### 4) 2010년 각 대륙별 inflation_rate가 가장 심한 국가와 inflation_rate를 조회

- 우선 대륙별 최대값을 조회하는 쿼리 작성

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%209.png)

- base query와 합쳐서 작성

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%2010.png)

## TEST (Window Function)

### 1) 각 행에 숫자를 1, 2, 3, ..., 형태로 추가한다. (row_n 으로 표시), row_n 기준으로 오름차순으로 출력

- Alias 적용

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%2011.png)

### 2) 올림픽 년도를 오름차순 순번대로 작성

- 두가지 쿼리가 같은 결과를 보여줌.

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%2012.png)

### 3)  (1) WITH 절 사용하여 각 운동선수들이 획득한 메달 갯수를 내림차순으로 정렬
(2) (1) 쿼리를 활용하여 그리고 선수들의 랭킹을 추가, 상위 5개만 추출 : OFFSET 0 ROWS FETCH NEXT 5 ROWS ONLY

- 내림차순 : desc로 구현
- 두가지 쿼리가 같은 결과를 보여줌.

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%2013.png)

### 4) 남자 69KG 역도 경기에서 매년 금메달리스트 조회, 매년 전년도 챔피언도 같이 조회

- 두가지 쿼리가 같은 결과를 보여줌.

![Untitled](/Images/0503_Chapter06,07_test_02/Untitled%2014.png)