# 9장 스택, 큐
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
#### 풀이1. push()할 때 큐를 이용해 재정렬
스택이나 큐 ADT(추상 자료형)를 실제로 구현할 때는, 대개 스택은 연결 리스트로 하고 큐는 배열로 한다. 그런데 여기서는 재밌게도 큐로 스택을 디자인하라고 한다.

파이썬의 리스트나 데크는 스택과 큐의 모든 기능을 제공하기 때문에 이 문제는 원래 큐 ADT에는 없지만 파이썬의 자료형에서 지원하는 기능을 사용하면 매우 쉽게 풀 수도 있다.<br>
그러나 여기서는 문제의 의도에 맞게 큐의 FIFO에 해당하는 연산만 사용해서 구현해본다.<br>
먼저, 큐의 선언은 데크(10장 참조)로 한다.
```python
self.q = collections.deque()
```
큐를 데크로 선언했지만 문제의 의도에 맞게 여기서는 큐의 연산만을 사용해 구현할 것이다.<br>
push()할 때만 다소 복잡한 편인데, 다음과 같이 요소를 삽입한 후에 방금 삽입한 요소를 맨 앞에 두는 상태로 전체를 재정렬한다.
```python
self.q.append(x)
for _ in range(len(self.q) - 1):
    self.q.append(self.q.popleft())
```
이렇게 하면 큐에서 맨 앞 요소를 끄집어낼 때 스택처럼 가장 먼저 삽입한 요소가 나오게 될 것이다.<br>
물론 요소 삽입 시 시간 복잡도가 O(n)이 되어 다소 비효율적이긴 하지만, 이렇게 하면 스택을 큐로 어렵지 않게 구현할 수 있다.

전체 코드는 다음과 같다.
```python
class MyStack:
  def __init__(self):
    self.q = collections.deque()

  def push(self, x):
    self.q.append(x)
    # 요소 삽입 후 맨 앞에 두는 상태로 재정렬
    for _ in range(len(self.q )- 1):
      self.q.append(self.q.popleft()

  def pop(self):
    return self.q.popleft()
    
  def top(self):
    return self.q[0]

  def empty(self):
    return len(self.q) == 0
```
<br><br>

### 문제 24 스택을 이용한 큐 구현
* **내가 짠 코드**<br>
```python
class Node(object):
  def __init__(self, val=0, next=None):
    self.val = val
    self.next = next

class Stack(object):
  def __init__(self):
    self.last = None

  def push(self, x):
    self.last = Node(x, self.last)
    cur = self.last
    
    if cur.next != None:
        while cur.next != None:
          cur = cur.next
        last = Node(self.last.val,None)
        self.last = self.last.next
        cur.next = last

  def pop(self):
    last = self.last.val
    self.last = self.last.next
    return last

  def peek(self):
    return self.last.val

  def empty(self):
    return self.last == None


q = Stack()
q.push(1)
q.push(2)
print(q.peek())
print(q.pop())
print(q.empty())
```
<br><br>

### 문제 24 스택을 이용한 큐 구현
#### 풀이1. 스택 2개 사용
이번에는 큐를 스택으로 구현하는 방법이다. 앞서 스택 구현과 비슷한 방법으로 할 수 있을까?

앞서 문제 풀이에서 push() 부분을 다시 한번 살펴보면 다음과 같다.
```python
self.q.append(x)
for _ in range(len(self.q )- 1):
    self.q.append(self.q.popleft()
```
여기에는 앞서와는 다른 중요한 차이점이 있다.<br>
지난 풀이에서는 큐에 요소를 삽입한 후 맨 앞의 요소부터 끄집어냈다. 그렇게 해서 원래의 큐에 덧붙여 나가는 형태로, 추가 공간 없이 풀이가 가능했다.<br>
그러나 이번에는 맨 뒤의 아이템을 끄집어낼 수밖에 없다. 이렇게 하면 다음번에 또 다시 맨 뒤의 아이템을 끄집어 내게 되는데, 결국은 무한반복 문제에서 헤어나올 수 없다는 점이 문제다.

즉 이전과 동일하게 하나의 큐를 이용해서는 풀이할 수 없다. 따라서 이 문제를 스택의 연산만을 사용해서 풀기 위해서는 2개의 스택이 필요하다.<br>
특히 pop()과 peek()는 결국 같은 데이터를 끄집어낸다는 점에 착안해, 이번에는 pop()을 할 때 peek()를 호출하고 여기에 재입력하는 알고리즘을 다음과 같이 구현했다.
```python
if not self.output:
    while self.input:
        self.output.append(self.input.pop())
```
또한 이렇게 구현해도 output의 값이 모두 팝(pop())되기 전까지는 다시 재입력이 일어나지 않기 때문에, 분할 상환 분석에 따른 시간 복잡도는 여전히 O(1)이다.
```python
class MyQueue:
    def __init__(self):
        self.input = []
        self.output = []
        
    def push(self, x):
        self.input.append(x)
        
    def pop(self):
        self.peek()
        return self.output.pop()
        
    def peek(self):
        # output이 없으면 모두 재입력
        if not self.output:
            while self.input:
                self.output.append(self.input.pop())
        return self.output[-1]
        
    def empty(self):
        return self.input == [] and self.output == []
```
앞서 문제와 달리 좀 더 효율적으로 구현하기 위해 2개의 스택을 사용했고, 앞서와 달리 push()는 간단한 반면, 여기서는 peek()를 좀 더 복잡한 형태로 구현했다.
<br><br>




























