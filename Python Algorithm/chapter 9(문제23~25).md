# 목차
* [chapter 9. 스택, 큐](#9장-스택-큐)
  + [큐](#큐)
* [리트코드](#리트코드)
  + [문제 23 큐를 이용한 스택 구현](#문제-23-큐를-이용한-스택-구현)
  + [문제 23 큐를 이용한 스택 구현 풀이](#문제-23-큐를-이용한-스택-구현-풀이)
    - [풀이1. push( ) 할 때 큐를 이용해 재정렬](#풀이1-push할-때-큐를-이용해-재정렬)
  + [문제 24 스택을 이용한 큐 구현](#문제-24-스택을-이용한-큐-구현)
  + [문제 24 스택을 이용한 큐 구현 풀이](#문제-24-스택을-이용한-큐-구현-풀이)
    - [풀이1. 스택 2개 사용](#풀이1-스택-2개-사용)
  + [문제 25 원형 큐 디자인](#문제-25-원형-큐-디자인)
  + [문제 25 원형 큐 디자인 풀이](#문제-25-원형-큐-디자인-풀이)
    - [풀이1. 배열을 이용한 풀이](#풀이1-배열을-이용한-풀이)

<br><br>



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
    for _ in range(len(self.q) - 1):
      self.q.append(self.q.popleft())

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
for _ in range(len(self.q) - 1):
    self.q.append(self.q.popleft())
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

### 문제 25 원형 큐 디자인
> 259p

* **내가 짠 코드**<br>
```python
class MyCircularQueue(object):
  def __init__(self, k):
    self.q = [None] * k
    self.size = k
    self.idx = 0
    self.rear = 0

  def enQueue(self, x):
    if self.q[idx] == None:
      self.q[idx] = x
      self.rear = idx
      self.idx  = (self.rear + 1)
      return True
    else:
      return False

  def Rear(self):
    return self.q[rear]

  def isFull(self):
    for i in range(self.size):
      if self.q[i] != None:
        continue
      else:
        return False
    return True

  def deQueue(self):
    self.q[0] = None
```
-> 풀기 실패함<br>

```python
class MyCircularQueue(object):
  def __init__(self, k):
    self.q = [None] * k
    self.size = k
    self.front = 0 # 원소가 있는 첫 인덱스
    self.rear = 0 # 원소가 없는 첫 인덱스

  def enQueue(self, x):
    rear = self.rear
    if self.q[rear] == None:
      self.q[rear] = x
      
      if self.rear == self.size-1:
        self.rear = 0
      else:
        self.rear += 1
      return True
    else:
      return False

  def Rear(self):
    if self.q[self.rear - 1] != None:
      return self.q[self.rear - 1]
    else:
      return '리턴할 요소가 없습니다.'

  def isFull(self):
    for i in range(self.size):
      if self.q[i] != None:
        continue
      else:
        return False
    return True

  def deQueue(self):
    if self.q[self.front] != None:
      self.q[self.front] = None
      
      if self.front == self.size-1:
        self.front = 0
      else:
        self.front += 1
      return True
    else:
      return False

  def Front(self):
    if self.q[self.front] != None:
      return self.q[self.front]
    else:
      return '리턴할 요소가 없습니다.'



circularQueue = MyCircularQueue(5)
print(circularQueue.enQueue(10))
print(circularQueue.enQueue(20))
print(circularQueue.enQueue(30))
print(circularQueue.enQueue(40))
print(circularQueue.Rear())
print(circularQueue.isFull())
print(circularQueue.deQueue())
print(circularQueue.deQueue())
print(circularQueue.enQueue(50))
print(circularQueue.enQueue(60))
print(circularQueue.Rear())
print(circularQueue.Front())
```
-> 원형 큐의 삽입과 삭제 원리 파악 후 다시 짜본 코드(완성)
<br><br>

### 문제 25 원형 큐 디자인 풀이
#### 풀이1. 배열을 이용한 풀이
원형 큐(Circular Queue)라는 명칭은 생소할 수도 있을 것 같다. 오래된 국내 책에서는 '원형 큐'가 아닌 '환형 큐'로 표기되어 있어, 예전에 공부한 분들이라면 '환형 큐'라는 명칭이 더 낯익을지도 모르겠다.

'원형 큐'는 FIFO 구조를 지닌다는 점에서 기존의 큐와 동일하다. <br>
그러나 마지막 위치가 시작 위치와 연결되는 다음 그림 9-6(259p)과 같은 원형 구조를 띠기 때문에, 링 버퍼(Ring Buffer)라고도 부른다.

기존의 큐는 공간이 꽉 차게 되면 더 이상 요소를 추가할 수 없었다. 심지어 앞쪽에 요소들이 deQueue()로 모두 빠져서 충분한 공간이 남게 돼도 그쪽으로는 추가할 수 있는 방법이 없다.<br>
그래서 앞쪽에 공간이 남아 있다면 이 그림처럼 동그랗게 연결해 앞쪽으로 추가할 수 있도록 재활용 가능한 구조가 바로 원형 큐다.<br>
원형 큐의 삽입과 삭제 원리는 다음 그림 9-7(260p)과 같다.

동작하는 원리는 투 포인터와도 비슷하다. <br>
마지막 위치와 시작 위치를 연결하는 원형 구조를 만들고, 요소의 시작점과 끝점을 따라 투 포인터가 움직인다.<br>
그림 9-7(260p)처럼 enQueue()를 하게 되면 rear 포인터가 앞으로 이동하고, deQueue()를 하게 되면 front 포인터가 앞으로 이동한다.<br>
이렇게 enQueue()와 deQueue()를 반복하게 되면 서로 동그랗게 연결되어 있기 때문에 투 포인터가 빙글빙글 돌면서 이동하는 구조가 된다.

이 그림의 enQueue(60) 이후에는 rear 포인터가 원래의 front 포인터 자리까지 도달해 빙글빙글 돌고 있는 모습을 확인할 수 있다.<br>
만약 rear 포인터가 front 포인터와 같은 위치에서 서로 만나게 된다면, 다시 말해 만나는 위치까지 이동했다면, 그때는 정말로 여유 공간이 하나도 없다는 얘기가 되므로 공간 부족 에러를 발생시킨다.

원형 큐를 구현하는 방법에는 여러 가지가 있으나 여기서는 가장 일반적인 방법인 배열로 구현해본다. <br>
아울러 배열로 구현했을 대 공간을 재활용한다는 원형의 이점을 내세울 수 있기도 하다.
```python
def __init__(self, k: int):
    self.q = [None] * k
    self.maxlen = k
    self.p1 = 0
    self.p2 = 0
```
먼저, 이처럼 초기화 시에는 큐의 크기 k를 입력값으로 받는다. k 값은 당연히 최대 길이인 maxlen이 되고, front 포인터는 p1으로 정하고, rear 포인터는 p2로 정한다. 둘 다 초깃값은 0으로 한다.

다음은, 요소를 삽입하는 enQueue() 연산을 구현해보자.
```python
def enQueue(self, value: int) -> bool:
    if self.q[self.p2] is None:
        self.q[self.p2] = value
        self.p2 = (self.p2 + 1) % self.maxlen
        return True
    else:
        return False
```
rear 포인터 p2 위치에 값을 넣고, 포인터를 한 칸 앞으로 이동한다. <br>
단, 전체 길이만큼 모듈로(나머지) 연산을 하여 포인터의 위치가 전체 길이를 벗어나지 않게 한다.<br>
만약 rear 포인터의 위치가 None이 아니라면 다른 요소가 있는 공간이 꽉 찬 상태이거나 비정상적인 경우이므로 False를 리턴한다.

* **참고 | 모듈로 연산**<br>
  모듈로 연산(modulo)은, 앞의 숫자를 뒤의 숫자로 나눴을때, 그 나머지를 나타낸다.<br>
  **모듈로 연산자는 % 이다.** 따라서 퍼센트를 나타내는 것이 아니라, 나머지를 나타낸다.<br>
  아래에서 11 % 3 은, 11을 3으로 나눴을때의 나머지를 구하라는 얘기이므로, 나머지가 2 이다.<br>
<br><br>

이제 요소를 삭제하는 deQueue() 연산을 구현할 차례다. 그런데 이 문제에서 요구하는 deQueue() 연산은 요소를 꺼내지 않고 삭제만 수행하도록 정의되어 있다.<br>
큐에서 요소를 꺼내는 것은 문제의 예제에서 보듯 앞쪽은 Front()로, 뒤쪽은 Rear()로, 각기 서로 다른 연산으로 정의되어 있다. 다소 특이한 제약 사항으로, "Introduction to Algorithms" 책이나 리스트 9-1(261p)에 정리한 워싱턴 주립대학 강의 자료 수도코드르 살펴보면, 동일한 원형 큐를 구현하면서 분명히 deQueue()에는 요소 삭제뿐만 아니라 추출도 함께 수행한다.<br>
그러나 원래 리트코드의 문제에서는 삭제만 하도록 정의하고 있다.<br>
어쨌든 문제의 제약 사항인 만큼, 여기서도 문제의 조건에 따라 deQueue()는 다음과 같이 삭제만 수행하도록 구현해본다.
```python
def deQueue(self) -> bool:
    if self.q[self.p1] is None:
        return False
    else:
        self.q[self.p1] = None
        self.p1 = (self.p1 + 1) % self.maxlen
        return True
```
먼저, front 포인터 p1의 위치에 None을 넣어 삭제를 하고, 포인터를 한 칸 앞으로 이동한다.<br>
다음으로, 마찬가지로 모듈로 연산으로 최대 길이를 넘지 않도록 한다.

정리하면, enQueue()는 rear 포인터를 이동하고 데큐는 front 포인터를 이동한다. 이게 배열을 이용한 원형 큐 구현의 전부다.<br>
이제 문제에서 정의한 나머지 연산을 모두 구현해서 정리한 전체 코드는 다음과 같다.
```python
class MyCircularQueue:
    def __init__(self, k: int):
        self.q = [None] * k
        self.maxlen = k
        self.p1 = 0
        self.p2 = 0
        
    # enQueue(): rear 포인터 이동
    def enQueue(self, value: int) -> bool:
        if self.q[self.p2] is None:
            self.q[self.p2] = value
            self.p2 = (self.p2 + 1) % self.maxlen
            return True
        else:
            return False
            
    # deQueue(): front 포인터 이동
    def deQueue(self) -> bool:
        if self.q[self.p1] is None:
            return False
        else:
            self.q[self.p1] = None
            self.p1 = (self.p1 + 1) % self.maxlen
            return True
            
    def Front(self) -> int:
        return -1 if self.q[self.p1] is None else self.q[self.p1]
        
    def Rear(self) -> int:
        return -1 if self.q[self.p2 - 1] is None else self.q[self.p2 - 1]
        
    def isEmpty(self) -> bool:
        return self.p1 == self.p2 and self.q[self.p1] is None
        
    def isFull(self) -> bool:
        return self.p1 == self.p2 and self.q[self.p1] is not None
```
여기서 구현한 모든 연산은 리트코드 문제의 풀이 기준을 따랐으나, 원래 큐에는 Rear() 연산이 없다. 큐는 맨 앞에 있는 요소를 가져오는 front() 또는 peek()라는 이름으로 정의된 연산만 있기 때문이다.<br>
그러나 원형 큐를 구현하기 위해서는 2개의 포인터를 사용하는 만큼, Rear() 연산을 구현하는 일이 그리 어려운 일은 아니다. 또한 이후에 소개할 데크 등의 자료형은 애초에 ADT에서 이러한 연산을 정의하고 있기도 하다.


























