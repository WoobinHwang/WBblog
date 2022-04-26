---
title: "데이터베이스의 객체 2"
author: "woobin"
date: '2022-04-26'
---

# 0426_Database_Object_02

### 제약 조건

1. NOT NULL : 해당 칼럼에는 NULL값을 입력 할 수 없음.
    
    ![Untitled](/Images/0426_Database_Object_02/Untitled.png)
    
    - NOT NULL 조건을 걸었더니 에러가 뜨는것이 확인됨.
    
- USER_CONSTRAINTS 명령어로 계약조건을 확인 할 수 있다.
type이 c로 나오는 모습

![Untitled](/Images/0426_Database_Object_02/Untitled%201.png)

1. UNIQUE : 중복되는 값을 입력 할 수 없음

![Untitled](/Images/0426_Database_Object_02/Untitled%202.png)

- 하지만 NUll값에 의한건 해당안됨.

![Untitled](/Images/0426_Database_Object_02/Untitled%203.png)

1. 기본키 : 기본키의 예시중 하나로 PRIMARY는 NOTNULL과 UNIQUE의 특성을 반영한 KEY이다.
    - constraint를 찍어본 결과 type이 p로 찍히는 모습.

![Untitled](/Images/0426_Database_Object_02/Untitled%204.png)

![Untitled](/Images/0426_Database_Object_02/Untitled%205.png)

1. 외래키 : 테이블간의 참조 데이터 무결성을 위한 제약조건
ex) table1의 컬럼1과 table2의 컬럼1을 연결하는식의 방식

1. CHECK : 컬럼에 입력되는 데이터를 체크해 특정 조건에 맞는 데이터만 입력 받고 그렇지 않으면 오류를 출력

![Untitled](/Images/0426_Database_Object_02/Untitled%206.png)

—— 조건에 맞지 않기에 오류가 발생하고 조건에 맞춰 데이터를 입력하는 모습

1. DEFAULT : 따로 입력하지 않아도 기본적으로 출력되게 하는 값들
값을 넣으려면 형식에 맞춰 입력시켜줘야함.
ex) 날짜 형식 ‘22/04/23’ , 숫자 형식 ‘1’ or 1

![Untitled](/Images/0426_Database_Object_02/Untitled%207.png)

---

### 테이블 변경

1. 컬럼명 변경

![Untitled](/Images/0426_Database_Object_02/Untitled%208.png)

1. 컬럼 타입 변경

![Untitled](/Images/0426_Database_Object_02/Untitled%209.png)

1. 컬럼 추가

![Untitled](/Images/0426_Database_Object_02/Untitled%2010.png)

1. 컬럼 삭제

![Untitled](/Images/0426_Database_Object_02/Untitled%2011.png)

1. 계약조건 추가

![Untitled](/Images/0426_Database_Object_02/Untitled%2012.png)

1. 계약조건 삭제

![Untitled](/Images/0426_Database_Object_02/Untitled%2013.png)

1. 테이블 복사

![Untitled](/Images/0426_Database_Object_02/Untitled%2014.png)

### 뷰(view) : 하나 이상의 테이블이나 다른 뷰의 데이터를 볼 수 있게 하는 데이터베이스 객체
=⇒ 테이블과 비슷

1. 뷰 생성

![Untitled](/Images/0426_Database_Object_02/Untitled%2015.png)

1. 뷰 삭제

![Untitled](/Images/0426_Database_Object_02/Untitled%2016.png)

### 인덱스 : 테이블에 있는 데이터를 빨리 찾기 위한 용도의 데이터베이스객체

- 인덱스를 분류하면 다음과 같이 분류 할 수 있다.
1. 인덱스 구성 컬럼 개수에 따른 분류 : 단일 인덱스와 결합 인덱스
2. 유일성 여부에 따른 분류: unique 인덱스, non-unique 인덱스
3. **인덱스 내부 구조에 따른 분류**: **B-tree 인덱스, 비트맵 인덱스, 함수 기반 인덱스**

- 예시) unique 인덱스를 만들고,
ex2_10에 적용한 모습

![Untitled](/Images/0426_Database_Object_02/Untitled%2017.png)

 —— 앞서 제약조건에서 언급한 unique와 같은 성질을 띔

- 제약조건을 걸어두고 특징을 본 모습

![Untitled](/Images/0426_Database_Object_02/Untitled%2018.png)

### 마치며

- 컬럼명이나 테이블명은 대부분 대문자로 들어가기때문에 가급적 대문자로 적어두는게 문제가 덜 발생할것같다.