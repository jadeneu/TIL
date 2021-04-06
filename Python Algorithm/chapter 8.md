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















