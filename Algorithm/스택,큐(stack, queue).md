# 1. 스택 (Stack)

- 데이터를 임시 저장하기 위해 사용하는 자료구조이며, 데이터를 입력하고 출력하는 방향이 정해져있다는 점이 서로 비슷하다.

- Stack은 쌓는다는 의미로써, `데이터를 한쪽에서만 넣고 빼는 자료구조`이다.
- 가장 마지막에 들어온 데이터가 가장 먼저 나가므로 LIFO(Last-in-First-out, `후입선출`) 방식

## :question: 왜/언제 stack을 사용할까?

- 이전 작업의 기억(뒤집기, 되돌리기, 되돌아가기)
  - 괄호 매칭
  - 함수 호출(재귀 호출)
  - 백트래킹
  - DFS(깊이 우선 탐색)

- 파이썬은 `리스트(List)로 스택을 간편하게 사용할 수 있다.
 - .append()
 - .pop()

 # 2. 큐 (Queue)

- Queue는 `한 쪽에서 데이터를 넣고, 다른 한 쪽에서만 데이터를 뺄 수 있는 자료구조`이다. 
- 즉, 양쪽 끝에서 데이터의 삽입과 삭제가 각각 이뤄진다.

- 가장 먼저 들어온 데이터가 가장 먼저 나가므로 FIFO(First-in First-out, `선입선출`) 방식

```python
n = int(input())
queue = list(range(1, n+1))

while len(queue) > 1:
  print(queue.pop(0), end = ' ')
  queue.append(queue.pop(0))
print(queue[0])
>>> pop(0)은 비효율적이다.
```

## 큐의 맨 앞 데이터를 가져오는 행위
- dequeue

## 큐의 맨 뒤에 데이터를 삽입하는 행위
- enqueue

## :question: 왜/언제 Queue을 사용할까?
- 순서와 대기
- 프로세스 관리(데이터 버퍼)
- 클라이언트/서버 (Message Queue)
- BFS(너비 우선 탐색)
- 다익스트라 - 우선순위큐

## 큐 자료구조도 파이썬 리스트(List)로 간편하게 사용 가능!

- Queue
dequeue <- [1, 2, 3, 4] <- enqueue

- List
pop(0) <- [1, 2, 3, 4] <- append()

###  리스트를 이용한 큐 자료구조의 단점
- `데이터를 뺄 때` 큐 안에 있는 데이터가 많은 경우 `비효율적`이다.
- 맨 앞 데이터가 빠지면서, 리스트의 인덱스가 하나씩 당겨 지기 때문이다.

## 덱 (Deque, Double-Ended Queue) 자료구조

```python
from collections import deque

d = deque()
print(type(d)) # <class 'collections.deque'>

>>> 스택이나 큐처럼 덱의 오른쪽에서 요소 삽입 : append
for i in range(10):
  d.append(i)
print(d) # deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

>>> 덱의 왼쪽에서 요소 삽입 : appendleft
d.appendleft(10)
print(d) # deque([10, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

>>> 덱 중간에 요소 삽입 : insert
d.insert(5,11)
print(d) # deque([0, 1, 2, 3, 4, 11, 5, 6, 7, 8, 9])

>>> 덱 왼쪽/오론쪽에 통째로 append
d.extend([0, 0, 0])
print(d) # deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 0, 0])
d. extendleft([0, 0, 0])
print(d) # deque([0, 0, 0, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

>>> 스택처럼 덱의 오른쪽에서 요소 삭제 : pop
d.pop()
print(d) # deque([0, 1, 2, 3, 4, 5, 6, 7, 8])

>>> 큐처럼 덱의 왼쪽에서 요소 삭제 : popleft
d.popleft()
print(d) # deque([1, 2, 3, 4, 5, 6, 7, 8, 9])

>>> 일반적인 리스트로 전환도 가능하다.
print(list(d)) # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## :bulb: 큐(queue)말고 덱(deque)을 사용하는 이유

- `덱(deque)`은==`양 방향`으로 삽입과 삭제가 자유로운 큐
appendleft() -> [1, 2, 3, 4] <- append()
popleft() ->                 -> pop()

- 따라서 데이터의 삽입 추출이 많은 경우, 시간을 단축 시킬 수 있다.

