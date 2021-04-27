## 큐
> 큐(Queue)는 시퀀스의 한쪽 끝에는 엔티티(객체)를 추가하고, 다른 반대쪽 끝에는 제거할 수 있는 엔티티 컬렉션이다.

**FIFO(First-In-First-Out)(선입선출)** 로 처리되는, 줄을 서는 것에 비유할 수 있는 큐는 상대적으로 스택에 비해서는 쓰임새가 적다.<br>
그러나 스택에 비해서 그렇다는 얘기일 뿐, 이후에 살펴보게 될 데크(Deque)나 우선순위 큐(Priority Queue) 같은 변형들은 여러 분야에서 매우 유용하게 쓰인다.

이외에도 너비 우선 탐색(Breadth-First Search)(BFS)아니 캐시 등을 구현할 때도 널리 사용된다.<br>
파이썬에는 동일한 이름의 queue라는 모듈이 있긴 하지만 이 모듈은 사실상 자료구조로서의 큐보다는 동기화 기능에 집중된 모듈로, 큐 자료형을 위해 널리 쓰이는 모듈은 아니다.<br>
사실상 파이썬의 리스트는 큐의 모든 연산을 지원하기 때문에 그대로 사용해도 무방하지만 좀 더 나은 성능을 위해서는 이후에 살펴볼 양방향 삽입, 삭제가 모두 O(1)에 가능한 데크를 사용하는 편이 가장 좋다.
<br><br>

## 리트코드
### 문제 23 큐를 이용한 스택 구현
> 255p

* **내가 짠 코드**<br>
풀이 보고 코드 짬
```python
import collections

class MyStack(object):
  def __init__(self):
    self.q = collections.deque()

  def push(self,a):
    self.q.append(a)

    for _ in range(len(self.q )- 1):
      self.q.append(self.q.popleft())

  def top(self):
    return self.q[0]

  def pop(self):
    return self.q.popleft()

  def empty(self):
    return len(self.q) == 0



stack = MyStack()
stack.push(1)
stack.push(2)
print(stack.top())
print(stack.pop())
print(stack.empty())
```
<br><br>

### 문제 23 큐를 이용한 스택 구현 풀이
#### 
