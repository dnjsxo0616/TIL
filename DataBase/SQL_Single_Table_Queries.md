# 1. Querying data

## `SELECT` statement
- 테이블에서 데이터 `조회`

### SELECT syntax
```
SELECT
  select_list
FROM
  table_name;
```
- SELECT 키워드 다음에 데이터를 `선택하려는 필드`를 하나 이상 지정
- FROM 키워드 다음에 데이터를 `선택하려는 테이블`의 이름을 지정

### SELECT 정리

- SELECT 문을 사용하여 테이블의 데이터를 조회 및 반환
- SELECT `*` (asterisk)를 사용하여 테이블의 모든 필드 선택

# 2. Sorting data

## `ORDER BY` clause
- 조회 결과의 레코드를 정렬

### ORDER BY syntax
```
SELECT
  select_list
FROM
  table_name
ORDER BY
  column1 [ASC/DESC],
  column2 [ASC/DESC],
  ....;
```
- FROM clause 뒤에 위치
- 하나 이상의 컬럼을 기준으로 결과를 오름차순, 내림차순으로 `정렬`
  - ASC : 오름차순 `(기본 값)`
  - DESC : 내림차순

### SELECT statement 실행순서
- FROM -> SELECT -> ORDER BY
1. 테이블에서 (FROM)
2. 조회하여 (SELECT)
3. 정렬 (ORDER BY)

# 3. Filtering data

## `DISTINCT` clause
-  조회 결과에서 중복된 레코드를 제거

### DISTINCT syntax
```
SELECT DISTINCT
  select_list
FROM
  table_name;
```
- SELECT 키워드 바로 뒤에 작성해야 한다.
- SELECT DISTINCT 키워드 다음에 고유한 값을 선택하려는 하나 이상의 필드를 지정

## `WHERE` clause
- 조회 시 `특정 검색 조건`을 지정

### WHERE 구조(syntax)
```
SELECT
  select_list
FROM
  table_name
WHERE
  search_condition;
```
- FROM clause 뒤에 위치
- search_conditon은 `비교연산자 및 논리연산자(AND, OR, NOT 등)`를 사용하는 구문이 사용됨

## Comparison Operators
- 비교 연산자
  - =, >=, IS, LIKE, IN, BETWEEN...AND

## Logical Operators
- 논리 연산자
  -  AND(&&), OR(||), NOT(!)

### `IN` 연산자(operators)
- 값이 특정 목록 안에 있는지 확인

### `LIKE` 연산자(operator)
- 값이 특정 패턴에 일치하는 지 확인 with Wildcards
  - `'%'` : `0개 이상의 문자열`과 일치하는지 확인
  - `'_'` : `단일 문자`와 일치하는지 확인

## `LIMIT` clause
- 조회나느 레코드 수를 제한

### LIMIT syntax
```
SELECT
  select_list
FROM
  table_name
LIMIT [offset,] row_count;
```
```
SELECT
  select_list
FROM
  table_name
LIMIT 3, 5; # == LIMIT 4 OFFSET 3;
"""
4부터 5번째 후 까지 # 4, 5, 6, 7, 8
"""
```

# 4. Grouping data

## `GROUP BY` clause
- 레코드를 그룹화하여 요약본 생성 with `집계 함수`

### Aggregation Functions
- 값에 대한 계산을 수행하고 단일한 값을 반환하는 함수
  - SUM, AVG, MAX, MIN, COUNT

## GROUP BY syntax
```
SELECT
  c1, c2,...., cn, aggregate_function(ci)
FROM
  table_name
GROUP BY
  c1, c2,..., cn ;
```
- FROM 및 WHERE 절 뒤에 배치
- GROUP BY 절 뒤에 그룹화할 필드 목록을 작성

## GROUP BY 예시
```
>>> jobTitle 필드를 그룹화(여러개의 jobTitle 정보를 한 개로 합침)
SELECT
  jobTitle
FROM
  employees
GROUP BY
  jobTitle;
```
```
>>> COUNT 함수가 각 그룹에 대한 집계된 값을 계산
SELECT
  jobTitle, COUNT(*)
FROM
  employees
GROUP BY
  jobTitle;
```
## GROUP BY의 조건
- 특정 컬럼을 `그룹화` 하는 `GROUP BY`
- 특정 컬럼을 그룹화한 결과에 조건을 거는 `HAVING`

:bulb: WHERE랑 HAVING을 혼동하는 경우가 많은 데 `WHERE`은 `그룹화 하기 전`이고, `HAVING`은 `그룹화 한 후`의 조건이다.
```
SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼 HAVING 조건식;
```
### ORDER BY가 존재하는 경우
```
SELECT 컬럼 FROM 테이블 [WHERE 조건식]
GROUP BY 그룹화할 컬럼 [HAVING 조건식] ORDER BY 컬럼1 [, 컬럼2, 컬럼3 ...];
```
# SELECT statement 실행 순서
- FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY -> LIMIT
1. 테이블에서 (FROM)
2. 특정 조건에 맞춰 (WHERE
3. 그룹화하고 (GROUP BY)
4. 만약 그룹 중에서 조건이 있다면 맞추고 (HAVING)
5. 조회하여 (SELECT)
6. 정렬하고 (ORDER BY)
7. 특정 위치의 값을 가져온다 (LIMIT)