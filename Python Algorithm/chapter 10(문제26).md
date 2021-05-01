# 목차
* [chapter 10. 데크, 우선순위 큐](#10장-데크-우선순위-큐)
  + [데크](#데크)
* [리트코드](#리트코드)
  + [문제 26 원형 데크 디자인](#문제-26-원형-데크-디자인)
  + [문제 26 원형 데크 디자인 풀이](#문제-26-원형-데크-디자인-풀이)
    - [풀이1. 이중 연결 리스트를 이용한 데크 구현](#풀이1-이중-연결-리스트를-이용한-데크-구현)
<br><br>


# 10장 데크, 우선순위 큐
10장에서는 스택과 큐의 연산을 모두 갖고 있는 복합 자료형인 **데크**와 추출 순서가 일정하게 정해져 있지 않은 **우선순위 큐**에 대해 살펴보자.
<br><br>
## 데크
이전 장에서 원형 큐를 구현하면서 Front()와 Rear()를 모두 구현한 바 있다.

사실 Front()는 큐의 연산이 맞지만 Rear()는 큐에 정의되어 있지 않은 연산이라는 점도 얘기한 바 있다. 이 연산은 데크에 존재하는 연산으로, 데크의 정의는 다음과 같다.

> 데크(Deque)는 더블 엔디드 큐(Double-Ended Queue)의 줄임말로, 글자 그대로 양쪽 끝을 모두 추출할 수 있는, 큐를 일반화한 형태의 추상 자료형(ADT)이다.

<br>

데크는 양쪽에서 삭제와 삽입을 모두 처리할 수 있으며, 스택과 큐의 특징을 모두 갖고 있다.<br>
이 추상 자료형(ADT)의 구현은 배열이나 연결리스트 모두 가능하지만, 특별히 다음과 같이 이중 연결 리스트(Doubly Linked List)로 구현하는 편이 가장 잘 어울린다.

> 그림 10-1

이중 연결 리스트로 구현하게 되면, 그림 10-1(266p)처럼 양쪽으로 head와 tail이라는 이름의 두 포인터를 갖고 있다가 새로운 아이템이 추가될 때마다 앞쪽 또는 뒤쪽으로 연결시켜 주기만 하면 된다. 당연히 연결 후에는 포인터를 이동하면 된다.

파이썬은 데크 자료형을 다음과 같이 collections 모듈에서 deque라는 이름으로 지원한다.
```python
>>> import collections
>>> d = collections.deque()
>>> type(d)
<class 'collections.deque'>
```
이 collections.deque는 그림 10-1(266p)과 마찬가지로 이중 연결 리스트로 구현되어 있다.

CPython에서는 고정 길이 하위 배열(Subarray)을 지닌 이중 연결 리스트로 구현되어 있으며, 내부 구현을 살펴보면 마친가지로 다음과 같이 구조체인 dequeobject가 block노드의 이중 연결 리스트로 구현되어 있는 것을 확인할 수 있다.<br>
또한 dequeobject는 왼쪽, 오른쪽 인덱스 정보와 최대 길이 등 여러 가지 부가 정보를 함께 보관하고 있는 풍부한 구조체임을 확인할 수 있다.
```c
// cpython/Modules/_collectionsmodule.c
typedef struct BLOCK {
    struct BLOCK *leftlink;
    PyObject *data[BLOCKLEN];
    struct BLOCK *rightlink;
} block;

typedef struct {
    ...
    block *leftblock;
    block *rightblock;
    Py_ssize_t leftindex;
    Py_ssize_t rightindex;
    Py_ssize_t maxlen;
    ...
} dequeobject;
```
<br>

이번에는 이중 연결 리스트를 이용해 데크 자료형을 직접 구현하는 문제를 한번 풀이해보자.
<br><br>

## 리트코드
### 문제 26 원형 데크 디자인
> 268p
* **내가 짠 코드**<br>
-> 이중 연결 리스트를 구현하는 방법을 몰라서 코드 구현 못했음..
<br><br>

### 문제 26 원형 데크 디자인 풀이
#### 풀이1. 이중 연결 리스트를 이용한 데크 구현
이 문제는 원형 데크(Circular Deque)를 디자인하는 문제다. 데크란 앞서 설명에서 언급했듯이 양쪽 끝을 모두 추출할 수 있는 큐를 말한다.

25번 문제에서는 원형 큐를 배열로 구현했으므로, 이번에는 실제 파이썬의 데크 구현체이기도 한 이중 연결 리스트로 구현해보자.<br>
또한 이 데크 구현 문제의 경우 원형 큐 구현 문제와 달리, 맨 앞에 노드를 추가하는 insertFront() 연산도 있다.<br>
일반적인 배열로는 맨 앞에 요소를 추가하는 작업는 시간 복잡도가 O(n)이기 때문에 구현이 쉽지 않다. 그러나 연결 리스트는 맨 앞에 노드를 추가하는 작업이 그리 어렵지 않다.

먼저 다음과 같이 초기화 함수 __ init__()을 정의하자.
```python
def __init__(self, k: int):
    self.head, self.tail = ListNode(None), ListNode(None)
    self.k, self.len = k, 0
    self.head.right, self.tail.left = self.tail, self.head
```
우리가 구현하려는 데크는 CPython의 구현과 유사하게 왼쪽, 오른쪽 인덱스 역할을 하는 head, tail을 정의하고, 최대 길이 정보를 k로 설정한다.<br>
여기에 추가로, 현재 길이 정보를 담는 변수가 될 len을 self.len = 0으로 따로 정의해둔다.

다음은 앞쪽에 노드를 추가하는 연산 insertFront()를 구현해보자.
```python
def insertFront(self, value: int) -> bool:
    if self.len == self.k:
        return False
    self.len += 1
    self._add(self.head, ListNode(value))
    return True
```
새로운 노드 삽입시 최대 길이에 도달했을 때는 False를 리턴하고, 이외에는 _ add() 메소드를 이용해 head 위치에 노드를 삽입한다. 

다음은 뒤쪽에 노드를 추가하는 연산이다.
```python
def insertLast(self, value: int) -> bool:
    ...
    self._add(self.tail.left, ListNode(value))
    return True
```
뒤쪽에 추가하는 연산 insertLast()도 마찬가지다. 한 가지 다른 점은 head가 아닌 tail.left에 삽입한다는 점이다.

이제 실제 삽입을 수행하는 내부 함수를 구현해보자. 내부에서만 사용한다는 의미로 PEP 8 명명 규칙 기준에 따라 밑줄( _ ) 하나로 시작하도록 메소드 명을 다음과 같이 _ add()로 정했다.
```python
def _add(self, node: ListNode, new: ListNode):
    n = node.right
    node.right = new
    new.left, new.right = node, n
    n.left = new
```
이중 연결 리스트의 삽입 메소드인 _ add()는 이 코드에서처럼 여러 단계의 다소 복잡한 구현 과정을 거친다. 이를 도식화해보면 그림 10-2(270p)와 같다.

> 그림 10-2

이중 연결 리스트에 신규 노드를 삽입하는 _ add() 메소드는 이 그림과 같이 이미 있는 노드를 찢어내고 그 사이에 새로운 노드 new를 삽입하는 형태가 된다. 파란색으로 표기한 번호 순서대로 실행이 된다.

삭제 또한 크게 다르지 않다. 앞쪽 삭제는 head의 노드를, 뒤쪽 삭제는 tail 위치의 노드를 처리한다.<br>
뒤쪽의 경우 다음 코드와 같이, 좀 더 정확히는 tail.left.left를 _ del() 메소드로 처리하게 된다.
```python
def deleteFront(self) -> bool:
    ...
    self.len -= 1
    self._del(self.head)
    
def deleteLast(self) -> bool:
    ...
    self.len -= 1
    self._del(self.tail.left.left)
```
이처럼 이중 연결 리스트의 삭제는 삽입보다는 비교적 간단한 편이다.

이제 전체 코드를 정리해보면 다음과 같다.
```python
class MyCircularDeque:
    def __init__(self, k: int):
        self.head, self.tail = ListNode(None), ListNode(None)
        self.k, self.len = k, 0
        self.head.right, self.tail.left = self.tail, self.head
        
    # 이중 연결 리스트에 신규 노드 삽입
    def _add(self, node: ListNode, new: ListNode):
        n = node.right
        node.right = new
        new.left, new.right = node, n
        n.left = new
        
    def _del(self, node: ListNode):
        n = node.right.right
        node.right = n
        n.left = node
        
    def insertFront(self, value: int) -> bool:
        if self.len == self.k:
            return False
        self.len += 1
        self._add(self.head, ListNode(value))
        return True
        
    def insertLast(self, value: int) -> bool:
        if self.len == self.k:
            return False
        self.len += 1
        self._add(self.tail.left, ListNode(value))
        return True
        
    def deleteFront(self) -> bool:
        if self.len == 0:
            return False
        self.len -= 1
        self._del(self.head)
        return True

    def deleteLast(self) -> bool:
        if self.len == 0:
            return False
        self.len -= 1
        self._del(self.tail.left.left)
        return True
        
    def getFront(self) -> int:
        return self.head.right.val if self.len else -1
        
    def getRear(self) -> int:
        return self.tail.left.val if self.len else -1
        
    def isEmpty(self) -> bool:
        return self.len == 0
    
    def isFull(self) -> bool:
        return self.len == self.k
```
코드가 길어서 얼핏 어려워 보이지만 연산의 종류가 많을 뿐, 연결 리스트로 원형 데크를 구현하는 일은 그리 어렵지 않다.

앞서 9장에서는 원형 큐를 배열로 구현해봤고, 이번에는 원형 데크를 연결 리스트로 구현해봤다.<br>
그런데 사실 원형 데크를 이중 연결 리스트로 구현하게 되면 원형의 이점을 전혀 살릴 수 없게 된다.<br>
이 문제는 데크를 소개하면서 이중 연결 리스트로 구현이 가능하다는 것을 보여주기 위한 구현일 뿐, 실제로 원형의 이점을 살리기 위해서라면 원래 이 문제는 연결 리스트가 아닌 배열로 풀이해야 한다.

원형으로 구현하는 이유는 뒤쪽으로 요소를 채우다가 공간이 다 차게되면 tail과 head를 연결해 앞쪽의 빈 공간을 활용하려는 의도인데, 연결 리스트는 애초에 빈 공간이라는 개념이 존재하지 않기 때문에 우너형은 아무런 의미가 없다.<br>
또한 데크의 연산은 맨 처음과 맨 끝의 값을 추출할 뿐이며 맨 끝의 다음 값을 추출하는지 등의 연산은 존재하지 않기 때문에, 서로 연결되어 있을 필요 또한 없다.<br>
이 구현의 경우 사실상 아무런 의미 없이 tail과 head가 서로 이중 연결 리스트로 연결되어 있으며, 단순히 풀이를 위해 구현되어 있는 셈이다.
<br><br>
























