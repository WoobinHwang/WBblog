---
title: "SQL 함수"
author: "woobin"
date: '2022-04-28'
---

# 0428_SQL_Function

### 숫자 함수

1. ABS(n)
: 절대값을 반환
2. CEIL(n), FLOOR(n)
: FLOOR는 가우스의 개념으로 n과 같거나 n보다 작은 정수중에 가장 큰 정수를 뜻함. ( CEIL은 반대의 의미)
3. ROUND(n, i), TRUNC(n1, i)
: ROUND는 n의 소수점 (i + 1)번째의 자리에서 반올림.
: TRUNC는 n의 소수점 i번째의 자리에서 뒷부분을 다 0으로 변환
4. POWER(n1, n2), SQRT(n)
:  제곱과 제곱근을 반환
5. MOD(n1, n2), REMAINDER(n1, n2)
: MOD는 n1에서 n2를 나눈 나머지 값
: REMAINDER는 그 나머지에서 n2를 뺀 음수의 값을 반환
6. EXP(n), LN(n), LOG(n2, n1)
: EXP는 지수 함수 e(2.718...)의 n제곱의 값
: LN은 밑수를 e로 하여 n의 로그 값
: LOG는 밑수를 n2로 하여 n1의 로그 값을 반환

### 문자 함수

1. INITCAP(char), LOWER(char), UPPER(char)
: INITCAP은 각 영단어의 첫번째를 대문자로 나머지는 소문자로 변환
: LOWER, UPPER는 문자열을 소문자, 대문자로 변환
2. CONCAT(char1, char2), **SUBSTR**(char, n, len)
: CONCAT은 문자열을 붙여 반환
: **SUBSTR**은 문자열의 n번째 문자부터 len개의 문자를 반환
3. LTRIM(char, set), RTRIM(char, set)
: 문자열의 왼쪽 / 오른쪽에서 set를 제거하고 반환
4. LPAD(name, n, set), RPAD(name, n, set)
: name이라는 컬럼에서 문자열의 길이가 n이 될 때 까지 set를 왼쪽 / 오른쪽에 입력
5. REPLACE(char, search, change), TRANSLATE(char, search, change)
: char 문자열에서 search 문자열을 찾아 change 문자열로 바꿈. TRANSLATE는 문자 하나씩 찾아 바꾸므로 권하지는 않음
6. INSTR(char, search, start, n), LENGTH(char)
: INSTR은 char 문자열에서 start번째의 문자에서 출발하여 n번째 search 문자열이 몇 번째 자리에 위치 해 있는지 반환
: LENGTH는 char문자열의 길이(문자의 개수)를 반환

### 날짜 함수

1. SYSDATE, SYSTIMESTAMP
: 현재 날짜와 시간을 반환
2. ADD_MONTHS(date, n)
: date날짜에 n달을 더한 후 반환
3. MONTHS_BETWEEN(date1, date2)
: date1의 달에서 date2의 달을 뺀 개월 수를 반환
4. LAST_DAY(date)
: 그 달의 마지막 일자를 반환
5. ROUND(date, format), TRUNC(date, format)
: ROUND는 format에 따라 반올림 한 날짜
TRUNC는 format을 기준으로 뒷자리를 잘라낸 날짜를 반환
6. NEXT_DAY(date, char)
: date날짜부터 char 날짜가 되는 날짜를 반환

### 변환 함수

- 형변환
1. TO_CHAR (data, format)
: data를 format의 형태에 맞게 반환
2. TO_NUMBER (data)
: data를 NUMBER 형태로 반환
3. TO_DATE(data, format), TO_TIMESTAMP(data, format)
: data를 format형태로 반환

### NULL관련 함수

1. NVL(expr1, expr2) WHERE expr1 IS NULL
: expr1의 컬럼이 null일때 expr2의 컬럼을 반환
2. NVL2(expr1, expr2, expr3)
: expr1이 null이 아니면 expr2 실행, expr1이 null이면 expr3을 실행
3. COALESCE(expr1, expr2, ...)
: expr중에 처음으로 null이 안되는 조건의 값으로 반환
4. LNNVL(조건식)
: 조건식이 NULL이나 부정문이 나올 때, TRUE를 반환, 조건식이 참 값이 나오면 FALSE를 반환한다.
5. NULLIF(expr1, expr2)
: expr1과 expr2를 비교해 같으면 NULL을 같지 않으면 expr1을 반환

### 기타 함수

1. GREATEST(data1, data2, ...), LEAST(data1, data2, ...)
: 데이터 값 중에서 가장 큰 값과 가장 작은 값을 반환
2. DECODE(expr, search1, result1, search2, result2, ... , default)
: expr과 search1을 비교해 두 값이 같으면 result1을 반환 같지 않으면 search2와 비교해 값이 같으면 result2를 반환을 반복하여 같은 값이 없으면 default로 값을 반환