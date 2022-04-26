---
title: "SQL Query문"
author: "woobin"
date: '2022-04-26'
---

# 0426_SQL_Query

## SELECT문
: 테이블이나 뷰에 있는 데이터를 조회 할 때 사용하는 명령문

- SELECT * 혹은 컬럼
FROM * 테이블 또는 VIEW
WHERE 조건
ORDER BY 컬럼

- 어디서 가져올 것인지 -> FROM
무엇을 가져올 것인지 -> SELECT
어떻게 가져올 것인지 -> WHERE

- 자주 쓰이며 설명할게 없어서 예시로 대체
예시)

![Untitled](/Images/0426_SQL_Query/Untitled.png)

## INSERT문
: 데이터를 입력하여 삽입 할 때 사용하는 명령문

- 자주 쓰이며 설명할게 없어서 예시로 대체
예시)

![Untitled](/Images/0426_SQL_Query/Untitled%201.png)

- VALUES 대신에 SELECT구문을 이용하여 입력 해 넣는것도 가능

![Untitled](/Images/0426_SQL_Query/Untitled%202.png)

## UPDATE문
: 기존 데이터를 수정 할 때 사용하는 명령문

- 아래의 형태를 띄며 대체 할 데이터를 SET 라는 문구로 가져가야 한다는 게 포인트

```sql
UPDATE 테이블명
SET 컬럼1 = 변경 할 값 1,
		컬럼2 = 변경 할 값 2, ...
WHERE 조건;
```

- 예시)

![Untitled](/Images/0426_SQL_Query/Untitled%203.png)

## MERGE문 (병합)
: 조건을 비교하여 테이블에 해당 조건에 맞는 데이터가 없으면 insert, 있으면 update하는 명령문

- 아래의 형태를 띄며 굉장히 복잡하다;

```sql
MERGE INTO 테이블명
	USING (update)나 insert 될 데이터의 원천)
		ON (update 될 조건)
WHEN MATCHED THEN
	SET 컬럼1 = 값1, ...
WHERE update 조건
	DELETE WHERE update_delete 조건
WHEN NOT MATCHED THEN
	INSERT (컬럼1, ...) VALUES (값1, ...)
	WHERE insert 조건;
```

- 예시)

![Untitled](/Images/0426_SQL_Query/Untitled%204.png)

## DELETE문
: 데이터를 삭제 할 때 사용하는 명령문

- 설명할게 없어서 예시로 대체
예시)

![Untitled](/Images/0426_SQL_Query/Untitled%205.png)

## COMMIT, ROLLBACK, TRUNCATE

- COMMIT : 변경한 데이터를 DB에 마지막으로 반영
ROLLBACK : 반대로 변경한 데이터를 변경하기 이전 상태로 되돌림
TRUNCATE: 한번 실행 시 데이터 바로 삭제, rollback으로 적용 안됨

- ex3_5 테이블 생성

![Untitled](/Images/0426_SQL_Query/Untitled%206.png)

- 명령프롬프트에서 DB로 조회

![Untitled](/Images/0426_SQL_Query/Untitled%207.png)

- commit 하기

![Untitled](/Images/0426_SQL_Query/Untitled%208.png)

- 다시 명령프롬프트에서 DB로 조회

![Untitled](/Images/0426_SQL_Query/Untitled%209.png)

## 의사컬럼
: 테이블의 컬럼처럼 동작하지만 실제로 테이블에 저장되지 않는 컬럼

- 컬럼중 ROWNUM과 ROWID를 이용해봄.
- ROWNUM로 조회가 가능하다, 행의 번호 정도로 인식 됨.
예시)

![Untitled](/Images/0426_SQL_Query/Untitled%2010.png)

- 테이블 생성시 각자 다른 ROWID가 생성되어 그걸로 조회도 가능하다!
예시)

![Untitled](/Images/0426_SQL_Query/Untitled%2011.png)