# 목차
* [chapter 8. 연결 리스트](#8장-연결-리스트)
* [리트코드 문제](#리트코드-문제)
  + [문제 14 두 정렬 리스트의 병합](#문제-14-두-정렬-리스트의-병합)
  + [문제 14 두 정렬 리스트의 병합 풀이](#문제-14-두-정렬-리스트의-병합-풀이)
    - [풀이1. 재귀 구조로 연결](#풀이1-재귀-구조로-연결)
    - [파이썬. 연산자 우선순위](#파이썬-연산자-우선순위)
    - [문법. 변수 스왑](#문법-변수-스왑)
  + [문제 15 역순 연결 리스트](#문제-15-역순-연결-리스트)
  + [문제 15 역순 연결 리스트 풀이](#문제-15-역순-연결-리스트-풀이)
    - [풀이1. 재귀 구조로 뒤집기](#풀이1-재귀-구조로-뒤집기)
    - [풀이2. 반복 구조로 뒤집기](#풀이2-반복-구조로-뒤집기)
  + [문제 16 두 수의 덧셈](#문제-16-두-수의-덧셈)
  + [문제 16 두 수의 덧셈 풀이](#문제-16-두-수의-덧셈-풀이)
    - [풀이1. 자료형 변환](#풀이1-자료형-변환)
    - [풀이2. 전가산기 구현](#풀이2-전가산기-구현)
    - [문법. 숫자형 리스트를 단일 값으로 병합하기](#문법-숫자형-리스트를-단일-값으로-병합하기)
<br><br> 




# 8장 연결 리스트
## 리트코드 문제
### 문제 14 두 정렬 리스트의 병합
> 213p

* **내가 짠 코드**<br>
```python

```
<br><br>

### 문제 14 두 정렬 리스트의 병합 풀이
#### 풀이1. 재귀 구조로 연결
여기서는, 정렬된 리스트라는 점이 중요하다. 즉 병합 정렬에서 마지막 조합 시 첫 번째 값부터 차례대로만 비교하면 한 번에 해결되듯이, 이 또한 병합 정렬의 마지막 조합과 동일한 방식으로 첫 번째부터 비교하면서 리턴하면 쉽게 풀 수 있는 문제다.<br>
먼저 전체 코드부터 살펴보자.
```python
def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
  if (not l1) or (12 and l1.val > l2.val):
    l1, l2 = l2, l1
  if l1:
    l1.next = self.mergeTwoLists(l1.next, l2)
  return l1
```
하나씩 풀어서 살펴보자. 먼저 l1과 l2의 값을 비교해 작은 값이 왼쪽에 오게 하고, next는 그다음 값이 엮이도록 재귀 호출하는 게 이 전체 코드의 전부다. 

여기서 첫 번째 if 문인 괄호로 엮인 부분과 변수를 스왑(Swap)하는 부분부터 살펴보자.
```python
if (not l1) or (12 and l1.val > l2.val):
    l1, l2 = l2, l1
```
사실 이 if 구문의 경우 괄호를 생략해도 동일한 순서로 실행된다. 
```python
if not l1 or 12 and l1.val > l2.val:
```
이 중에서 가장 우선순위가 높은 것은 비교 연산 >이다. 다음으로 not l1이다. 그다음은 and, 다음은 or가 된다.<br>
이에 따라 좀 더 명확하게 괄호로 구분해보면 다음과 같다.
```python
if (not l1) or (12 and (l1.val > l2.val)):
```
이제 이 코드를 우리가 풀이했던 코드와 다시 한번 비교해보면, 비교 연산 >에 대한 괄호만 생략했을 뿐 실제 우리가 의도한 순서와 동일하다. 따라서 이 코드는 괄호를 모두 제거해도 우리가 의도한 순서대로 실행된다.<br>
하지만 괄호가 없으면 어느 것이 먼저 실행되는지 상당히 헷갈린다.<br>
특히 앞의 코드에서 or보다 and가 먼저 실행된다는 점은 미리 공부하지 않았다면 알기가 어렵다.<br>
코딩 테스트 시에는 사소한 실행 순서 문제로도 트러블에 빠지게 되면 해결이 매우 어려워지므로 명확하게 괄호로 처리하는 편이 훨씬 낫다.

이제 다음과 같이 변수를 스왑한다.
```python
l1, l2 = l2, l1
```
이후에는 l1의 next를 재귀호출하면서 다음번 연결 리스트가 계속 스왑될 수 있게 한다.

이제 전체를 실행하면 연결 리스트가 어떻게 변경되는지, 다음 그림 8-4(215p)에서 살펴보자.<br>
먼저, 이 그림에서 **L1과 L2는 두 연결 리스트의 첫 번째 값을 각각 가리킨다.** (그림에서 L1과 L2는 코드에서는 변수 l1과 l2)

앞서 코드 풀이에서 설명했던 것처럼, 스왑하면서 그다음 값이 엮이도록 계속 재귀호출하면, 그림 8-4(215p)의 순서대로 연결 리스트가 점점 하나로 병합되면서 엮이게 된다.<br>
마지막에는 L1이 널(Null)이 되면서, 즉 코드에서는 l1이 None이 되면서 재귀가 끝나고 리턴을 시작한다.<br>
실제로는 이처럼 마지막에 리턴을 시작하면 백트래킹되면서 엮이게 된다. 백트래킹이 종료되면 이제 두 정렬 리스트가 병합되어 하나의 연결 리스트가 된다.
<br><br>

#### 파이썬. 연산자 우선순위
216p 참고

#### 문법. 변수 스왑
두 변수를 스왑(Swap)하는 가장 일반적이고 널리 사용되는 방법은 다음과 같이 임시 변수를 사용하는 방법이다.
```python
temp = a
a = b
b = temp
```
이 방식은 거의 모든 언어에서 활용할 수 있는 가장 기본적인 방식이다.<br>
그러나 앞서 풀이에서는 임시 변수 없이 a, b = b, a로 바로 스왑했다. 변수 선언을 포함하면 다음과 같은 코드가 된다.
```python
a: int = 1
b: int = 2

a, b = b, a
```
이 방식은 파이썬에서 지원하는 매우 강력한 기능 중 하나로서 다중 할당이라 불리우며, 가독성 또한 좋으므로 별다른 이슈가 없는 한 파이썬에서는 이 방식으로 스왑하는 게 가장 좋다.

숫자형인 경우, 간단한 덧셈, 뺄셈을 통해 임시 변수 없이 스왑할 수 있는 방법이 있다. 간단한 수학문제로 볼 수 있는데, 다음과 같이 가능하다.
```python
x += y
y = x - y
x -= y
```
이 방식은 추가 공간을 필요로 하지 않는다는 점에서 공간 복잡도 또한 필요하지 않은 좋은 풀이다.
<br><br>

### 문제 15 역순 연결 리스트
> 219p

* **내가 짠 코드**<br>
```python

```


<br><br>

### 문제 15 역순 연결 리스트 풀이
#### 풀이1. 재귀 구조로 뒤집기
연결 리스트를 뒤집는 문제는 매우 일반적이면서도 활용도가 높은 문제로, 실무에서도 빈번하게 쓰인다.<br>
**재귀 구조**와 **반복 구조**, 2가지 방식으로 모두 풀이해보자.<br>
먼저, 재귀 풀이는 다음과 같다.
```python
def reverseList(self, head: ListNode) -> ListNode:
  def reverse(node: ListNode, prev: ListNode = None):
    if not node:
      return prev
    next, node.next = node.next, prev
    return reverse(next, node)
    
  return reverse(head)
```
<br>

예시)
```python
class ListNode:
  def __init__(self, val=0, next=None):
    self.val = val
    self.next = next
    
  def _print_all(self):
    while self:
      print(self.val, end=' ')
      self = self.next
    print()
    
class Solution:
  def reverse(self, current: ListNode, prev: ListNode) -> ListNode:
    if not current:
      return prev
      
    next, current.next = current.next, prev
    return self.reverse(next, current)
    
  def reverseList(self, head: ListNode) -> ListNode:
    return self.reverse(head, None)
```
```
solution = Solution()
L1_3 = ListNode(4)
L1_2 = ListNode(2,L1_3)
L1_1 = ListNode(1,L1_2)

print("Before : ", end=" ")
L1_1._print_all()
result = solution.reverseList(L1_1)
print("After : ", end=" ")
result._print_all()
```
출력 결과 ↓
```
Before : 1 2 4
After : 4 2 1
```
<br>

다음 노드 next와 현재 노드 node를 파라미터로 지정한 함수를 계속해서 재귀 호출한다.<br>
node.next에는 이전 prev 리스트를 계속 연결해주면서 node가 None이 될 때까지 재귀 호출하면 마지막에는 백트래킹되면서 연결 리스트가 거꾸로 연결된다.<br>
여기서 맨 처음에 리턴된 prev는 뒤집힌 연결 리스트의 첫 번째 노드가 된다.
<br><br>

#### 풀이2. 반복 구조로 뒤집기
다음으로 반복 풀이를 살펴보자.
```python
def reverseList(self, head: ListNode) -> ListNode:
  node, prev = head, None
  
  while node:
    next, node.next = node.next, prev
    prev, node - node, next
    
  return prev
```
마찬가지로, node.next를 이전 prev 리스트로 계속 연결하면서 끝날 때까지 반복한다.<br>
node가 None이 될 때 prev는 뒤집힌 연결 리스트의 첫 번째 노드가 된다.<br>
next, node.next = node.next, prev로 다중 할당하는 부분은 재귀나 반복 양쪽 모두 동일하다.<br>
반복 풀이의 경우 prev에 node를, node에 next를 별도로 셋팅하며, 이를 이용해 node가 None이 될 때까지 계속 while 반복문을 돌게 된다.
<br><br>

### 문제 16 두 수의 덧셈
> 221p

* **내가 짠 코드**<br>
```python

```


<br><br>

### 문제 16 두 수의 덧셈 풀이
#### 풀이1. 자료형 변환
얼핏 드는 생각은, 연결 리스트를 문자열로 이어 붙인 다음에 숫자로 변환하고, 이를 모두 계산한 후 다시 연결 리스트로 바꾸면 쉽게 풀이할 수 있을 것 같다. 물론 이 작업에 얼마나 많은 시간이 걸릴지는 잘 모르겠다.<br>
그렇다면 한번 그 방식대로 풀이를 시도해보자.

먼저, 역순으로 된 연결 리스트를 뒤집어야 한다.<br>
(반복 풀이 방식) ↓
```python
def reverseList(self, head: ListNode) -> ListNode:
  node, prev = head, None
  
  while node:
    next, node.next = node.next, prev
    prev, node = node, next
    
  return prev
```
바로 이전 문제에서 풀이했던 연결 리스트를 뒤집는 함수를 다시 동일하게 가져왔다.

이번에는 덧셈 연산을 위해 연결 리스트를 파이썬의 리스트로 변경해야 한다.
```python
def toList(self, node: ListNode) -> List:
  list: List = []
  while node:
    list.append(node.val)
    node = node.next
  return list
```
이 코드는 node를 반복하며 값을 리스트에 추가하는 함수다.

반대로 리스트를 연결 리스트로 변경하는 함수도 작성해보자.
```python
def toReversedLinkedList(self, result: str) -> ListNode:
  prev: ListNode = None
  for r in result:
    node = ListNode(r)
    node.next = prev
    prev = node
    
  return node
```
이 코드는 순서대로 엮어 나가는 방식으로 구현했는데, 이렇게 하면 뒤집힌 연결 리스트가 된다.<br>
이 문제에서는 출력 또한 역순으로 하도록 문제에서 요구하고 있으므로 이 상태로 그대로 내보내면 될 것 같다.<br>
만약 뒤집는 작업이 필요한 경우, 마찬가지로 이전 풀이의 함수를 사용하면 어렵지 않게 수행할 수 있다.

이제 실제로 덧셈 풀이를 작성해보자.
```python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
  a = self.toList(self.reverseList(l1))
  b = self.toList(self.reverseList(l2))
```
이 코드에서 a와 b는 어떤 값이 될지 예상이 되는가? 먼저 연결 리스트를 뒤집었고, 이를 다시 파이썬 리스트로 변환했다.<br>
따라서 a와 b는 각각 뒤집힌 연결 리스트의 파이썬 리스트가 될 것이다.

이제 덧셈 연산을 하면 되는데 이를 위해서는 리스트를 int 형태로 결합해야 한다.
```python
resultStr = int(''.join(str(e) for e in a)) +
            int(''.join(str(e) for e in b))
```
현재 이 리스트는 문자열이 아닌 숫자형 리스트며, 따라서 이를 합치기 위해서는 문자형으로 먼저 변경이 필요하다.<br>
여기서는 str(e)로 각 항목을 문자로 변경한 다음 join()으로 합쳤다. 그런데 덧셈 연산을 위해서는 다시 숫자형이 되어야 한다.<br>
따라서 최종 값은 int로 다시 변경해줘야 한다.

마지막으로, 최종 계산 결과를 연결 리스트로 바꿔보자.<br>
앞서 만들어둔 toReversedLinkedList()를 사용하면 쉽게 변환할 수 있을 것 같다.<br>
단 여기서는 문자열을 입력값으로 받기 때문에 다음과 같이 str()로 한 번 변환하는 작업은 필요하다.
```python
return self.roReversedLinkedList(str(resultStr))
```
<br>

이제 모두 통합하면 전체 코드는 다음과 같다.
```python
class Solution:
  # 연결 리스트 뒤집기
  def reverseList(self, head: ListNode) -> ListNode:
    node, prev = head, None

    while node:
      next, node.next = node.next, prev
      prev, node = node, next

    return prev
    
  # 연결 리스트를 파이썬 리스트로 변환
  def toList(self, node: ListNode) -> List:
    list: List = []
    while node:
      list.append(node.val)
      node = node.next
    return list
    
  # 파이썬 리스트를 연결 리스트로 변환
  def toReversedLinkedList(self, result: str) -> ListNode:
    prev: ListNode = None
    for r in result:
      node = ListNode(r)
      node.next = prev
      prev = node

    return node
    
  # 두 연결 리스트의 덧셈
  def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
    a = self.toList(self.reverseList(l1))
    b = self.toList(self.reverseList(l2))
    
    resultStr = int(''.join(str(e) for e in a)) + \
                int(''.join(str(e) for e in b))
                
    # 최종 계산 결과 연결 리스트 변환
    return self.roReversedLinkedList(str(resultStr))
```
코드가 다소 길지만 풀이 자체는 전혀 어렵지 않다. 수행 속도에도 아무런 문제가 없다.<br>
그러나 이 같은 방식이 깔끔한 풀이라고 할 수도 없다. 좀 더 우아한 방식으로 풀이할 수 있는 방법을 찾아보자.
<br><br>

#### 풀이2. 전가산기 구현
여기서는 논리 회로의 전가산기(Full Adder)와 유사한 형태를 구현해보자.<br>
이진법이 아니라 십진법이라는 차이만 있을 뿐, 자리올림수(Carry)를 구하는 것까지 가산기의 원리와 거의 동일하다.<br>
전가산기의 진리표와 회로도는 다음 그림 8-5(225p)와 같다.

입력값 A와 B, 이전의 자리올림수(Carry in) 이렇게 3가지 입력으로 합(Sum)과 다음 자리올림수(Carry out) 여부를 결정한다.<br>
그림 8-5(225p)에서 우측 그림은 전가산기 회로도를 나타낸다.<br>
입력값 A, B는 오른쪽으로는 XOR 게이트를 통과한 결과가 나아가고, 아래로는 AND 게이트를 통과한 결과가 나아간다. 그렇게 합과 다음 자리올림수 위치에 도달한 결과가 그림 좌측의 진리표다.<br>
실제로 덧셈 연산을 수행하는 논리 회로의 원리며, 산술 논리 장치를 구성하는 디지털 회로의 일부분이다. 산술 논리 장치는 컴퓨터의 중앙 처리 장치, 그러니까 우리가 잘 아는 CPU의 한 부분이기도 하다.<br>
* **참고**<br>
  + Sum은 A와 B의 XOR을 한 뒤에 다시 Carry in과 XOR을 한 값이다. 왜냐하면 XOR은 두 개가 다를시 1을 반환하기 때문에 결국 더했을 때 그 자리에 올 수와 같다. 즉 A, B를 XOR하고 그 값을 Carry in과 XOR을 하면 sum(그자리에 올 0 or 1)값이 된다.

  + Carry out은 A와 B가 둘다 1이어서 밑의 AND 연산으로 1이되고, 그 밑의 or 연산으로 1이되어 올림수가 발생하는 경우와 또 다른 하나는 A와 B의 XOR 연산 즉 둘 중 하나만 1이어서 XOR로 1이 되고, Carry in이 1이되어서 그 밑의 AND연산으로 1, 그 밑의 or 연산으로 최종 1이 반환되어 올림수가 발생하는 경우 이렇게 2가지이다.
<br>

여기서는 연산 결과로 나머지를 취하고 몫은 자리올림수 형태로 올리는 전가산기의 전체적인 구조만 참고해 풀이해본다.
<br>

```python
sum = 0
if l1:
  sum += li.val
  l1 = li.next
if l2:
  sum += l2.val
  l2 = l2.next
```
이 코드에서 먼저, 두 입력값의 합을 구한다. l1, l2는 그럼 8-5(225p)의 오른쪽 전가산기 회로도에서 A, B의 역할과 동일하다.<br>
두 입력값의 연산을 수행하고 다음과 같이 자릿수가 넘어갈 경우에는 자리올림수를 설정할 것이다.
```python
carry, val = divmod(sum + carry, 10)
```
이처럼 자리올림수를 계산하게 되는데, 전가산기와 마찬가지로 이전의 자리올림수 carry를 받아오고, 이를 divmod()로 계산한다.<br>
divmod()는 파이썬의 내장 함수로, 몫과 나머지로 구성된 튜플을 리턴한다. 즉 다음과 같이 (a // b, a % b)와 동일한 결과를 출력한다.
```python
>>> divmod(10, 3)
(3, 1)
>>> (10 // 3, 10 % 3)
(3, 1)
```
<br>

다시 문제로 돌아와 carry, val = divmod(sum + carry, 10)을 하게 되면, 가산 결과가 두 자릿수가 될 경우 몫은 자리올림수로 설정해 다음번 연산에 사용되게 하고 나머지는 값으로 취한다.<br>
이제 이 값을 연결 리스트로 만들어 주면 된다.<br>
원래 가산기는 논리 회로를 이용해 이진 연산을 수행하지만 여기서는 전체적인 구조만 참고하여 십진 연산에 응용해봤다.<br>
전체 코드는 다음과 같다.
```python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
  root = head = ListNode(0)
  
  carry = 0
  while l1 or 12 or carry
    sum = 0
    # 두 입력값의 합 계산
    if l1:
      sum += li.val
      l1 = li.next
    if l2:
      sum += l2.val
      l2 = l2.next
      
    # 몫(자리올림수)과 나머지(값) 계산
    carry, val = divmod(sum + carry, 10)
    head.next = ListNode(val)
    head = head.next
    
  return root.next
```
자료형을 일일이 변환하던 풀이에 비해, 코드가 한결 깔끔하며 풀이가 우아하다. 성능 또한 비슷하다.
<br><br>

#### 문법. 숫자형 리스트를 단일 값으로 병합하기
앞서 문제의 풀이1에서 최종 결과를 리턴하는 과정에서 애초에 숫자형 리스트를 문자형으로 바꿨다가 숫자형으로 다시 한번 바꿔주는 불필요한 작업을 진행했다. 실제로 이 같은 코드는 좋은 코드라고 할 수 없다.<br>
그렇다면 a = [1, 2, 3, 4, 5] 처럼 숫자형으로 이루어진 리스트가 있을 때 이를 하나로 합치는 좀 더 우아한 코드를 만들어 볼 수 있을까?
```python
>>> ''.join(str(e) for e in a)
'12345'
```
이 방식의 코드는 아무래도 가독성이 떨어지고 영 보기가 좋지 않다. 좀 더 깔끔한 방법은 없을까?

다음과 같은 방식을 시도해볼 수 있을 것 같다.
```python
>>> ''.join(map(str, a))
'12345'
```
이 경우 임시 변수 e를 사용하지 않아 깔끔하며, map(str,...로 이어지는 부분이 문자열로 변환을 암시하는 듯하여 가독성도 좋다.

그렇다면 이전 풀이에서 처리했던 코드를 방금 살펴본 map()을 이용한 코드로 바꿔보자.
```python
# 1
resultStr = int(''.join(str(e) for e in a)) + \
            int(''.join(str(e) for e in b))
            
# 2
resultStr = int(''.join(map(str, a))) + \
            int(''.join(map(str, b)))
```
이 코드에서 #1이 이전 풀이에서 처리했던 코드, #2가 방금 살펴본 map()을 이용한 코드다. 실제로 바꿔보니 #2가 한결 더 깔끔함을 느낄 수 있다.<br>
그러나 이 방식도 원래는 숫자형인데 문자형으로 바꿨다가 다시 숫자형으로 바꾼다는 부분이 영 마음에 들지 않는다.<br>
애초에 숫자형으로 바로 변경할 수는 없을까?
```python
>>> functools.reduce(lambda x, y: 10 * x + y, a, 0)
12345
```
이런 식으로 처리할 수 있을 것 같다.<br>
functools 는 '함수를 다루는 함수'를 뜻하는 고계 함수(Higher-Order Function)를 지원하는 함수형 언어 모듈이며, 리트코드에서 기본적으로 import되어 있다.<br>
여기서 reduce는 두 인수의 함수를 누적 적용하라는 메소드다. 즉 다음의 코드를 살펴보자.
```python
>>> functools.reduce(lambda x, y: x + y, [1, 2, 3, 4, 5])
15
```
이 코드의 결과는 ((((1 + 2)+3)+4)+5)이다. 그러니까 15가 된다.<br>
간단한 예시라 누적 적용한 결과가 그냥 합한 것과 다르지 않지만, 어떤 원리인지는 쉽게 이해될 것이다.

다시 앞서 코드 functools.reduce(lambda x, y: 10 * x + y, a, 0)의  결과를 살펴보면, 값 x에 계속 10을 곱하면서 자릿수 10^n 형태로 올려나가고 그 뒤에 y를 더해서 10^0 자릿수를 채워나가는 방식이다.

이외에도 좀 더 가독성을 높일 수 있도록 operator 모듈을 활용하는 방법도 있다.<br>
이 경우 연산자 명칭(함수)을 reduce() 메소드의 파라미터로 지정할 수 있어 가독성이 매우 높다.<br>
이 또한 고계 함수이기 때문에 이처럼 파라미터로 함수를 넘기는 것이 가능하다.
```python
>>> from operator import add, mul
>>> functools.reduce(add, [1, 2, 3, 4, 5])
15
>>> functools.reduce(mul, [1, 2, 3, 4, 5])
120
```















