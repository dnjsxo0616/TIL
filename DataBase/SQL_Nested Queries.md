# 1. Introduction to Subquery

## Subquery
- 'A query inside a query'
- 단일 쿼리문에 여러 테이블의 `데이터를 결합`하는 방법

## Subquery 예시
- table1에서 가장 나이가 어린 사람을 삭제해야 한다면?
```
SELECT MIN(age) FROM table1;
  +
DELETE FROM table1
WHERE age = 위에서 찾은 값;
```
```
DELETE FROM table1
WHERE age = (
  SELECT MIN(age) FROM table1 # 이 줄이 Subquery
);

>> Subquery 특징
- 조건에 따라 하나 이상의 테이블에서 데이터를 검색하는 데 사용
- SELECT, FROM, WHERE, HAVING 절 등에서 다양한 맥락에서 사용
- FROM에 Subquery를 사용하면 AS로 별칭 지정을 무조건 해줘야한다.(별도의 파생된 테이블로 간주해서)
```

## :question: JOIN과 Subquery의 차이는? (다른 테이블의 결과를 결합하는 느낌이 같지만)
- `JOIN`은 서로가 같은 값을 서로가 참조하는 FK(외래키)의 관점으로 봤다면
- `Subquery`는 외래키의 관점 보다는 완전히 다른 상황에서 쿼리문을 다시 실행해서 그 결과값을 얻기 위함이다.

## `EXISTS` operator
- 쿼리 문에서 반환된 레코드의 존재 여부를 확인

## EXISTS 구조
```
SELECT
  select_list
FROM
  table
WHERE
  [NOT] EXISTS (subquery);

>> subquery가 하나 이상의 행을 반환하면 EXISTS 연산자는 true를 반환하고 그렇지 않으면 false를 반환
>> 주로 WHERE 절에서 subquery의 반환 값 존재 여부를 확인하는데 사용
```

# 2. Conditional Statements

## `CASE` statement
- SQL문에서 `조건문`을 구성

## CASE 구조
```
CASE case_value
  WHEN when_value1 THEN statements
  WHEN when_value2 THEN statements
  ...
  [ELSE else-statements]
END CASE;
>> case_value가 when_value와 동일한 것을 찾을 때까지 순차적으로 비교
>> when_value와 동일한 case_value를 찾으면 해당 THEN 절의 코드를 실행
>> 동일한 값을 찾지 못하면 ELSE 절의 코드를 실행
  - ELSE 절이 없을 때 동일한 값을 찾지 못하면 오류 발생

SELECT contactFirstName, creditLimit,
  CASE
    WHEN creditLimit > 100000 THEN 'VIP'
    WHEN creditiLimit > 70000 THEN 'Platinum'
    ELSE 'Bronze'
  END AS grade
FROM customers;
```

# :bulb: 글(articles), 댓글(comment), 사용자(uers)

## N : 1 관계 (1:1 관계)
- articles : users
- comment : articles
- commnet : users
  - N이 외래키를 갖게된다.

### 1. 내가 작성한 모든 글을 조회
- 1명의 유저는 게시글을 여러 개 작성할 수 있다.(작성하지 않을 수도 있다.)
  - 글은 여러명이 달 수도 있으니 글에 userld라는 외래키를 articles에 달아준다.
  - users에 외래키가 있으면 작성자 이름이 중복되면 찾을 수가 없다.

### 2. 1개의 게시글에는 여러 댓글이 작성될 수 있다.(작성되지 않을 수도 있다.)
  - comment에 외래키를 주어 각 댓글에 작성한 게시글에 맞는 외래키를 작성한다.
  - articles에 fk를 주면 댓글이 여러 개 달리는 것을 작성할 수 없다.

### 3. 한 명의 유저는 댓글을 여러개 작성할 수 있다. (작성하지 않을 수도 있다.)
  - users를 참조하는 외래키를 comment에 작성한다.

## 그러면 comment의 달린 글과 사용자에 대한 외래키로 모든 정보를 알 수가 있다.
- 값을 찾으려면 comment와 uers or articles를 JOIN해서 정보를 추출할 수 있다.