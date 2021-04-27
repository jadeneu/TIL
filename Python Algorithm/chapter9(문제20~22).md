# 목차
* [ 스택, 큐](#9장-스택-큐)
  + [스택](#스택)
  + [연결 리스트를 이용한 스택 ADT 구현](#연결-리스트를-이용한-스택-adt-구현)
* [리트코드](#리트코드)
  + [문제 20 유효한 괄호](#문제-20-유효한-괄호)
  + [문제 20 유효한 괄호 풀이](#문제-20-유효한-괄호-풀이)
    - [풀이1. 스택 일치 여부 판별](#풀이1-스택-일치-여부-판별)
  + [문제 21 중복 문자 제거](#문제-21-중복-문자-제거)
  + [문제 21 중복 문자 제거 풀이](#문제-21-중복-문자-제거-풀이)
    - [풀이1. 재귀를 이용한 분리](#풀이1-재귀를-이용한-분리)
    - [풀이2. 스택을 이용한 문자 제거](#풀이2-스택을-이용한-문자-제거)
  + [문제 22 일일 온도](#문제-22-일일-온도)
  + [문제 22 일일 온도 풀이](#문제-22-일일-온도-풀이)
    - [풀이1. 스택 값 비교](#풀이1-스택-값-비교)
<br><br>



# 9장 스택, 큐
스택(Stack)과 큐(Queue)는 프로그래밍이라는 개념이 탄생할 때부터 사용된 가장 고전적인 자료구조 중 하나로, 그중에서도 스택은 거의 모든 애플리케이션을 만들 때 사용되는 자료구조다.<br>
스택은 **LIFO(Last-In-First-Out)**, 큐는 **FIFO(First-In-First-Out)** 로 처리된다.<br>
스택과 큐는 다음과 같은 간단한 비유를 들면 쉽게 이해가 된다. 스택은 잔뜩 쌓아둔 접시를 떠올려보자. 마지막에 쌓은 접시가 맨 위에 놓일 것이라, 가장 마지막에 쌓은 접시를 제일 먼저 꺼내게 된다. 큐는 맛집에 입장하기 위해 줄을 서는 것을 떠올려보자. 가장 먼저 줄을 선 사람이 가장 먼저 입장한다.

파이썬은 스택 자료형을 별도로 제공하지는 않지만, **리스트가 사실상 스택의 모든 연산을 지원한다**. 큐 또한 마찬가지다. **리스트는 큐의 모든 연산을 지원한다**.<br>
다만 리스트는 동적 배열로 구현되어 있어 큐의 연산을 수행하기에는 효율적이지 않기 때문에, 큐를 위해서는 데크(Deque)라는 별도의 자료형을 사용해야 좋은 성능을 낼 수 있다.
<br><br>

---

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
풀이 방법이 떠오르지 않아서 풀이보고 코드 짜봄
```python
import collections

def duplicate(s):
  counter, seen, stack = collections.Counter(s), set(), []

  for char in s:
    counter[char] -= 1
    if char in seen:
      continue
    if stack and char < stack[-1] and counter[stack[-1]] > 0:
      seen.remove(stack.pop())
    stack.append(char)
    seen.add(char)
  
  return ''.join(stack)
  


s = 'cbacdcbc'
print(duplicate(s))
```
<br><br>

### 문제 21 중복 문자 제거 풀이
#### 풀이1. 재귀를 이용한 분리
문제에 대한 확실한 이해가 필요하다.

먼저 사전식 순서라는 용어를 이해해야 한다.<br>
사전식 순서란 글자 그대로 사전에서 가장 먼저 찾을 수 있는 순서를 말하며, <br>
**bcabc**에서 중복 문자를 제외하면 **사전에서 가장 먼저 찾을 수 있는 문자열은 abc**가 될 것이다.<br>
(bcabc에서 나올 수 있는 문자는 bca, bac, cab, abc가 있는데, 이 중에서 사전에서 가장 먼저 찾을 수 있는 문자는 abc)

만약 앞에 e가 하나 더 붙은 **ebcabc가 입력값이라면** 결과는 **eabc**가 될 것이다.<br>
반면 **입력값이 ebcabce라면** 결과는 **abce**가 될 수 있을 것이다.
<br><br>

입력값 cbacdcbc를 사전식 순서로 중복 문자를 제거하는 과정의 알고리즘은 다음 그림 9-3(248p)과 같이 나타낼 수 있으며, 결과는 파란색 글자인 acdb가 된다.

이 그림에서 중복 문자를 제외한(여기서는 집합으로 처리) 알파벳 순으로 문자열 입력값을 모두 정렬한 다음, 가장 빠른 a부터 접미사 suffix를 분리하여 확인한다.<br>
다음 순서는 b인데, b의 경우 c,d가 뒤에 올 수 없기 때문에 이를 기준으로 분리할 수 없다.
```python
if set(s) == set(suffix):
    ...
```
분리 가능 여부는 이 코드와 같이 전체 집합과 접미사 집합이 일치하는지 여부로 판별한다.

집합은 중복된 문자가 제거된 자료형이므로, 그림 9-3(248p)에서 b를 기준으로 했을 때는 **s = {'b', 'c', 'd'}, suffix = {'b', 'c'}** 가 되며 d를 처리할 수 없으므로 분리할 수 없다. 따라서 다음 순서인 c로 넘긴다.<br>
이제 c는 **s = {'b', 'c', 'd'}, suffix = {'b', 'c', 'd'}** 로 일치 여부를 판별하는 if문이 True이므로 분리할 수 있다.<br>
이제 다음과 같이 진행한다.
```python
return char + self.removeDuplicateLetters(suffix.replace(char, ''))
```
여기서부터 실제로 분리하는 처리를 하게 되는데, 현재 문자, 즉 char를 리턴하는 재귀 호출 구조로 처리한다.<br>
또한 char는 이미 분리되는 기준점이 되었으므로 이후에 이어지는 모든 char는 replace()로 제거한다.<br>
이렇게 하면 일종의 분할 정복(자세한 내용은 22장 참조)과 비슷한 형태로 접미사 suffix의 크기는 점점 줄어들게 되고 더 이상 남지 않을 때 백트래킹(12장 참조)되면서 결과가 조합된다.

전체 코드는 다음과 같다.
```python
def removeDuplicateLetters(self, s: str) -> str:
    # 집합으로 정렬
    for char in sorted(set(s)):
        suffix = s[s.index(char):]
        # 전체 집합과 접미사 집합이 일치할 때 분리 진행
        if set(s) == set(suffix):
            return char + self.removeDuplicateLetters(suffix.replace(char, ''))
    return ''
```
<br>

* **참고 | replace()**<br>
  replace()는 문자열 변경을 할 수 있는 함수이다.<br>
  
  replace()의 사용 방법은 아래와 같다.<br>
  ```
  replace(old, new, [count]) -> replace("찾을값", "바꿀값", 바꿀횟수)
  ```
<br><br>
#### 풀이2. 스택을 이용한 문자 제거
```python
stack.append(char)
seen.add(char)
```
먼저 스택에는 이 코드와 같이 앞에서부터 차례대로 쌓아 나간다.
<br><br>

```python
counter, stack = collections.Counter(s), []
...
while stack and char < stack[-1] and counter[stack[-1]] > 0:
    seen.remove(stack.pop())
```
만약 현재 문자 char가 스택에 쌓여 있는 문자(이전 문자보다 앞선 문자)이고, 뒤에 다시 붙일 문자가 남아 있다면(카운터가 0 이상이라면), 쌓아둔 걸 꺼내서 없앤다.<br>
카운팅에는 collections.Counter()를 이용한다. 

입력값 cbacdcbc에서 a가 들어올 때, 이미 이전에 들어와 있던 c와 b는 다음 그림 9-4(250p)와 같이 제거가 된다. 카운터가 0 이상인 문자인 c와 b는 뒤에 다시 붙일 문자가 남아 있기 때문이다.
```python
if char in seen:
    continue
```
여기서 seen은 집합(Set) 자료형으로, 이미 처리된 문자 여부를 확인하기 위해 사용했으며, 이처럼 이미 처리된 문자는 스킵한다.<br>
여기서는 처리된 문자 여부를 확인하기 위해 in을 이용한 검색 연산으로 찾아냈다. 그러나 해당 기능은 스택에는 존재하지 않는 연산이기 때문에 별도의 집합 자료형에만 사용했다.

사실 여기서 스택으로 가정하고 사용한 파이썬의 자료형은 리스트이고, 리스트는 검색도 잘 지원하기 때문에 굳이 스택이라는 자료 구조로 한정짓지 않고 풀이한다면 seen 변수 없이도 다음과 같은 형태로 얼마든지 풀이가 가능하다.

전체코드 자체는 오히려 더 깔끔해진다.
```python
def removeDuplicateLetters(self, s: str) -> str:
    counter, stack = collections.Counter(s), []
    
    for char in s:
        counter[char] -= 1
        if char in stack:
            continue
        # 뒤에 붙일 문자가 남아 있다면 스택에서 제거
        while stack and char < stack[-1] and counter[stack[-1]] > 0:
            stack.pop()
        stack.append(char)
     
     return ''.join(stack)
```
그러나 이 방식은 스택에 없는 검색 연산을 수행한 변칙적인 풀이 방법이기 때문에, 여기서는 정석대로 스택에서 가능한 연산만을 수행하고 검색 기능은 seen 변수에서 실행되게 하여 다음과 같이 풀이한다.<br>
최종 풀이 코드는 다음과 같다.
```python
def removeDuplicateLetters(self, s: str) -> str:
    counter, seen, stack = collections.Counter(s), set(), []
    
    for char in s:
        counter[char] -= 1
        if char in seen:
            continue
        # 뒤에 붙일 문자가 남아 있다면 스택에서 제거
        while stack and char < stack[-1] and counter[stack[-1]] > 0:
            seen.remove(stack.pop())
        stack.append(char)
        seen.add(char)
     
     return ''.join(stack)
```
stack 변수라는 이름에 걸맞게 스택 ADT에 정의된 연산만을 사용해 문제를 풀이했다.<br>
물론 딱 한 군데, -1로 가장 최근 요소를 조회하기는 했지만, 적어도 전체 요소 검색과 같은 정의되지 않은 연산은 수행하지 않고 되도록 정석대로 풀이하고자 했다.
<br><br>

### 문제 22 일일 온도
> 252p

* **내가 짠 코드**<br>
```python
def temperature(t):
  temp = []

  for i in range(len(t)-1):
    cnt = 0 # 기다리는 날 수
    for j in range(i+1, len(t)):
      cnt += 1
      sub = t[j] - t[i]
      if sub > 0:
        temp.append(cnt)
        break
      else:
        continue

  return temp


T = [73, 74, 75, 71, 69, 72, 76, 73]
print(temperature(T))
```
→ 해결 못함
<br><br>
풀이 보고 코드 짜봄
```python
def temperature(t):
  stack = []
  answer = [0] * len(t)

  for i, temp in enumerate(t):
    while stack and t[i] > t[stack[-1]]:
      last = stack.pop()
      answer[last] = i - last
    stack.append(i)
  return answer
    

T = [73, 74, 75, 71, 69, 72, 76, 73]
print(temperature(T))
```

<br><br>


### 문제 22 일일 온도 풀이
#### 풀이1. 스택 값 비교
이 문제는 앞서 7장에서 풀어본 8번 '빗물 트래핑' 문제와 유사한 방법으로 풀이할 수 있다.<br>
먼저, 예제 입력값을 기준으로 도식화해보면 다음 그림 9-5(253p)와 같다.

현재의 인덱스를 계속 스택에 쌓아두다가, 이전보다 상승하는 지점에서 현재 온도와 스택와 쌓아둔 인덱스 지점의 온도 차이를 비교해서, 더 높다면 다음과 같이 스택의 값을 팝으로 꺼내고 현재 인덱스와 스택에 쌓아둔 인덱스의 차이를 정답으로 처리한다.
```python
while stack and cur = T[stack[-1]]:
    last = stack.pop()
    answer[last] = i - last
stack.append(i)
```
예를 들어 그림 9-5(253p)에서 입력값 [73, 74, 75, 71, 69, 72, 76, 73]일 때 인덱스가 5인 지점에서 스택에는 [2,3,4]가 있다. 계속 온도가 떨어지기만 하면서 처리되지 못하고 스택에 계속 쌓여 있었기 때문이다.<br>
인덱스 5의 온도는 72, 스택에 있는 2,3,4의 온도는 각각 75, 71, 69이다. 72는 71, 69보다 높기 때문에 현재 인덱스와 71의 인덱스와는 두 칸 차이로 정답은 2가 되고, 69의 인덱스와는 한 칸 차이로 정답은 1이다.<br>
처리한 인덱스는 스택에서 팝으로 제거된다. 75도인 2는 현재 온도보다 높기 때문에 계속 스택에 남아 있게 된다.

그림 9-5(253p)을 보면, 다음 차례인 인덱스 6에서 스택에는 [2,5]가 남아 있고, 각각 75, 72이다.<br>
이제 현재 온도 76도는 72나 75보다 모두 높기 때문에 스택은 모두 처리되어 비워질 것이다.<br>
72의 인덱스와는 한 칸 차이로 정답은 1이고, 75의 인덱스와는 무려 4칸 차이다. 이보다 높은 온도가 그간 나오지 않았기 때문에 스택에서 비워지지 못하고 계속 남아 있다가 이제서야 처리가 된다.<br>
아울러 디폴트는 0이다. 만약 더 높은 온도가 나오지 않아 스택이 비워지지 않았다면 해당 인덱스는 해당 없음, 즉 문제에서 요구하는 바와 같이 0으로 남게 된다.

이 풀이의 전체 코드는 다음과 같다.
```python
def dailyTemperatures(self, T: List[int]) -> List[int]:
    answer = [0] * len(T)
    stack = []
    for i, cur in enumerate(T):
        # 현재 온도가 스택 값보다 높다면 정답 처리
        while stack and cur > T[stack[-1]]:
            last = stack.pop()
            answer[last] = i - last
        stack.append(i)
        
    return answer
```
<br><br>






























