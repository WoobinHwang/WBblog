---
title: "PL/SQL 제어문, 함수, 프로시져"
author: "woobin"
date: '2022-05-03'
---

# 0503_PL/SQL_Control,Function,Procedure

## 제어문

### IF문 (조건문)

- 형태

```sql
IF 조건1 THEN
	처리1;
ELSIF 조건2 THEN
	처리2;
ELSE
	처리3;
END IF;
```

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled.png)

- + 비슷한 조건문으로 CASE문도 있음

### LOOP문 (반복문)

```sql
LOOP
	처리문;
	EXIT WHEN 끝나는 조건;
END LOOP;
```

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%201.png)

- + 비슷한 반복문으로 WHILE문과 FOR문이 있음

- 2중 반복문으로 구구단 만들기
    - WHILE문 속에 FOR문 집어넣기 (성공)
    
    ![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%202.png)
    
    - FOR문 속에 WHILE문 집어넣기 (실패)
    
    ![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%203.png)
    
    - WHILE문 속에 WHILE문 집어넣기 (== 2중 while문) (실패)
    
    ![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%204.png)
    

 ——> 아무래도 WHILE문 내부에는 return이나 break의 특징이 들어가있는듯하다.

### GOTO문

- GOTO문을 만나면 GOTO문이 지정하는 라벨로 넘어가는 제어문
- 구구단으로 예시

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%205.png)

### NULL문

- IF문이나 CASE문에서 아무것도 수행하길 원하지 않을 때 NULL문을 사용하고는 함.

```sql
예시)
IF 조건1 THEN
	처리1
else NULL;
```

## 사용자 정의 함수

- 함수 생성

```sql
CREATE OR REPLACE FUNCTION 함수이름(매개변수, ...)
RETURN 데이터 타입
IS[AS]
	변수, 상수 선언
BEGIN
	실행부
	...
	RETURN 반환값;
[EXCePTION
	예외 처리부]
END [함수이름];
```

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%206.png)

- 함수 호출

```sql
SELECT 함수이름(매개변수, ...)
```

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%207.png)

### 프로시저

: 특정한 로직을 처리하기만 하고 결과 값을 반환하지는 않는 서브 프로그램. 테이블에서 데이터를 추출해 입맛에 맞게 조작하고 그 결과를 다른 테이블에 다시 저장하거나 갱신하는 일련의 처리를 할 때 주로 프로시저를 사용

```sql
CREATE OR REPLACE PROCEDURE 프로시져 이름
	(매개변수명1 [IN | OUT | IN OUT] 데이터 타입 [:= 디폴트 값]
	, ...)
IS[AS]
	변수, 상수 등 선언
BEGIN
	실행부
[EXcePTION
	예외 처리부]
END [프로시져 이름];
```

- 프로시저 생성

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%208.png)

- 프로시저 실행

```sql
EXEC 프로시저명(매개변수1 ,...);
```

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%209.png)

- 제대로 입력 되었는지 테이블 조회하기

![Untitled](/Images/0503_PL_SQL_Control,Function,Procedure/Untitled%2010.png)

- 매개변수 디폴트 값 설정
: 프로시저를 실행 할 때는 반드시 매개변수의 개수에 맞춰 값을 전달 해 실행해야 한다.
- OUT, IN OUT 매개변수
: OUT매개변수란 프로시저 실행 시점에 OUT매개변수를 변수 형태로 전달하고 프로시저 실행부에서 이 매개변수에 특정 값을 할당한다.
- RETURN문
: 프로시저에서 RETURN문을 만나면 이후 로직을 처리하지 않고 수행을 종료. 즉, 프로시저를 빠져나간다.