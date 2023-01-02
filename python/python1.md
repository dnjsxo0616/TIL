# Python 기초

## 컴퓨터 프로그래밍 언어

### **프로그래밍**

: 명령어의 모음(집합)

### **언어**

자신의 생각을 나타내고 전달하기 위해 사용하는 체계 문법적으로 맞는 말의 집합

:bulb: 파이썬에서는 대문자 소문자 구분!
---

## **객체와 변수**

### 객체

- 객체<br/>
: 어떠한 속성값과 행동을 가지고 있는 데이터 ex) 자동차를 사전적 의미만 담는 것이 아니라 내가 탈 수 있고, 차량번호 정보 등 이러한 정보와 행위를 묶은 데이터를 하나의 자동차 객체로 볼 수 있다.

### 변수

- 변수<br/>
: 메모리에 저장되어 있는 객체를 참조하기 위해 사용되는 이름<br/>
: 동일 이름에 다른 객체를 언제든 `할당`할 수 있기 때문에 변수라고 불림
```python
X = 10
```
- 변수 연산
```python
i = 5
j = 3
# 1.
i + j # 5 + 3
```
- 변수 할당<br/>
: 같은 값을 동시에 할당할 수 있음
```python
x = y = 1004
```
: 다른값을 동시에 할당 할 수 있음
```python
x, y = 1, 2
```
: Error
```python
x,y = 1
# Type Error Traceback(most recent call last)
# cannot unpack non-iterable int object => 할당하지 않는 변수 있음.
```
```python
x,y = 1,2,3
# Type Error Traceback(most recent call last)
# too many values to unpack => 너무 많은 값들이 있음
```
## :question: x = 10 y = 20 일 때, 각각 값을 바꿔서 저장하는 코드를 작성해라.<br/>
```python
# 임의의 변수를 설정
tmp = x
x = y
y = tmp
```
```python
# 할당값을 서로 바꾼다
x,y = y,x
```

## **데이터 타입(자료형)**
---
## 숫자
---
### 수치형

- 정수(int)<br/>
: 모든 정수의 타입은 int  
: 오버플로우(데이터 타입별로 가능한 메모리 크기를 넘는 것)가 발생하지 않는다.

- 실수(Float)  
: 부동소수점  
  - 2진수로 숫자를 표현<br/>
  
  : Floating point rounding error - 값 비교하는 과정에서 실수인 경우 주의
```python
3.14 - 3.02 == 0.12 # 거짓이다
# 0.12000000000000001
```
- ans
```python
# 1. 매우 작은 수 보다 작은지 확인 
abs(a - b) <= 1e-10

# 2. math 모듈 활용
import math
math.isClose(a,b)
```

### 불린형(Boolean Type)
: True(1) / False(0) 값을 가진 타입은 bool
: 비교/논리 연산을 수행함에 있어서 활용됨

### 연산자(operator)

- 산술 연산자<br/>
// : 몫, ** : 거듭제곱

- 복합 연산자<br/>
: 연산과 할당이 함께 이뤄짐 Ex) a += b -> a = a+b

- 비교 연산자
: 값을 비교하며, True / False 값을 리턴함 Ex) == : 같음. != : 같지않음

- 논리 연산자
: 논리식을 판단하여 True와 False를 반환함<br/>Ex) A and B : A,B 모두 True시 Ture<br/>A or B : A와 B 모두 False시 False<br/>Not : Ture를 False로, False를 True로
---

## 컨테이너(Container)
: 여러 개의 값을 담을 수 있는 것(객체)  
: 순서가 있다 != 정렬되어 있다.

### 시퀀스 
- 문자열  
: 모든 문자는 str타입  
: 문자열은 작은 따옴표(')나 큰 따옴표(")를 활용

- 인덱싱  
: s[1] = 'b' -> 0부터 세어나간다. Ex) [a b c d] `index->` [0 1 2 3]  
- 슬라이싱  
: s[2:5] -> 문자열[시작 인덱스 : 끝 인덱스(포함하지 않는다.):step(건너뛰는)]  
```python
num = [1 2 3 4]
num[2:4] # 2 3 
num[0:4:2] # 1 3
```

### 리스트(List)
: 변경 가능한 값들의 나열된 자료형 즉 배열  
: 변경 가능, 반복 가능, 요소는 콤마로 구분

- 리스트 생성
```python
#1
my_list = []
type(my_list) # <class 'list>

#2 (권장하지 않는다.)
another_list = list()
type(another_list) # <class 'list>
```

- 리스트의 접근과 변경
```python
# 값 접근
a = [1, 2, 3]
print(a[0]) # 1

# 값 변경
a[0] = '1'
print(a) # ['1', 2, 3]
```

- 리스트 값 추가/삭제
```python
# 값 추가 .append()
even_numbers = [2,4,6,8]
even_numbers.append(10)
print(even_numbers) # [2,4,6,8,10]

# 값 삭제 .pop()
even_numbers = [2,4,6,8]
even_numbers.pop(0)
print(even_numbers) # [4,6,8]
```
### None
: 값이 없음을 표현하는 자료형 중 하나이다.  
: 일반적으로 반환 값이 없는 함수에서 사용하기도 함.
```python
print(type(None)) # <class 'NoneType'>
a = None
print(a) # None
```
---
# 단축키
- 주석 처리하기 :  Ctrl + /
- 파일 만들기 : Ctrl + n
- 터미널 정리하기 : clear
- 블록 씌우기 : Shift 누르고 방향키로 선택