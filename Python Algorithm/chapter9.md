# 9장 스택, 큐
스택(Stack)과 큐(Queue)는 프로그래밍이라는 개념이 탄생할 때부터 사용된 가장 고전적인 자료구조 중 하나로, 그중에서도 스택은 거의 모든 애플리케이션을 만들 때 사용되는 자료구조다.<br>
스택은 **LIFO(Last-In-First-Out)**, 큐는 **FIFO(First-In-First-Out)** 로 처리된다.<br>
스택과 큐는 다음과 같은 간단한 비유를 들면 쉽게 이해가 된다. 스택은 잔뜩 쌓아둔 접시를 떠올려보자. 마지막에 쌓은 접시가 맨 위에 놓일 것이라, 가장 마지막에 쌓은 접시를 제일 먼저 꺼내게 된다. 큐는 맛집에 입장하기 위해 줄을 서는 것을 떠올려보자. 가장 먼저 줄을 선 사람이 가장 먼저 입장한다.

파이썬은 스택 자료형을 별도로 제공하지는 않지만, **리스트가 사실상 스택의 모든 연산을 지원한다**. 큐 또한 마찬가지다. **리스트는 큐의 모든 연산을 지원한다**.<br>
다만 리스트는 동적 배열로 구현되어 있어 큐의 연산을 수행하기에는 효율적이지 않기 때문에, 큐를 위해서는 데크(Deque)라는 별도의 자료형을 사용해야 좋은 성능을 낼 수 있다.
<br><br>

## 스택
> 스택은 다음과 같은 2가지 주요 연산을 지원하는 요소의 컬렉션으로 사용되는 추상 자료형이다.
> * push(): 요소를 컬렉션에 추가한다.
> * pop(): 아직 제거되지 않은 가장 최근에 삽입된 요소를 제거한다.
<br>

스택은 콜 스택(Call Stack)이라 하여 컴퓨터 프로그램의 서브루틴에 대한 정보를 저장하는 자료구조에도 널리 활용된다. 컴파일러가 출력하는 에러도 스택처럼 맨 마지막 에러가 가장 먼저 출력되는 순서를 보인다.
<br>

* **서브루틴이란?**<br>
서브루틴은 반복되는 특정 기능을 모아 별도로 묶어 놓아 이름을 붙여 놓은 것으로 메인루틴을 보조하는 역할을 한다. 보통 언어에서는 함수나 메소드 등으로 불리며 사용된다. <br>
(반복되어 사용하는 것을 메모리에 한번 적재하여 여러번 사용할 수 있도록 하는 방법이다.)

이외에도 꽉 찬 스택에 요소를 삽입하고자 할 때 스택에 요소가 넘쳐서 에러가 발생하는 것을 스택 버퍼 오버플로(Stack Buffer Overflow)라고 부른다.

스택 추상 자료형(Abstract Data Type)(이하 ADT)을 연결 리스트로 구현한 것은 그림 9-1(242p)과 같다. 참고로 ADT는 자료형의 연산을 정의한 것으로 실제 구현 방법은 명시하지 않는다. <br>
이 그림에서 ADT는 스택의 연산을 지원하기 위해 1부터 5까지 각 요소가 접시 쌓듯 차곡차곡 놓이겠지만, 실제로 연결 리스트로 구현하게 된다면 물리 메모리 상에는 순서와 관계 없이 여기저기에 무작위로 배치되고 포인터로 가리키게 될 것이다.
<br><br>

## 연결 리스트를 이용한 스택 ADT 구현
연결 리스트를 이용해 실제로 스택을 한번 구현해보자. 먼저 다음과 같이 연결 리스트를 담을 Node 클래스부터 정의한다.
```python
class Node:
  def __init__(self, item, next):
    self.item = item
    self.next = next
```
초기화 함수 __ init __ ()에서 노드의 값을 item으로, 다음 노드를 가리키는 포인터는 next로 정의한다. 이제 스택의 연산인 push()와 pop()을 담은 Stack 클래스를 다음과 같이 정의한다.
```python
class Stack:
  def __init__(self):
    self.last = None
    
  def push(self, item):
    self.last = Node(item, self.last)
    
  def pop(self):
    item = self.last.item
    self.last = self.last.next
    return item
```
push()는 연결 리스트에 요소를 추가하면서 가장 마지막 값을 next로 지정하고, 포인터인 last는 가장 마지막으로 이동시킨다.<br>
pop()은 가장 마지막 아이템을 끄집어내고 last 포인터를 한 칸 앞으로 전진시킨다. 즉 이전에 추가된 값을 가리키게 한다.<br>
이제 다음과 같이 1부터 5까지 값을 스택에 입력해보자.
```python
stack = Stack()
stack.push(1)
stack.push(2)
stack.push(3)
stack.push(4)
stack.push(5)
```
스택 변수 stack의 입력된 값을 도식화하면 그림 9-2(244p)와 같다.

stack은 각각 이전 값을 가리키는 연결 리스트로 구현되어 있으며, 가장 마지막 값은 last 포인터가 가리킨다. 이제 pop() 메소드로 스택의 값을 차례대로 출력해보자.
```python
>>> for _ in range(5):
...     print(stack.pop())
...
5
4
3
2
1
```
가장 최근에 입력된 순서대로, 즉 LIFO 순으로 잘 출력되는 것을 확인할 수 있다. 이렇게 연결 리스트를 이용해 스택 ADT를 어렵지 않게 구현해보았다. 이제 스택과 관련한 문제를 몇 가지 풀어보자.
<br><br>

## 리트코드
### 문제 20 유효한 괄호
> 245p

* **내가 짠 코드**<br>
문제 이해가 안 돼서 풀이 보고 코드 짜봄
```python
def isValid(s):
    stack = []
    table = {
        ')' : '(',
        ']' : '[',
        '}' : '{'
    }

    for i in s:
      if i not in table:
        stack.append(i)
      elif not stack or table[i] != stack.pop():
        return False
    return len(stack) == 0



s = '()[]{}'
print(isValid(s))
```
<br><br>

### 문제 20 유효한 괄호 풀이
#### 풀이1. 스택 일치 여부 판별
전형적인 스택 문제로, 매우 쉬우면서도 기본기를 점검할 수 있는 좋은 문제다.<br>
( , [ , { 는 스택에 푸시(Push)하고, ) , ] , } 를 만날 때 스택에서 팝(Pop)한 결과가 매핑 테이블 결과와 매칭되는지 확인하면 된다.<br>
여기서는 먼저 매핑 테이블을 만들어 놓고 테이블에 존재하지 않으면 무조건 푸시하고, 팝했을 때 결과가 일치하지 않으면 False를 리턴하도록 다음과 같이 구현했다.
```python
stack = []
table = {
    ')': '(',
    '}': '{',
    ']': '[',
}
...
if char not in table:
  stack.append(char)
elif table[char] != stack.pop():
  return False
```
여기서 stack은 간편하게 파이썬의 동적 배열 구현인 리스트를 사용했다.<br>
파이썬 리스트는 스택 연산인 푸시와 팝이 O(1)에 동작하기 때문에 성능에도 큰 문제가 없다. 이제 실제로 실행시키기 위해서는 예외 처리가 필요하다.<br>
이 문제의 경우에도 리트코드에는 비정상적인 테스트 케이스가 다수 포함되어 있기 때문에 제대로 실행되기 위해서는 다음과 같이 예외 처리를 해야 한다.
```python
  elif not stack or table[char] != stack.pop():
      return False
return len(stack) == 0
```
이 코드에서는 팝 결과가 일치하는지 않는지 확인하는 것 외에도 스택이 비어 있는지 여부를 함께 확인하여 True, False를 결정하게 한다. 이처럼 예외 처리가 반드시 포함되어야 한다.<br>
이제 전체 코드는 다음과 같다.
```python
def isValid(self, s: str) -> bool:
    stack = []
    table = {
        ')': '(',
        '}': '{',
        ']': '[',
    }
    
    # 스택 이용 예외 처리 및 일치 여부 판별
    for char in s:
        if char not in table:
          stack.append(char)
        elif not stack or table[char] != stack.pop():
          return False
    return len(stack) == 0
```
<br><br>

### 문제 21 중복 문자 제거
> 247p

* **내가 짠 코드**<br>
```python
def duplicate(s):
  to_set = sorted(set(s))
  to_str = ''
  for char in to_set:
    to_str += char

  return to_str
  

s = input()
print(duplicate(s))
```














