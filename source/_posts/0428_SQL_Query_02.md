---
title: "SQL Query문 2"
author: "woobin"
date: '2022-04-28'
---

# 0428_SQL_Query_02

## 연산자

### 수식 연산자
: +, - , *, /

### 문자 연산자
: ‘ || ’

예시) 두 개의 컬럼을 사이에 ‘-’를 붙여 하나의 컬럼으로 출력

![Untitled](/Images/0428_SQL_Query_02/Untitled.png)

### 논리 연산자
: <, <=, !=, 등등이 있음.

---

## 표현식
: CASE WHEN 조건 THEN ... ELSE ... END

![Untitled](/Images/0428_SQL_Query_02/Untitled%201.png)

## 조건식

### 비교 조건식
ANY, SOME : OR연산자와 같은 개념
ALL : AND연산자와 같은 개념

- 대표적으로 ANY 를 사용한 예시)

![Untitled](/Images/0428_SQL_Query_02/Untitled%202.png)

### 논리 조건식
: AND, OR, NOT을 이용하여 조건을 TRUE / FALSE로 반환

![Untitled](/Images/0428_SQL_Query_02/Untitled%203.png)

### NULL 조건식
: IS NULL, IS NOT NULL을 이용하여 특정 값이 NULL인지 여부를 체크

```sql
-- 해당 코드
SELECT *
FROM test2
WHERE col2 IS NOT NULL;
```

![Untitled](/Images/0428_SQL_Query_02/Untitled%204.png)

### BETWEEN AND 조건식
: 크거나 같고 작거나 같은 값을 찾음.

![Untitled](/Images/0428_SQL_Query_02/Untitled%205.png)

### IN, NOT IN 조건식
: ANY와 비슷하지만 ANY는 등호나 논리 연산자와도 함께 사용 가능

![Untitled](/Images/0428_SQL_Query_02/Untitled%206.png)

### EXISTS 조건식
: IN과 비슷하지만 서브쿼리 절에서만 사용 가능

- 서브쿼리 : SQL문 속에 포함되어 있는 또 다른 SQL문

![Untitled](/Images/0428_SQL_Query_02/Untitled%207.png)

### LIKE 조건식
: 문자열의 패턴을 검색하여 데이터를 찾음.

- 밑줄 ( _ )을 이용하여 문자열의 위치 지정도 가능

![Untitled](/Images/0428_SQL_Query_02/Untitled%208.png)