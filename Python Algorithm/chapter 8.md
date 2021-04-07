# 8장 연결 리스트
## 리트코드 문제
### 문제 17 페어의 노드 스왑
> 229p

* **내가 짠 코드**<br>

<br><br>

### 문제 17 페어의 노드 스왑 풀이
#### 풀이1. 값만 교환
매우 직관적인 방법으로 풀이해보자. <br>
연결 리스트의 노드를 변경하는 게 아닌, 노드 구조는 그대로 유지하되 값만 변경하는 방법이다.<br>
대개 연결 리스트는 복잡한 여러 가지 값들의 구조체로 구성되어 있고, 사실상 값만 바꾸는 것은 매우 어려운 일이다. 그러나 이 문제에서는 단 하나의 값으로 구성된 단순한 연결 리스트이고, 값을 바꾸는 정도는 어렵지 않게 가능하다.
```python
def swapPairs(self, head: ListNode) -> ListNode:
  cur = head
  
  while cur and cur.next:
    # 값만 교환
    cur.val, cur.next.val = cur.next.val, cur.val
    cur - cur.next.next
    
  return head
```
이처럼 연결 리스트의 값 .val을 변경하는 정도로 쉽게 풀이가 가능하다.

면접 시 화이트보드에 이 같은 방식을 기술한다면, 면접관에게 "쉽게 풀기 위해 이렇게 변칙적으로 풀이한다."는 식의 충분한 설명이 필요하다.
<br><br>

#### 풀이2. 반복 구조로 스왑
단순히 값을 바꾸는 일에 비해 연결 리스트 자체를 바꾸는 일은 생각보다 다소 복잡한 문제다.<br>
왜냐하면 1->2->3->4->5->6인 연결 리스트가 있을 경우, 3->4를 4->3으로 바꾼다고 할 때 단순리 둘만 바꾼다고 끝나는 문제가 아니라, 그 앞인 2가 가리키는 연결 리스트와 5로 연결되는 연결 리스트도 다 같이 변경해야 하기 때문이다.
```python
b = a.next
a.next = b.next
b.next = a
```
이처럼 b가 a를 가리키고 a는 b의 다음을 가리키도록 변경한다. 그러나 아직 a의 이전 노드가 b를 가리키지는 못한다.
<br>

```python
prev.next = b

a = a.next
prev = prev.next.next
```
a의 이전 노드 prev가 b를 가리키게 하고, 다음번 비교를 위해 prev는 두 칸 앞으로 이동한다.<br>
그리고 다시 다음번 교환을 진행할 것이다. 변수명을 문제 풀이에 맞게 조금 수정하고 while 문을 포함한 전체 코드로 정리해보면 다음과 같다.
```python
def swapPairs(self, head: ListNode) -> ListNode:
  root = prev = ListNode(None)
  prev.next = head
  while head and head.next:
    # b가 a(head)를 가리키도록 할당
    b = head.next
    head.next = b.next
    b.next = head
    
    # prev가 b를 가리키도록 할당
    prev.next = b
    
    # 다음번 비교를 위해 이동
    head - head.next
    prev = prev.next.next
  return root.next
```
이전 풀이들과 달리 연결 리스트의 head를 가리키는 노드가 직접 바뀌는 풀이이므로 head를 리턴하지 못하고 그 이전 값을 root로 별도로 설정한 다음 root.next를 리턴하게 했다.
<br><br>

#### 풀이3. 재귀 구조로 스왑
재귀로는 훨씬 더 깔끔하게 풀이할 수 있다. 먼저 재귀 풀이를 코드로 표현하면 전체 코드는 다음과 같다.
```python
def swapPairs(self, head: ListNode) -> ListNode:
  if head and head.next:
    p = head.next
    # 스왑된 값 리턴 받음
    head.next = self.swapPairs(p.next)
    p.next = head
    return p
  return head
```
반복 풀이와 달리 포인터 역할을 하는 p 변수는 하나만 있어도 충분하며, 더미 노드를 만들 필요도 없이 head를 바로 리턴할 수 있어 공간 복잡도가 낮다.<br>
p는 head.next가 되고, p.next는 head가 된다. 물론 그 사이에 재귀 호출로 계속 스왑된 값을 리턴받게 된다.<br>
다른 연결 리스트 문제들의 풀이와 마찬가지로, 실제로는 백트래킹되면서 연결 리스트가 이어지게 된다.<br>
전체적인 구조와 실행 순서는 다음 그림 8-6(232p)과 같다.
<br><br>

### 문제 18 홀짝 연결 리스트
> 233p

* **내가 짠 코드**<br>

### 문제 18 홀짝 연결 리스트 풀이
#### 풀이1. 반복 구조로 홀짝 노드 처리
쉽게 풀 수 있을 것 같은 문제이지만, 공간 복잡도와 시간 복잡도의 제약 사항이 있다.<br>
여러 번 언급한 바 있지만, 이런 문제는 제약이 없을 경우 연결 리스트를 리스트로 바꾸고 파이썬 리스트가 제공하는 슬라이싱과 같은 다양한 함수를 사용하면 좀 더 쉽고 직관적이며 또한 빠르게 풀 수 있다. 그러나 이러한 풀이 방식은 우아하지 않다.<br>
특히 오프라인 코딩 테스트 시에 이 같은 편법을 시도하다가는 면접관에게 다시 풀어달라는 지적을 받게 될 수 있다.

홀수 노두 다음에 짝수 노두가 오게 재구성하라고 했으니 홀(odd), 짝(even) 각 노드를 구성한 다음 홀수 노드의 마지막을 짝수 노드의 처음과 이어주면 될 것 같다.<br>
먼저 입력값은 1->2->3->4->5로 구성된 연결 리스트로 가정해보자.
```python
odd = head
even = head.next
even_head = head.next
```
홀수 변수는 head이고, 짝수 변수는 head.next이다. 짝수의 헤드는 head.next이므로 even_head = even = head.next로 일단 시작한다.
<br>

```python
while even and even.next:
  odd.next = odd.next.next
  odd = odd.next
  even.next = even.next.next
  even = even.next
```
이와 같이 while 반복문으로 루프를 돌면서 처리한다. 각 반복의 결과를 도식화해보면 그림 8-8(234p)과 같다.

첫 번째 반복 시 홀수 odd는 1->3으로 연결되고, 3을 값으로 갖게 된다. 짝수 even은 2->4가 되고, 4를 값으로 갖는다.<br>
두 번째는 3->5와 4->None이 되고, 마지막으로 5와 None이 남게 된다.<br>
그렇다면 다중 할당을 이용해 코드의 라인 수를 좀 더 줄여볼 수 있을까?
```python
odd.next, odd = odd.next.next, odd.next
```
이런 형태가 가능할까?

아쉽게도 가능하지 않다. 다시 한번 자세히 살펴보자.<br>
이 코드의 첫 번째 반복의 경우 1->3이 되지만 odd는 2가 된다. 아직 odd는 head와 동일한 참조며, head.next가 2이기 때문이다.<br>
즉 우리가 기대하는 결과는 1->3이 되고 odd가 3이 되는 것인데, 이처럼 다중 할당으로 처리하게 되면 서로 다른 결과가 된다.<br>
따라서 이렇게 처리할 수 없다. 다만, 홀짝 처리를 하나로 묶어서 다음과 같이 두 줄로 처리하는 다중 할당은 가능하다.
```python
odd.next, even.next = odd.next.next, even.next.next
odd, even = odd.next, even.next
```
이렇게 해서 끝까지 처리되면 홀수 odd는 5, 짝수 even은 None이 된다.
<br>

```python
odd.next = even_head
```
이제 odd.next를 짝수의 헤드 even_head와 연결한다. 즉 5->2가 된다. 마지막으로 다음과 같이 head를 리턴한다
```python
return head
```
head는 1, even_head는 2가 되기 때문에, 앞서 처리된 내용을 바탕으로 1부터 결과를 쭉 나열해보면 1->3->5->2->4->None이 된다. 우리가 기대했던 결과다.<br>
아울러 odd, even, even_head 등의 변수들은 n의 크기에 관계 없이 항상 일정하게 사용하기 때문에, 공간 복잡도 또한 O(1)으로 제약 조건을 만족한다.<br>
간단한 예외 처리를 추가한 전체 코드는 다음과 같다.
```python
def oddEvenList(self, head: ListNode) -> ListNode:
  # 예외 처리
  if head is None:
    return None
    
  odd = head
  even = head.next
  even_head = head.next
  
  # 반복하면서 홀짝 노드 처리
  while even and even.next:
    odd.next, even.next = odd.next.next, even.next.next
    odd, even = odd.next, even.next
    
  # 홀수 노드의 마지막을 짝수 헤드로 연결
  odd.next = even_head
  return head
```
<br><br>

### 문제 19 역순 연결 리스트 II
> 237p

* **내가 짠 코드**<br>

<br><br>

### 문제 19 역순 연결 리스트 II 풀이
#### 풀이1. 반복 구조로 노드 뒤집기
연결 리스트는 1->2->3->4->5->6, m은 2, n은 5로 가정하고 풀이를 진행해보자.
```python
root = start = ListNode(None)
root.next = head
for _ in range(m - 1):
  start = start.next
end = start.next
```
여기서 start는 변경이 필요한 2의 바로 앞 지점인 1을 가리키게 하고 end는 start.next인 2로 지정한다.<br>
그리고 head는 1인데, 그보다도 더 앞에 root를 만들어 head보다 이전에 위치시킨다. 나중에는 root.next를 최종 결과로 리턴하게 될 것이다.

이렇게 할당된 start와 end는 꿑까지 값이 변하지 않는다. 즉 1과 2로 마지막까지 유지되며, start와 end를 기준으로 그림 8-9(238p)와 같이 반복하면서 역순으로 뒤집는다.

그림에서 2)부터가 반복하며 진행되는 부분이다.<br>
start.next를 tmp로 지정하고, start.next는 end.next가 된다. 그리고 end.next는 end.next.next로 한 칸 더 앞의 값을 끌어온다. 그리고 start.next.next를 tmp로 지정한다.<br>
즉 이전에 start.next였던 노드를 배치하는 것과 동일하다. 이를 코드로 구현해보면 다음과 같다.
```python
for _ in range(n - m):
  tmp, start.next, end.next = start.next, end.next, end.next.next
  start.next.next = tmp
```
이 같은 구조로 n - m만큼 반복하면 최종 결과는 그림 8-9(238p)에서 5)번 결과가 된다.<br>
연결 리스트에서 인덱스 m에서 n까지 노드가 잘 뒤집어졌다. 이제 root.next를 리턴하면 된다.

다중 할당으로 간편하게 처리했지만, 굳이 다중 할당으로 처리하지 않고 다음과 같이 모두 한 줄씩 따로 처리해도 문제 없이 동일한 결과를 얻을 수 있다.
```python
for _ in range(n - m):
  tmp = start.next
  start.next = end.next
  end.next = end.next.next
  start.next.next = tmp
```
그러나 전체 코드는 되도록 간결한 형태로 다중 할당으로 처리한다.<br>
예외 처리를 추가한 전체 코드는 다음과 같다.
```python
def reverseBetween
```














