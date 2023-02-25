# 1. Introduction to Join

- 관계형 데이터 베이스
  - 데이터 간의 `관계`가 있는 데이터 항목들의 모음
  - 관계 : `여러` 테이블 간의 (논리적) 연결

## :question: 왜?? 일반적으로 데이터베이스에서는 하나의 테이블이 아닌 여러 테이블로 나누어 저장하고 결합하여 출력하나?
- 동명이인이 있다면..혹은 특정 데이터가 수정된다면(ex. student -> 학생) `데이터 관리`가 너무 어렵다.
- 테이블을 분리하면 관리는 용이해질 수 있으나 출력할 때는 테이블 한 개만을 출력할 수 밖에 없어 `다른 테이블과 연결지어 출력`하는 것이 필요하다. => `JOIN`

# 2. Joining tables

## `JOIN` clause
- 둘 이상의 테이블에서 데이터를 검색하는 방법

## `INNER JOIN` clause
- `두 테이블`에서 `값이 일치`하는 레코드에 대해서만 결과를 반환

### INNER JOIN 구조
```
SELECT
  select_list
FROM
  table1
INNER JOIN table2
  ON table1.fk = table2.pk;

>> FROM 절 이후 메인 테이블 지정  : 조회의 기준이 되는 테이블 (table1)
>> INNER JOIN 절 이후 메인 테이블과 조인할 테이블을 지정 (table2)
>> ON 키워드 이후 조인 조건을 작성 : 조인 조건은 table1과 table2 간의 레코드를 일치시키는 규칙을 지정

SELECT
  *
FROM
  articles
INNER JOIN users
  ON articles.userId = users.id;
```

## `LEFT JOIN` clause
- 오른쪽 테이블의 `일치`하는 레코드와 함께 왼쪽의 `모든` 레코드 반환

### LEFT JOIN 구조
```
SELECT
  select_list
FROM
  table1
LEFT [OUTER] JOIN table2
  ON table1.fk = table2.pk;

>> FROM 절 이후 왼쪽 테이블 지정 (table1)
>> LEFT JOIN 절 이후 오른쪽 테이블 지정 (table2)
>> ON 키워드 이후 조인 조건을 작성
>> 왼쪽 테이블의 각 레코드를 오른쪽 테이블의 모든 레코드와 일치시킨다.

SELECT
  *
FROM
  articles
LEFT JOIN users
  ON articles.userId = users.id;
```

## `RIGHT JOIN` clause
- 왼쪽 테이블의 `일치`하는 레코드와 함께 오른쪽 테이블의 `모든` 레코드 반환

### RIGHT JOIN 구조
```
SELECT
  select_list
FROM
  table1
RIGHT [OUTER] JOIN table2
  ON table1.fk = table2.pk;

>> FROM 절 이후 왼쪽 테이블 지정 (table1)
>> RIGHT JOIN 절 이후 오른쪽 테이블 지정 (table2)
>> ON 키워드 이후 조인 조건을 작성
>> 오른쪽 테이블의 각 레코드를 왼쪽 테이블의 모든 레코드와 일치시킨다.


SELECT
  *
FROM
  articles
RIGHT JOIN users
  ON articles.userId = users.id;
```

# 두 개의 테이블 모두 JOIN할 때
```
SELECT * FROM tableA
LEFT JOIN tableB ON tableA.fk = tableB.id
UNION
SELECT * FROM tableA
RIGHT JOIN tableB ON tableA.fk = tableB.id; 
```

# 3개 이상의 테이블을 JOIN할 때
```
>> 방법 1
SELECT   P.PLAYER_NAME 선수명, T.TEAM_ID 팀명, S.STATIUM_NAME 구장명
FROM     PLAYER P INNER JOIN TEAM T
ON       P.TEAM_ID = T.TEAM_ID
         INNER JOIN STADIUM S
ON       T.STADIUM_ID = S.STADIUM_ID
ORDER BY 선수명 ; 

>> 방법 2
SELECT   P.PLAYER_NAME 선수명, T.TEAM_ID 팀명, S.STATIUM_NAME 구장명
FROM     PLAYER P, TEAM T, STADIUM S
WHERE    P.TEAM_ID = T.TEAM_ID
AND      T.STADIUM_ID = S.STADIUM_ID
ORDER BY 선수명 ; 
>>