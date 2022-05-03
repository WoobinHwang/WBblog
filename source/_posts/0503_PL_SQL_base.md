---
title: "PL/SQL 구성요소"
author: "woobin"
date: '2022-05-03'
---

# 0503_PL/SQL_base

# PL/SQL 구성요소

- 실제 복잡한 비즈니스 로직을 만드는 것은 PL / SQL을 사용하여 작성

### 블록

```sql
이름부
IS(AS)
선언부
BEGIN
실행부
EXCEPTION
예외 처리부
END;
```

### 익명블록 만들기

- 실행이 안된다면 상단에 아래 문구를 입력 했는지 확인
SET SERVEROUTPUT ON

![Untitled](/Images/0503_PL_SQL_base/Untitled.png)

## 구성요소

### 변수

- 변수 선언 시 형식
    - 변수명 데이터타입 := 초기값;
- INTEGER의 하위 타입
1. NATURAL
: 음수 제외, 0 포함
2. NATURALN
: 음수 제외, NULL 할당 불가, 선언시 반드시 초기화 필요
3. POSITIVE
: 양수, 0 미포함
4. POSITIVEN
: 양수, NULL 할당 불가, 선언시 반드시 초기화 필요
5. SIGNTYPE
: -1, 0, 1
6. SIMPLE_INTEGER
: NULL이 아닌 모든 값, 선언시 반드시 초기화 필요

### 상수

- 변하지 않는 값을 상수로 선언함, 변수가 아니기 때문에 한 번 할당하면 변하지 않음. (예시 : 파이)

### 연산자

1. *, /, **
: 곱하기, 나누기, 제곱
2. ||
: 문자열 연산자
3. 기타등등...

### DML문

테이블과 연동하여 PL / SQL을 작성

![Untitled](/Images/0503_PL_SQL_base/Untitled%201.png)

- 출력코드

```sql
DBMS_OUTPUT.PUT_LIne("~~~");
```

- 테이블에서 데이터 타입 불러오는 법

```sql
vs_emp_name employees.emp_name%TYPE;
vs_dep_name departments.department_name%TYPE;
```

![Untitled](/Images/0503_PL_SQL_base/Untitled%202.png)

### PRAGMA 키워드

- 컴파일러가 실행되기 전에 처리하는 전처리기 역할
1. PRAGMA AUTONOMOUS_TRANSACTION
: 트랜잭션 처리를 담당하며, 현재 블록 내부에서 데이터베이스에 가해진 변경사항을 COMMIT이나 ROLLBACK하라는 지시를 하는 역할
2. PRAGMA EXCEPTION_INIT(예외 명, 예외 번호)
: 사용자 정의 예외 처리를 할 떄 사용하며, 특정 예외 번호를 명시해서 컴파일러에 이 예외를 사용한다는 것을 알리는 역할
3. PRAGMA RESTRICT_REFERECES(서브 프로그램명, 옵션)
: 오라클 패키지를 사용 할 때 선언 해 놓으면 패키지에 속한 프로그램에서 옵션 값에 따라  동작을 제한 할 때 사용.
4. PRAGMA SERIALLY_RESUABLE
: 패키지 메모리 관리를 쉽게 할 목적으로 사용되며, 패키지에 선언된 변수에 대해 한 번 호출된 후 메모리를 해제