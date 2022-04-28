---
title: "GROUP 쿼리, 집합 연산자"
author: "woobin"
date: '2022-04-28'
---

# 0428_Group_Query_and_SET_Operator

## 기본 집계 함수

1. COUNT(expr)
: 행 개수를 반환 (NULL값은 계산 안됨.)

1-1. DISTINCT
      : 해당 컬럼을 유일한 값으로 조회

1. SUM(exopr)
: 전체 합계를 반환
2. AVG(expr)
: 평균값을 반환
3. MIN(expr), MAX(expr)
: 최소값과 최대값을 반환
4. VARIANCE(expr), STDDEV(expr)
: 분산과 표준편차를 반환

### GROUP BY절, HAVING절

1. GROUP BY
: 해당 컬럼의 값이 같은 것 끼리 묶어서 반환
—> 참고) SELECT문에서 선언 된 컬럼명이나 표현식 중 집계함수를 제외하고는 모두 GROUP BY절에서 선언해야 한다!!

![Untitled](/Images/0428_Group_Query_and_SET_Operator/Untitled.png)

1. HAVING 절
: GROUP BY절 뒤에 위치하며, GROUP BY 한 결과에 조건을 제시하여 해당하는 데이터를 반환

![Untitled](/Images/0428_Group_Query_and_SET_Operator/Untitled%201.png)

### ROLLUP절, CUBE절
: 소그룹간의 합계를 계산

1. ROLLUP절
: 합계 결과를 아래에 붙여 출력
2. CUBE절
: 합계 결과를 위에 붙여 출력

### 집합(SET) 연산자

1. UNION, INTERSECT
: 합집합과 교집합의 역할
2. MINUS
: 차집합을 의미 (앞의 데이터에서 뒤의 데이터를 뺀 나머지)

- 집합 연산자 사용시 유의 사항
1. SELECT리스트의 개수와 데이터 타입이 일치해야한다.
2. 집합 연산자로 SELECT문을 연결할 때 ORDER BY절은 맨 마지막에서만 사용 가능
3. BLOB, CLOB, BfILE타입의 컬럼에 대해서는 집합 연산자를 사용 할 수 없다.
4. UNION, INTERSECT, MINUS 연산자는 LONG형 컬럼에는 사용 할 수 없다.