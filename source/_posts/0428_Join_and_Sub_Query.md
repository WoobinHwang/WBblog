---
title: "조인과 서브쿼리"
author: "woobin"
date: '2022-04-28'
---

# 0428_Join_and_Sub_Query

### ERD(Entity Relationship Diagram)
: 테이블끼리의 관계를 표현한 도표

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled.png)

## 내부 조인, 외부 조인

- 동등 조인
: WHERE 절에서 등호를 사용하여 2개 이상의 테이블이나 뷰를 연결하는 것

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%201.png)

- 세미 조인
: 서브쿼리를 이용하여 서브쿼리에 존재하는 데이터를 메인 쿼리에서 추출하는 것 (IN과 EXISTS가 있음)
    - IN 사용 예시)
    
    ![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%202.png)
    
    - EXISTS 사용 예시)
    
    ![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%203.png)
    
- 안티 조인
: 세미조인 개념의 반대. NOT IN이나 NOT EXISTS를 사용

- 셀프 조인
: 동일한 한 테이블을 사용하여 조인하는 방법
    - 예시)
    
    ![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%204.png)
    

- 외부 조인
: (+)를 사용하며 사용하며, 조건에 만족하지 않더라도 데이터를 모두 추출.
: (+)를 붙여주면 해당 테이블을 기준으로 조건에 맞는게 없다면 NULL값으로 반환.

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%205.png)

- 카타시안 조인
: WHERE절에 조인 조건이 없는 조인을 말하며, 결과 건수는 테이블들의 곱으로 나온다.

### ANSI조인

- ANSI 조인
: ANSI SQL문법을 사용한 조인이며,  WHERE절이 아닌 FROM절에 들어간다.
- 내부 조인의 예시)

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%206.png)

- 외부조인의 예시)

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%207.png)

- CROSS 조인
: 조인 조건을 명시하지 않는 CROSS조인
(테이블들의 곱만큼의 결과 건수가 나옴.)

- FULL OUTER 조인
: 외부 조인의 하나로 ANSI조인에서만 제공하는 기능
    - 다음과 같은 결과를 조회 할 수 있음.

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%208.png)

## 서브쿼리

- SQL문장 안에서 보조로 사용되는 또 다른 SELECT문

### 메인 쿼리와 연관성 없는 서브쿼리
: 메인 테이블과 조인 조건이 걸리지 않는 서브쿼리

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%209.png)

### 메인 쿼리와 연관성이 있는 서브쿼리
: 메인 테이블과 조인 조건이 걸린 서브 쿼리

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%2010.png)

### 인라인 뷰
: FROM 절에 사용하는 서브쿼리

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%2011.png)

- 퀴즈
- 101번 사원의 정보를 조회하기(정답 참고)
    - (각 테이블 연결하여 마무리)

![Untitled](/Images/0428_Join_and_Sub_Query/Untitled%2012.png)