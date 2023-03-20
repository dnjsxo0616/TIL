# 1. Transactions
- 여러 쿼리문을 묶어서 하나의 작업처럼 처리하는 방법
- (다 성공하던지 혹은 다 실패하던지 해야하는) 여러 쿼리문을 묶어서 하나의 작업처럼 처리하는 방법

## Transaction 예시
- 계좌이체 (인출 & 입금)
  - 송금 중에 알 수 없는 문제로 인출에는 성공했는데 입금에 실패한다면..?
  - 인출과 입금 모두 성공적으로 끝나야 거래가 최종 승인되고, 중간에 문제가 발생한다면 거래를 처음부터 없었던 거래로 만들어야 함
  - 결국 함께 성공하던지 실패해야 함

## Transaction 구조
```sql
START TRANSACTION;
state_ments;
... -- (출금과 송금이 들어간다.)
[ROLLBACK|COMMIT]; -- 마무리는 ROLLBACK, COMMIT 두 가지다.
```
- START TRANSACTION
  - 트랜잭션 구문의 시작을 알림
- COMMIT
  - 모든 작업이 정상적으로 완료되면 한꺼번에 DB에 반영
- ROLLBACK
  - 부분적으로 작업이 실패하면 트랜잭션에서 진행한 모든 연산을 취소하고 트랜잭션 실행 전으로 되돌림

```sql
-- 자동 COMMIT 비활성화
SET autocommit = 0; -- 활성하는 1

-- users 테이블 생성
CREATE TABLE users (
  id INT AUTO_INCREMENT,
  name VARCHAR(10) NOT NULL,
  PRIMARY KEY (id)
);
```
- 기본적으로 MySQL은 자동으로 변경 사항을 COMMIT 한다.
- 변경 사항을 자동으로 COMMIT 하지 않도록 다음과 같이 설정한다.

## Transaction 원리
```sql
START TRANSACTION;
INSERT INTO ...
INSERT INTO ...
```
- 만약 `INSERT`가 두 번 일어났다고 하면 이 데이터들이 실제 데이터가 바로 반영이 되는 것이 아니라 `임시 데이터 영역`에서 잠시 대기를 한다.
- 문제가 없이 `TRANSACTION`의 데이터들이 `COMMIT`이 이루어지면 영구 데이터 영역이 되고,
- `ROLLBACK`이 일어나면 데이터를 `파기`한다.

## Transaction 정리

- 쪼개질 수 없는 업무처리의 단위
- 전체가 수행되거나 또는 전혀 수행되지 않아야 한다.(All or Nothing)

# 2. Triggers
- 특정 이벤트`(INSERT, UPDATE, DELECT)`에 대한 응답으로 자동으로 실행되는 것이다.

- 트리거의 예시(~ 하겠다.)
  - ~ 를 추가한 후에 `~ 하겠다.`
  - ~ 를 수정하기 전에 `~ 하겠다.`
  - ~ 를 삭제한 후에 `~ 하겠다.`
  - 유저가 글을 쓰면 `자동으로 누가 글을 썼는 지 다른 테이블에 기록하는 것`
## Triggers 구조

```sql
CREATE TRIGGER trigger_name
  {BEFORE | AFTER} {INSERT | UPDATE | DELETE } -- 언제?
  ON table_name FOR EACH ROW -- 어디서?
  trigger_body; -- 트리거 코드 (~하겠다.)
```
- CREATE TRIGGER 키워드 다음에 생성하려는 트리거의 이름을 지정
- 각 레코드의 어느 지점에 트리거가 실행될지 지정 (삽입, 수정, 삭제 전/후)
- ON 키워드 뒤에 트리거가 속한 테이블의 이름을 지정
- 트리거가 활성화될 때 실행할 코드를 trigger_body에 지정
  - 여러 명령문을 실행하려면 BEGIN END 키워드로 묶어서 사용
- 트리거는 `DML`의 영향을 받는 필드 값에만 적용할 수 있다.

```sql
-- 트리거를 사용해 기존 게시글이 수정되면, 게시글의 수정일자 필드 값을 최신 일자로 업데이트하기
INSERT INTO articles (title, createdAt, updatedAt)
VALUES ('title', CURRENT_TIME(), CURRENT_TIME());


DELIMITER //
-- SQL의 구문 문자(;)를 변경한다. DELIMITER가 나올 때까지 종료 문자를 //로 설정한다.
CREATE TRIGGER beforeArticleUpdate
  BEFORE UPDATE 
  -- 업데이트 전은 최종적으로 업데이트 커밋이 일어나기 전에 CURRENT_TIME()도 같이 수정하는 것이다.
  -- 업데이트 후는 이미 데이터가 다른 것으로 수정된 후다. 그럼 수정된 걸 다시 수정하는 거라 2회 수정이다.
  ON articles FOR EACH ROW 
  -- 트리거가 바라보는 테이블 / ROW는 모든 행에 적용
BEGIN
-- 하나 이상의 구문 목록을 표현한다.
-- 트리거가 커지면 다중 구문을 구성하게 된다.
  SET NEW.updatedAt = CURRENT_TIME();
  -- 트리거 코드
  -- NEW는 트리거에서 특점 시점 전/후의 값에 접근 할 수 있도록 제공하는 키워드
  -- OLD와 NEW 2개 제공
  -- INSERT : NEW 가능 (OLD는 새로운 데이터를 삽입하기 때문에 말이 안된다.) / UPDATE : OLD, NEW 가능 / DELETE : OLD 가능
END//
DELIMITER ;
-- BEGIN-END 구문 사이에 여러 SQL문이 작성되기 때문에 하나의 트리거로써 작동될 수 있도록 사용
-- 종료 문자가 (;)로 다시 변경한다.
```
## Triggers 관련 추가 명령문
```sql
-- 트리거 목록 확인
SHOW TRIGGERS;

-- 트리거 삭제
DROP TRIGGER trigger_name;
```
