# 힙(Heap), 셋(Set)

일반적인 큐(queue)는 순서를 기준으로 가장 먼저 들어온 데이터가 가장 먼저 나가므로 FIFO(First-in First-out, 선입선출)방식

## :question: 순서가 아닌 다른 기준으로는?

- 우선순위 큐(Priority Queue)는 우선순위(중요도, 크기 등 순서 이외의 기준)를 기준으로 가장 우선순위가 높은 데이터가 가장 먼저 나가는 방식

## 우선 순위 큐(Priority Queue)

- 순서가 아닌 우선순위를 기준으로 가져올 요소를 `결정(dequeue)하는 큐`
  1. 가중치가 있는 데이터
  2. 작업 스케줄링
  3. 네트워크

## 우선 순위 큐(Priority Queue)를 구현하는 방법

1. 배열(Array)

2. 힙(Heap)

# 1. 힙 (Heap)

- 최대값 또는 최소값을 빠르게 찾아내도록 만들어진 구조
- 완전 이진 트리의 형태로 `느슨한 정렬 상태를 지속적으로 유지`한다.
- 힙 트리에서는 중복 값을 허용한다.

## :question: Heap은 언제 사용해야할까?

1. 데이터가 지속적으로 정렬되야 하는 경우
2. 데이터에 삽입/삭제가 빈번할 때

## 큐와 힙의 사용법 비교

- Queue
dequeue <- [가장오래된 데이터, , , 가장 최신의 데이터] <- enqueue

- Heap(min)
heapq.heappop() <- [가장 작은 데이터, , , , 가장 큰 데이터] <- heapq.heappush()

## 삽입 연산(insertion)과 삭제연산(deletion)

### 삽입 연산
1. 삽입하고자 하는 값을 트리의 가장 마지막 원소에 추가
2. 부모 노드와의 대소 관계를 비교하면서 만족할 때까지 `자리 교환`을 반복한다. (트리에서)

### 삭제 연산
1. 힙에서는 루트 노드만 삭제 가능하므로 `루트 노드(트리 구조에서 최상위에 있는 노드)`를 제거한다.
2. 가장 마지막 노드를 루트로 이동시킨다.

## heapq module
- 파이썬에서 제공하는 힙큐 모듈, 일반적인 리스트를 min heap으로 다룰 수 있게 해준다.

### 모듈 임포트
```python
import heapq
>>> 사용시 heapq.heappush로 적어줘야함
```
```python
from heapq import heappush, heappop //,.... 다른 함수들
>>> 사용할 때 heappush 함수만 적으면됨
```

### 노드 추가 : heappush 메소드
- heapq.heappush(heap(변수명), item(넣을 값))
```python
import heapq

numbers = [1, 100, 3, 5, 6, 20]
heapq.heqppush(numbers, 0)
print(heapq) # [0, 1, 5, 3, 100, 6, 20]
```

### 기존에 사용한 리스트를 힙으로 변환하기 : heapify 메소드
```python
import heapq

numbers = [1, 100, 3, 5, 6, 20]
heapq.heapify(numbers)
print(numbers) # [1, 5, 3, 100, 6, 20]  배열이 바꼈다.
"""
     1              1
    / \            / \
  100  3    ->    5   3
 /  \   \        / \   \
5   6   20     100  6   20
""" # 이런 형태로 바뀜
```

## 힙에서 원소 삭제, 노드 삭제 : heappop 메소드
- 가장 작은 원소를 꺼내며 리턴, 자동적으로 그 다음 작은 원소가 루트 노드가 됨
```python
import heapq
numbers = [1, 100, 3, 5, 6, 20]

heapq.heappop(numbers)
print(numbers) # [5, 3, 100, 6, 20]
>>> 계속 heapop(numbers)를 하게 되면 순차적으로 가장 작은 값들이 빠져나온다.
```

## 최대 힙
- heapq 모듈은 `최소 힙(min heap)`을 기능만을 동작한다. 그래서 값을 `마이너스(-)`로 바꾸면 가장 큰 값이 맨 처음에 나온다

```python
import heapq

list = [2, 6, 9, 4, 7, 1]
heap3 = []
for item in list:
    heapq.heappush(heap3, -item)
print(heap3) # [-9, -7, -6, -2, -4, -1]

while heap3:
    print(heapq.heappop(heap3)*(-1))
"""
9
7
6
4
2
1
"""
```

# 셋(Set)
- 집합을 나태니기 위한 자료형
- 중복을 허용하지 않는다
- 순서가 없다

## :question: Set은 언제 사용해야할까?
1. 데이터의 중복이 없어야 할 때(고유값들로 이루어진 데이터가 필요할 때)
2. 정수가 아닌 데이터의 삽입/삭제/탐색이 빈번히 필요할 때

## Set 생성하기
```python
set_t1 = {}
set_t2 = set([iterable])

mamamoo = {"솔라", "휘인", "화사", "문별", "제니"}

a = set('apple') 
# {'e', 'l', 'a', 'p'}
```

## Set 연산

1. .add()
2. .remove()
3. | (합)
4. - (차)
5. & (교)
6. ^ (대칭차)