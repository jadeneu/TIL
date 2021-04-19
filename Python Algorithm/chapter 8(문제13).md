# 목차
* [chapter 8. 연결 리스트](#8장-연결-리스트)
  + [연결 리스트의 시간 복잡도](#연결-리스트의-시간-복잡도)
* [리트코드 문제](#리트코드-문제)
  + [문제 13 팰린드롬 연결 리스트](#문제-13-팰린드롬-연결-리스트)
  + [문제 13 팰린드롬 연결 리스트 풀이](#문제-13-팰린드롬-연결-리스트-풀이)
    - [풀이1. 리스트 변환](#풀이1-리스트-변환)
    - [풀이2. 데크를 이용한 최적화](#풀이2-데크를-이용한-최적화)
    - [풀이3. 고(GO)를 이용한 데크 구현](#풀이3-고go를-이용한-데크-구현)
    - [풀이4. 런너를 이용한 우아한 풀이](#풀이4-런너를-이용한-우아한-풀이)
    - [참고. 런너 기법](#참고-런너-기법)
    - [문법. 다중 할당](#문법-다중-할당)
<br><br> 





# 8장 연결 리스트
> 연결 리스트는 데이터 요소의 선형 집합으로, 데이터의 순서가 메모리에 물리적인 순서대로 저장되지는 않는다.
<br>

연결 리스트(Linked List)는 컴퓨터과학에서 배열과 함께 가장 기본이 되는 대표적인 선형 자료구조 중 하나로 다양한 추상 자료형(Abstract Data Type)(이하 ADT) 구현의 기반이 된다.<br>
동적으로 새로운 노드를 삽입하거나 삭제하기가 간편하며, 연결 구조를 통해 물리 메모리를 연속적으로 사용하지 않아도 되기 때문에 관리도 쉽다.<br>
또한 데이터를 구조체로 묶어서 포인터로 연결한다는 개념은 여러 가지 방법으로 다양하게 활용이 가능하다.<br>
실제로 컴퓨터의 물리 메모리에는 구조체 각각이 그림 8-1(199p)과 같이 서로 연결된 형태로 구성되어 있으며, 메모리 어딘가에 여기저기 흩뿌려진 형상을 띤다.

### 연결 리스트의 시간 복잡도
연결 리스트는 배열과는 달리 특정 인덱스에 접근하기 위해서는 전체를 순서대로 읽어야 하므로 상수 시간에 접근할 수 없다.<br>
즉 탐색에서는 O(n)이 소요된다.<br>
반면, 시작 또는 끝 지점에 아이템을 추가하거나 삭제, 추출하는 작업은 O(1)에 가능하다.
<br><br>

## 리트코드 문제
### 문제 13 팰린드롬 연결 리스트
> 201p

* **연결리스트에 노드값 넣기(예)**<br>
```python
# 13. 팰린드롬 연결 리스트
import time
start_time = time.time()

class ListNode:
  def __init__(self, x):
    self.val = x
    self.next = None

def init_list() -> None:
  global node_A
  node_A = ListNode('1')
  node_B = ListNode('2')
  # node_C = ListNode('2')
  # node_D = ListNode('1')
  
  node_A.next = node_B
  # node_B.next = node_C
  # node_C.next = node_D

def print_list() -> None:
  global node_A
  node = node_A

  while node:
    print(node.val)
    node = node.next
  
"""
my code is here
"""
def isPalindrome(head: ListNode) -> bool:
  pass


init_list()
print_list()

print(isPalindrome(node_A))
end_time = time.time()
print(end_time - start_time)
```
or
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

head = ListNode(1,ListNode(2, ListNode(2, ListNode(1))))
```

<br><br>

* **내가 짠 코드**<br>
연결 리스트를 잘 모르겠어서 풀이 보고 파악함
<br><br>

### 문제 13 팰린드롬 연결 리스트 풀이
#### 풀이1. 리스트 변환
팰린드롬 여부를 판별하기 위해서는 앞뒤로 모두 추출할 수 있는 자료구조가 필요하다.<br>
파이썬의 리스트는 pop() 함수에 인덱스를 지정할 수 있어 마지막 요소가 아니라도 얼마든지 원하는 위치를 자유롭게 추출할 수 있다.<br>
따라서 이 문제는 연결 리스트를 파이썬의 리스트로 변활하여 리스트의 기능을 이용하면 쉽게 풀이할 수 있을 것 같다.

연결 리스트 입력값을 파이썬의 리스트로 변환해 풀이해보자.
```python
# class ListNode:
#   def __init__(self, val=0, next=None):
#     self.val = val
#     self.next = next
def isPalindrome(self, head: ListNode) -> bool:
  q: List = []
  
  if not head:
    return True # 아무것도 없으면 팬린드롬으로 판단한다
    
  node = head
  # 리스트 변환
  while node is not None:
    q.append(node.val)
    node = node.next # 다음노드를 현재노드로 교체한다
    
  # 팰린드롬 판별
  while len(q) > 1:
    if q.pop(0) != q.pop():
      return False
      
  return True
```
이처럼 파이썬의 리스트는 q.pop(0) != q.pop() 형태로 얼마든지 인덱스를 지정해서 비교가 가능하기 때문에 이 문제는 리스트만으로도 충분히 풀이할 수 있다.

* **참고 | pop()**<br>
  + pop()은 리스트의 맨 마지막 요소를 돌려주고 그 요소는 삭제한다.
    ```python
    >>> a = [1,2,3]
    >>> a.pop()
    3
    >>> a
    [1, 2]
    ```

  + pop(x)는 리스트의 x번째 요소를 돌려주고 그 요소는 삭제한다.
    ```python
    >>> a = [1,2,3]
    >>> a.pop(1)
    2
    >>> a
    [1, 3]
    ```
<br><br>

#### 풀이2. 데크를 이용한 최적화
```python
if q.pop(0) != q.pop():
```
앞서 풀이의 문제점은 q.pop(0)에서 첫 번째 아이템을 추출할 때의 속도 문제다.<br>
동적 배열로 구성된 리스트는 맨 앞 아이템을 가져오기에 적합한 자료형이 아니다. 왜냐하면 첫 번째 값을 꺼내오면 모든 값이 한 칸씩 시프팅(Shifting)되며, 시간 복잡도 O(n)이 발생하기 때문이다.<br>
이 때문에 최적화를 위해서는 맨 앞에 데이터를 가져올 때 O(n) 이내에 처리할 수 있는 자료형이 필요하다.<br>
파이썬의 데크(Deque)는 이중 연결 리스트 구조로 양쪽 방향 모두 추출하는 데 시간 복잡도 O(1)에 실행된다.

파이썬에서 리스트를 데크로 수정하려면 딱 두 군데만 수정하면 되기 때문에 무척 쉽게 변경할 수 있다.
```python
q: Deque = collections.deque()
...
if q.popleft() != q.pop():
...
```
<br>

이를 반영한 전체 코드는 다음과 같다. 데크 자료형을 선언하고 데크에서 제공하는 메소드로 비교한 것 외에 나머지 코드는 동일하다.
```python
# class ListNode:
#   def __init__(self, val=0, next=None):
#     self.val = val
#     self.next = next
def isPalindrome(self, head: ListNode) -> bool:
  # 데크 자료형 선언
  q: Deque = collections.deque()
  
  if not head:
    return True # 아무것도 없으면 팬린드롬으로 판단한다
    
  node = head
  # 리스트 변환
  while node is not None:
    q.append(node.val)
    node = node.next # 다음노드를 현재노드로 교체한다
    
  # 팰린드롬 판별
  while len(q) > 1:
    if q.popleft() != q.pop():
      return False
      
  return True
```
수정된 부분은 두 군데에 불과하지만 데크의 명시적인 선언만으로 상당한 속도 개선 효과가 있다.<br>
앞서 풀이에서 실행 속도가 164밀리초가 걸렸는데, 이 풀이는 **72밀리초**로 약 2배 이상 빨라지는 효과가 있다.
<br>

* **참고 | popleft()**<br>
맨 왼쪽에 있는 원소를 제거하기위해 popleft()를 쓴다.
<br>

* **참고 | 연결 리스트 종류**<br>
1. 단일 연결 리스트
<img src="https://user-images.githubusercontent.com/55045377/113668487-455bed00-96ed-11eb-9819-f3cf4594b9b9.png" width=60% height=60%>

2. 이중 연결 리스트
<img src="https://user-images.githubusercontent.com/55045377/113668605-76d4b880-96ed-11eb-99a0-268eec5c2687.png" width=50% height=50%>
<br><br>

#### 풀이3. 고(GO)를 이용한 데크 구현
> 204p 참고

<br><br>

#### 풀이4. 런너를 이용한 우아한 풀이
사실 이 팰린드롬 연결 리스트 문제의 제대로 된 풀이번은 런너(Runner) 기법을 활용하는 것이다.<br>
입력값이 1->2->3->2->1인 연결 리스트에 런너를 적용해 풀이하는 방법을 도식화하면 그림 8-2(207p)와 같다.

순서에 따라 2개의 런너, 즉 빠른 런너(Fast Runner)와 느린 런너(Slow Runner)를 각각 출발시키면, 빠른 런너가 끝에 다다를 때 느린 런너는 정확히 중간 지점에 도달하게 될 것이다.<br>
느린 런너는 중간까지 이동하면서 역순으로 연결 리스트 rev를 만들어 나간다.<br>
이제 중간에 도달한 느린 런너가 나머지 경로를 이동할 때, 역순으로 만든 연결 리스트의 값들과 일치하는지 확인해나가면 된다.

빠른 런너 fast와 느린 런너 slow의 초깃값은 다음과 같이 모두 head에서 시작한다.
```python
slow = fast = head
```
<br>

이제 런너를 이동할 차례다. 다음과 같이 next가 존재하지 않을 때까지 이동한다.
```python
while fast and fast.next:
  fast = fast.next.next
  ...
  slow = slow.next
```
빠른 런너인 fast는 두 칸씩, 느린 런너 slow는 한 칸씩 이동한다.
<br>

```python
while fast and fast.next:
  fast = fast.next.next
  rev, rev.next, slow = slow, rev, slow.next
```
그러면서 역순으로 연결 리스트 rev를 생성하는 로직을 slow 앞에 덧붙인다.

첫 rev의 값은 None에서 시작하고, 런너가 이동하면서 그림 8-2(207p)에서처럼 1->None, 2->1->None로 점점 이전 값으로 연결되는 구조가 된다.
<br>
```python
rev, rev.next, slow = slow, rev, slow.next
```
다중 할당 부분을 좀 더 자세히 살펴보면, 역순 연결 리스트는 현재 값을 slow로 교체하고 rev.next는 rev가 된다.<br>
즉 앞에 계속 새로운 노드가 추가되는 형태가 된다. 결국 이 연결 리스트는 slow의 역순 연결 리스트가 될 것이다.

입력값이 홀수일 때와 짝수일 때 마지막 처리가 다른데, 홀수일 때는 slow 런너가 한 칸 더 앞으로 이동하여 중앙의 값을 빗겨 나가야 한다.<br>
왜냐하면 여기서 3은 중앙에 위치한 값으로, 팰린드롬 체크에서 배제되어야 하기 때문이다.<br>
이는 fast가 아직 None이 아니라는 경우로 간주할 수 있으며, 따라서 이 경우 다음과 같이 slow를 한 칸 더 이동해 마무리한다.<br>
그림 8-2(207p)에 이 과정이 잘 나타나 있다.
```python
if fast:
  slow = slow.next
```
<br>

이제 팰린드롬 여부를 확인할 차례다. 다음과 같이 역순으로 만든 연결 리스트 rev를 반복한다.
```python
while rev and rev.val == slow.val:
  slow, rev = slow.next, rev.next
```
slow의 나머지 이동 경로와 역순으로 만든 rev의 노드를 하나씩 풀어가면서 비교한다.

정상적으로 비교가 종료됐다면 slow와 rev가 모두 끝까지 이동해 둘 다 None이 될 것이다.<br>
따라서 최종 결과는 return not rev 또는 return not slow 모두 가능하다.<br>
전체 코드는 다음과 같다.
```python
def isPalindrome(self, head: ListNode) -> bool:
  rev = None
  slow = fast = head
  # 런너를 이용해 역순 연결 리스트 구성
  while fast and fast.next:
    fast = fast.next.next
    rev, rev.next, slow = slow, rev, slow.next
  if fast:
    slow = slow.next
    
  # 팰린드롬 여부 확인
  while rev and rev.val == slow.val:
    slow, rev = slow.next, rev.next
  return not rev
```
이 풀이는 **64밀리초**가 걸렸다.
<br><br>

#### 참고. 런너 기법
런너(Runner)는 연결 리스트를 순회할 때 2개의 포인터를 동시에 사용하는 기법이다.<br>

2개의 포인터는 각각 빠른 런너(Fast Runner), 느린 런너(Slow Runner)라고 부르는데, 대개 빠른 런너(포인터)는 두 칸씩 건너뛰고 느린 런너(포인터)는 한 칸씩 이동하게 한다.
이때 빠른 런너가 연결 리스트의 끝에 도달하면, 느린 런너는 정확히 연결 리스트의 중간 지점을 가리키게 된다.<br>
이 같은 방식으로 중간 위치를 찾아내면, 여기서부터 값을 비교하거나 뒤집기를 시도하는 등 여러모로 활용할 수 있어 연결 리스트 문제에서는 반드시 쓰이는 기법이기도 하다.

#### 문법. 다중 할당
파이썬에서 다중 할당(Multiple Assignment)은 2개 이상의 값을 2개 이상의 변수에 동시에 할당하는 것을 말한다.<br>
다음과 같이 변수 a, b에 1, 2를 동시에 할당하는 코드가 이에 해당된다.
```python
>>> a, b = 1, 2
```
a와 b 변수 각각의 값을 출력해보면 1,2가 출력됨을 확인할 수 있다.
```python
>>> a
1
>>> b
2
```
<br>

이처럼 한 번에 여러 개의 값을 여러 변수에 할당할 수 있는 파이썬의 독특한 다중 할당 문법은 매우 유용하며 여러모로 쓰임새가 많다.<br>
방금 풀어본 13번 문제 풀이4를 보면 다음과 같이 3개의 값을 할당한 바 있다.
```python
rev, rev.next, slow = slow, rev, slow.next
```
아마도 이 코드를 보면서 의문이 들었을 수도 있다. 다음과 같이 두 줄로 분기하면 훨씬 더 가독성이 높은데, 왜 궅이 한 줄로 처리해서 가독성을 떨어트렸을까?
```python
rev, rev.next = slow, rev
slow = slow.next
```
<br>

그러나 아쉽게도 이렇게는 풀리지 않는다.<br>
이렇게 늘어트릴 경우 rev, rev.next = slow, rev라는 구문에서는 **slow와 rev가 동일한 참조**가 된다.<br>
이는 C++에서의 참조와 유사하며, **구문 중간에 rev = slow가 있으니 서로 같은 값을 참조하게 되는 것**이다.

왜 = 구문이 값을 할당하는 것이 아닌 서로 같은 값을 참조하게 되는지는 파이썬의 특징에서 기인한다.<br>
파이썬에는 원시 타입이 존재하지 않는다. 대신 모든 것이 객체다. 여러 가지 자료형은 물론 문자나 숫자 또한 모두 마찬가지다.<br>
문자와 숫자의 경우 불변 객체라는 점만 다를 뿐이라서, = 연산자를 이용해 할당을 진행하게 되면 값을 할당하는 것이 아니라 이 불변 객체에 대한 참조를 할당하게 된다.

이 같은 파이썬의 독특한 할당 방식은 다음과 같은 간단한 실험으로도 확인할 수 있다.
```python
>>> id(5)
4513566608
>>> a = 5
>>> id(a)
4513566608
>>> b = 5
>>> id(b)
4513566608
```
5라는 숫자에 대해 숫자 5와 변수 a, b 모두 동일한 ID를 갖는다.<br>
즉 5라는 값은 메모리 상에 단 하나만 존재하며, a, b 두 변수는 각각 이 값을 가리키는 참조라는 의미다.<br>
만약 5가 6으로 변경된다면, a, b 두 변수도 값이 6으로 변경될 것이다. 그러나 이런 일은 일어나지 않는다. 앞서 설명한 것처럼 숫자는 불변 객체이기 때문이다.

반면 숫자가 아니라 리스트와 같은 자료형이라면, 내부의 값은 얼마든지 변할 수 있다. 이 경우 이 리스트를 참조하는 모든 변수의 값도 따라서 함께 바뀌게 된다.

이번에는 문제 풀이에서처럼 rev = 1, slow = 2->3이라고 가정해보자. 여기서 slow는 연결 리스트이며, slow.next는 3이라는 의미다.
```python
rev, rev.next, slow = slow, rev, slow.next
```
이 경우 rev = 2->3, rev.next = 1, slow = 3이 되고, rev.next = 1이므로 **최종적으로 rev = 2->1, slow = 3이 된다.**

다중 할당을 하게 되면 이 같은 작업이 동시에 일어나기 때문에, 이 모든 작업은 중간 과정 없이 한 번의 트랜잭션으로 끝나게 된다.<br>
그러나 앞서 살펴본 두 줄 분기 코드인 다음과 같이 나눠서 처리하는 경우를 생각해보자.
```python
rev, rev.next = slow, rev
slow = slow.next
```
첫 줄을 실행한 결과, rev = 2->3, rev.next = 1 따라서 rev = 2->1이 되는데 여기서 중요한 점은 rev = slow라는 점이다.<br>
즉 동일한 참조가 되었으며 rev = 2->1이 되었기 때문에 slow = 2->1도 함께 되어 버린다.<br>
따라서 이후에 slow = slow.next의 결과는 slow = 1이 된다.<br>
결국, **최종 결과는 rev = 2->1, slow = 1로**, 앞서 다중 할당으로 한 번에 처리한 것과 다른 결과가 된다.<br>
따라서 앞서 풀이의 경우, 반드시 한 줄의 다중 할당으로 한 번에 처리해야 문제를 제대로 풀이할 수 있다.
<br><br>




















