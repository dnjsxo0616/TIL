# 1. 개요

## 1-1 HTTP Request Methods
- 리소스(resource, 자원)에 대한 행위(수행하고자 하는 동작)를 정의 HTTP verbs 라고도 한다.

## 1-2 대표 HTTP Request Methods
### 1. GET
  - 서버에 리소스의 표현을 요청
  - GET을 사용하는 요청은 데이터만 검색해야 한다.

### 2. POST
  - 데이터를 지정된 리소스에 제출
  - 서버의 상태를 변경

### 3. PUT
  - 요청한 주소의 리소스를 수정

### 4. DELETE
  - 지정된 리소스를 삭제

## 1-3 HTTP response status codes
- 특정 HTTP 요청이 성공적으로 완료되었는 지 여부를 나타냄
- 응답은 5개의 그룹으로 나뉨
  1. Informational responses (100-199)
  2. Successful responses (200-299)
  3. Redirection messages (300-399)
  4. Client error responses (400-499)
  5. Server error responses (500-599)

# 2. REST API

## 2-1 API (Application Programming Interface)
- 애플리케이션과 프로그래밍으로 소통하는 방법

## 2-2 API
### API는 복잡한 코드를 추상화하여 대신 사용할 수 있는 몇 가지 더 쉬운 구문을 제공
  - 예를 들어 집의 가전 제품에 전기를 공급