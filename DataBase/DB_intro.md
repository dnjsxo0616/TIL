# 데이터 베이스
- 체계적인 데이터 모음

## 데이터 베이스 역할
- 데이터를 저장하고 조작
  1. Create (저장)
  2. Read (조회)
  3. Update (갱신)
  4. Delete (삭제)

# Relational Database(관계형 데이터베이스)
- 데이터 간에 `관계`가 있는 데이터 항목들의 모음

## 관계형 데이터 베이스
- 테이블, 행, 열의 정보를 구조화하는 방식
- `서로 관련된 데이터 포인터를 저장`하고 이에 대한 `액세스`를 제공

## 관계
- 여러 테이블 간의 (논리적) 연결

### 관계로 인해 할 수 있는 것
- 이 관계로 인해 두 테이블을 사용하여 데이터를 다양한 형식으로 조회할 수 있다.
  - 특정 날짜에 구매한 모든 고객 조회
  - 지난 달에 배송일이 지연된 고객 조회

## 관계형 데이터베이스 용어

1. Table (aka Relation)
  - 데이터를 기록하는 최종 위치

2. Field (aka Column 열, Attribute)
  - 각 필드에서 고유한 데이터 형식(타입)이 지정됨

3. Record (aka Row 행, Tuple)
  - 각 레코드에는 구체적인 데이터 값이 저장됨

4. Database (aka Schema)
  - 테이블의 집합 (Set of tables)

5. Primary Key (기본 키)
  - 각 레코드의 고유한 값
  - 관계형 데이터베이스에서 `레코드의 식별자`로 활용

6. Foreign Key (외래 키)
  - 테이블의 필드 중 다른 테이블의 레코드를 식별할 수 있는 키
  - 각 레코드에서 서로 다른 테이블 간의 `관계를 만드는 데` 사용

## RDBMS (Relational Database Management System)

- `관계형` 데이터베이스를 관리하는 소프트웨어 프로그램 ex) MySQL
- DBMS : 데이터베이스를 관리하는 소프트웨어 프로그램
- 데이터 저장 및 관리를 용이하게 하는 시스템
- 데이터베이스와 사용자 간의 인터페이스 역학
  - 사용자가 데이터 구성, 업데이트, 모니터링, 백업, 복구 등을 할 수 있도록 도움

## 정리
- 모든 Table에는 `행`에서 고유하게 식별 가능한 `기본 키`라는 속성이 있다.
- `외래 키`를 사용하여 각 행에서 서로 다른 테이블 간의 `관계`를 만들 수 있음
- 데이터는 기본 키 또는 외래 키를 통해 `결합(join)`될 수 있는 여러 테이블에 걸쳐 구조화 됨

# SQL - Basics
- 컴퓨터와의 대화 -> 프로그래밍 언어
- 관계형 데이터베이스와의 대화 -> `SQL`

## 1. SQL (Structure Query Language)
- 데이터베이스에 정보를 저장하고 처리하기 위한 프로그래밍 언어

### `Structure`
- 테이블의 형태로 `구조화`된 관계형 데이터베이스

### `Structure Query`
- 테이블의 형태로 `구조화`된 관계형 데이터베이스에게 요청을 `질의(요청)`

## SQL Syntax
```
SELECT column_name FROM table_name;
```
- SQL 키워드는 `대소문자를 구분하지 않음`
  - 하지만 대문자로 작성하는 것을 권장 (명시적 구분)
- 각 SQL Statements의 `끝`에는 `세미콜론(;)이 필요`
  - 세미콜론은 각 SQL Statements을 구분하는 방법

## 2. SQL Statements
- SQL 언어를 구성하는 가장 기본적인 코드 블록

### SQL Statements 예시
```
SELECT column_name FROM table_name;
```
- 해당 예시 코드는 SELECT Statement라 부름
- 이 Statement는 SELECT, FROM 2개의 Keyword로 구성 됨

### SQL Statements 유형

1. DDL (Data Definition Language)
  - 데이터의 기본 구조 및 형식 변경
  - SQL 키워드 : CREATE, DROP, ALTER

2. DQL (Data Query Language)
  - 데이터 검색
  - SQL 키워드 : SELECT

3. DML (Data Manipulation Language)
  - 데이터 조작 (추가, 수정, 삭제)
  - SQL 키워드 : INSERT, UPDATE, DELETE

4. DCL (Data Control Language)
  - 데이터 및 작업에 대한 사용자 권한 제어
  - SQL 키워드 : COMMIT, ROLLBACK, GRANT, REVOKE

## 정리
- SQL은 데이터베이스와 `상호 작용`하고 데이터베이스에서 `데이터를 반환`하기 위한 언어

## 용어 정리

### Query
- 질의, 질문
- `"데이터베이스로부터 정보를 요청"` 하는 것
- 일반적으로 SQL로 작성하는 코드를 쿼리문(SQL문)이라함