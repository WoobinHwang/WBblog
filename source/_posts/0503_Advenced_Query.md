---
title: "고급 쿼리"
author: "woobin"
date: '2022-05-03'
---

# 0503_Advenced_Query

# 고급 쿼리

## 분석함수와 WINDOW함수

### 분석 함수

```sql
분석함수(매개변수) OVER
	(PARTITION BY expr1, expr2, ...
		ORDER BY expr3, expr4, ...
	window절)
```

1. row_number()
: 행 번호를 반환
2. RANK(), DENSE_RANK()
: RANK는 데이터의 순위를 매겨 반환
: DENSE_RANK는 순위를 건너뛰지 않으며 반환 (공동 2등 이후 3등이 출력)
3. CUME_DIST(), PERCENT_RANK()
: CUME_DIST는 누적 분포도 값 반환
: PERCENT_RANK는 현재 값보다 작은 데이터들의 누적 값을 반환
4. NTILE()
: 값을 expr에 명시된 값만큼 분할한 결과를 반환 (예시: 60 / 30 = 2)
5. LAG(column, n, base) LEAD()
: column의 컬럼에서 n만큼 선행 로우, 후행 로우의 값을 반환, 가져올 행이 없다면 base를 반환.

### window절
: 파티션으로 분할 된 그룹에 대해 별도로 다시 그룹을 만드는 역할

1. ROWS
: 로우 단위로 window절을 지정
2. RANGE
: 로우가 아닌 논리적인 범위로 window절을 지정
3. BETWEEN ~ AND
: window절의 시작과 끝 지점을 명시
4. UNBOUNDED PRECEDING
: 파티션으로 구분된 첫 번째 로우가 시작 지점
5. UNBOUNDED FOLLOWING
: 파티션으로 구분된 마지막 로우가 끝 지점
6. CURRENT ROW
: 시작 및 끝 지점이 현재 로우가 됨.
7. value_expr PRECEDING
: 끝 지점일 경우, 시작 지점은 value_expr PRECEDING
8. value_expr FOLLOWING
: 시작 지점일 경우, 끝 지점은 value_expr FOLLOWING

### window함수

- 모든 분석 함수를 window절과 함께 사용 할 수 있는 것은 아님.
분석 함수 중 AVG, CORR, COUNT, FRIST_VALUE, LAST_VALUE, MAX, MIN, NTH_VALUE, STDDEV, SUM, VARIANCE 등의 함수만 window절과 함께 사용 할 수 있다.

## 계층형 쿼리

- 2차원 형태태의 테이블에 저장된 데이터를 계층형 구조로 결과를 반환하는 쿼리

![Untitled](/Images/0503_Advenced_Query/Untitled.png)

### 계층형 쿼리 심화학습

1. 계층형 쿼리 정렬
: 계층형 구조에 맞추어 ORDER BY절로 순서를 바꿀 수 있다.
2. CONNECT_BY_ROOT
: 계층형 쿼리에서 최상위 로우를 반환하는 연산자
3. CONNECT_BY_ISLEAF
: CONNECT BY 조건에 정의된 관계에 따라 해당 로우가 최하위 자식 로우이면 1을 그렇지 않으면 0을 반환하는 의사컬럼
4. SYS_CONNECT_BY_PATH(column, char)
: 계층형 쿼리에서만 사용 할 수 있는 함수로, 루트 노드에서 시작해 자신의 행까지 연결된 경로 정보를 반환한다.
5. CONNECT_BY_ISCYCLE
: 현재 로우가 자식을 갖고 있는데 동시에 그 자식 로우가 부모 로우이면 1을, 그렇지 않으면 0을 반환.

## WITH절

- WITH절 형태

```sql
WITH
	별칭1 AS (쿼리)
	, ...
SELECT ...
FROM 별칭1, ...;
```

### 개선된 서브 쿼리

### 순환 서브 쿼리