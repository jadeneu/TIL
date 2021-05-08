# 목차
* [chapter 11. 해시 테이블](#11장-해시-테이블)
* [리트코드](#리트코드)
  + [문제 31 상위 K 빈도 요소](#문제-31-상위-k-빈도-요소)
  + [문제 31 상위 K 빈도 요소 풀이](#문제-31-상위-k-빈도-요소-풀이)
    - [풀이1. Counter를 이용한 음수 순 추출](#풀이1-counter를-이용한-음수-순-추출)
    - [풀이2. 파이썬다운 방식](#풀이2-파이썬다운-방식)
    - [문법. zip() 함수](#문법-zip-함수)
    - [문법. 아스테리스크(*)](#문법-아스테리스크--)
<br><br><br>

# 11장 해시 테이블
## 리트코드
### 문제 31 상위 K 빈도 요소
> 307p

* **내가 짠 코드**<br>
```python
import collections

def k_elements(nums, k):
  dic = collections.Counter(nums)
  elements = []

  for num in dic:
    if dic[num] >= k:
      elements.append(num)

  return elements


nums = [1,1,1,2,2,3]
k = 2
print(k_elements(nums,k))
```
<br><br>

### 문제 31 상위 K 빈도 요소 풀이
#### 풀이1. Counter를 이용한 음수 순 추출
요소의 값을 키로 하는 해시 테이블을 만들고 여기에 빈도 수를 저장한 다음, 우선순위 큐(Priority Queue)를 이용해 상위 k번만큼 추출하면 k번 이상 등장하는 요소를 손쉽게 추출할 수 있다.<br>
파이썬에서 우선순위 큐는 힙을 활용하는 heapq 모듈을 사용한다. 이 문제 또한 heapq 모듈을 사용해 풀이해본다.<br>
먼저 빈도 수는 collections.Counter를 이용하여 다음과 같이 쉽게 구해보자.
```python
freqs = collections.Counter(nums)
```
입력값이 [1,1,1,2,2,3]일 때 freqs의 결과를 출력해보면 다음과 같다.
```python
>>> type(s.freqs)
collections.Counter
>>> s.freqs
Counter({
    1: 3,
    2: 2,
    3: 1
})
```
<br>

이제 힙에 삽입해보자.<br>
삽입 방식은 2가지가 있는데, 첫째는 일반적인 파이썬의 리스트에 모두 삽입한 다음 마지막에 heapify()를 하는 방식과, 두 번재는 매번 heappush()를 하는 방식이다.<br>
heappush()로 삽입하게 되면 매번 heapify()가 일어나기 때문에 별도로 처리할 필요가 없다.<br>
이 방식이 원래 힙의 삽입 방식이기도 하다.<br>
여기서는 후자인 heappush()로 처리해본다.
```python
for f in freqs:
    heapq.heappush(freqs_heap, (-freqs[f], f))
```
여기서는 빈도 수를 키로 하고, freqs의 키를 값으로 했다. 즉 키/값을 바꿔서 힙에 추가했다.<br>
힙은 키 순서대로 정렬되기 때문에 이를 위해 빈도 수를 키로 한 것이다.<br>
또한, 값을 음수로 저장했다.<br>
파이썬 heapq 모듈은 최소 힙만 지원하기 때문이다.<br>
모듈 차원에서는 최대 힙도 지원하긴 하지만 메소드가 프로텍티드 멤버(Protected Member)로 선언되어 있고 함수명이 밑줄( _ )로 시작하기 때문에(밑줄로 시작되는 함수명이 내부 함수라는 오랜 관례가 있다. PEP 8에서도 내부 용도(Internal Use)로 규정한다) 직접 호출하는 게 권장하는 방법은 아니다.<br>
따라서 여기서는 최소 힙을 그대로 사용하되 음수로 변환해 가장 빈도 수가 높은 값이 가장 큰 음수가 되게 한다.<br>
이렇게 하면 최소 힙으로도 빈도 수가 가장 높았던 값을 추출할 수 있다.<br>
마지막으로 다음과 같이 heappop()으로 k번만큼 값을 추출하면 결과를 얻을 수 있다.
```python
topk = list()
for _ in range(k):
    topk.append(heapq.heappop(freqs_heap)[1])
```
이제 이렇게 생성한 topk 리스트가 최종 결과가 된다.

전체 코드는 다음과 같다.
```python
def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    freqs= collections.Counter(nums)
    freqs_heap = []
    # 힙에 음수로 삽입
    for f in freqs:
        heapq.heappush(freqs_heap,(-freqs[f],f))
        
    topk = list()
    # k번 만큼 추출, 최소 힙(Min Heap)이므로 가장 작은 음수 순으로 추출
    for _ in range(k):
        topk.append(heapq.heappop(freqs_heap)[1])
        
    return topk
```
<br><br>

#### 풀이2. 파이썬다운 방식
상위 k번만큼의 요소를 추출하기 위해, 힙에 넣고 빼는 작업들을 진행했다.<br>
이 작업을 자동으로 할 수는 없을까?<br>
분명 파이썬에는 이런 기능을 지원하는 함수가 있을 것 같다.<br>
좀 더 고민해보면 이 문제도 파이썬다운 방식으로 풀이할 수 있다.<br>
Counter에는 most_common()이라는 빈도 수가 높은 순서대로 아이템을 추출하는 기능을 제공한다.

다음과 같이 사용할 수 있다.
```python
>>> collections.Counter(nums).most_common(k)
[(1, 3), (2, 2)]
```
이제 여기서 정답인 1과 2를 추출하기만 하면 된다.<br>
여기서는 파이썬의 2가지 기능인 zip()과 * (별표)를 더 활용해본다.<br>
일단 여기서는 먼저 풀이를 진행하고 2가지 기능에 대해서는 이후에 다시 자세히 살펴보자.<br>
먼저 두 기능을 활용한 정답은 다음과 같다.
```python
>>> list(zip(*collections.Counter(nums).most_common(k)))[0]
(1, 2)
```
여기서는 튜플이 나왔지만 리스트와 마찬가지로 모두 정답으로 처리된다.

이제 전체 코드는 다음과 같이 한 줄로 처리할 수 있다.
```python
def topKFrequent(self, nums, k):
    return list(zip(*collections.Counter(nums).most_common(k)))[0]
```
<br><br>

#### 문법. zip() 함수
zip() 함수는 2개 이상의 시퀀스를 짧은 길이를 기준으로 일대일 대응하는 새로운 튜플 시퀀스를 만드는 역할을 한다.<br>
먼저 다음과 같이 a, b, c 리스트를 각각 정의하고 zip(a, b)를 실행한 결과를 살펴보자.
```python
>>> a = [1,2,3,4,5]
>>> b = [2,3,4,5]
>>> c = [3,4,5]
>>> zip(a,b)
<zip object at 0x105b6d9b0>
```
파이썬 2에서는 zip()의 결과가 바로 리스트가 된다. 하지만 파이썬 3+에서는 제너레이터를 리턴한다.<br>
제너레이터를 리턴하는 게 어떤 장점이 있는지는 3장의 81페이지에서 이미 충분히 설명한 바 있다.<br>
제너레이터에서 실젯값을 추출하기 위해서는 다음과 같이 list()로 한 번 더 묶어주면 된다.
```python
>>> list(zip(a,b))
[(1, 2), (2, 3), (3, 4), (4, 5)]
>>> list(zip(a,b,c))
[(1, 2, 3), (2, 3, 4), (3, 4, 5)]
```
아울러 zip()의 결과 자체는 리스트 시퀀스가 아닌 튜플 시퀀스를 만들기 때문에, 값을 변경하는 게 불가능하다.<br>
불변(Immutable) 객체다. 다음과 같이 값의 조직을 시도해보자.
```python
>>> d = list(zip(a,b,c))
>>> d[0]
(1, 2, 3)
>>> d[0][0]
1
>>> d[0][0] = 0
Traceback (most recent call last):
  ...
TypeError: 'tuple' object does not support item assignment
```
이 코드의 실행 결과를 보면 튜플 객체는 아이템 할당을 지원하지 않는다는 TypeError가 발생했다.<br>
튜플은 불변 객체이기 때문이다. 이처럼 zip()은 여러 시퀀스에서 동일한 인덱스의 아이템을 순서대로 추출하여 튜플로 만들어 주는 역할을 한다.<br>
앞서 문제 풀이에서 [(1, 3), (2, 2)]는 zip()으로 묶게 되면 [(1, 2), (3, 2)]가 될 것이다. 튜플의 값과 개수가 모두 2개라서 다소 헷갈릴 수 있는데, 개수를 늘려서 다음 코드를 살펴보면 좀 더 이해가 쉬울 것 같다.
```python
>>> a = ['a1', 'a2']
>>> b = ['b1', 'b2']
>>> c = ['c1', 'c2']
>>> d = ['d1', 'd2']
>>> list(zip(a, b, c, d))
[('a1', 'b1', 'c1', 'd1'), ('a2', 'b2', 'c2', 'd2')]
```
이처럼 a,b,c,d 각각의 리스트가 zip()으로 묶여서 2개의 튜플이 되었다.
<br><br>

#### 문법. 아스테리스크( * )
zip()의 파라미터는 1개가 될 수도 있고, 2개가 될 수도, 10개가 될 수도 있다.<br>
앞서 참고 박스 마지막의 입력값을 기준으로 실험해보면 다음과 같다.
```python
>>> list(zip(a))
[('a1',), ('a2',)]
>>> list(zip(a, b))
[('a1', 'b1'), ('a2', 'b2')]
>>> list(zip(a, b, c))
[('a1', 'b1', 'c1'), ('a2', 'b2', 'c2')]
```
어떻게 하면 이렇게 할 수 있을까? 여기에는 아스테리스크(Asterisk) 혹은 흔히 별표라고 부르는 * 를 활용한다.<br>
C를 개발해본 사람이라면 포인터 변수와 혼동될 수 있다.<br>
그러나 파이썬에는 포인터가 존재하지 않는다.<br>
게다가 생긴 건 비슷하지만 전혀 다른 동작을 수행한다.<br>
파이썬에서 * 는 언팩(Unpack)이다. 시퀀스 언패킹 연산자(Sequence Unpacking Operator)로 말 그대로 **시퀀스를 풀어 헤치는 연산자를 뜻하며**, 주로 튜플이나 리스트를 언패킹하는 데(풀어 헤치는 데) 사용한다.<br>
앞서 31번 문제의 풀이2에서와 같이 언패킹 했을 때와 하지 않았을 때를 비교해보면 그 용도를 분명히 알 수 있을 것이다.
```python
>>> collections.Counter(nums).most_common(k)
[(1, 3), (2, 2)]
# 언패킹을 했을 때(31번 문제 풀이2)
>>> list(zip(*collections.Counter(nums).most_common(k)))
[(1, 2), (3, 2)]
# 언패킹을 하지 않았을 때
>>> list(zip(collections.Counter(nums).most_common(k)))
[((1, 3),), ((2, 2),)]
```
입력값이 [1,1,1,2,2,3]일 때 collections.Counter(nums).most_common(k)의 결과가 [(1, 3), (2, 2)]임은 이미 살펴본 바 있다.<br>
그러나 이 값을 그대로 zip()으로 묶어 보면 엉뚱한 결과가 나온다.<br>
[((1, 3),), ((2, 2),)] 이런 식으로 튜플이 풀어지지 않고 그대로 하나의 값처럼 묶여 버렸다.<br>
이 경우 * 로 언패킹을 해줘야 튜플의 값을 풀어 헤칠 수 있다.<br>
언패킹한 값만 별도로 출력할 수가 없기 때문에 디버깅이 어렵지만, 아마 내부적으로는 튜플이 제거되고 [[1, 3], [2, 2]]과 같은 형태로 모두 리스트로 풀어질 것이다.<br>
이제 이 값을 zip()으로 묶으면 정상적으로 묶이게 된다. 즉 정답인 1과 2가 하나의 튜플로 묶이게 된다.<br>
간단한 예제를 하나 더 살펴보자.
```python
>>> fruits = ['lemon', 'pear', 'watermelon', 'tomato']
>>> fruits
['lemon', 'pear', 'watermelon', 'tomato']
```
fruits라는 리스트를 출력하면 당연히 리스트 형태로 출력된다.<br>
만약 이 리스트에서 각 요소의 값만 출력하고 싶다면 어떻게 하면 될까?
```python
>>> print(fruits[0], fruits[1], fruits[2], fruits[3])
lemon pear watermelon tomato
```
이렇게 출력할 수 있다.<br>
또는 다음과 같이 for 반복문으로 순회하여 값을 출력할 수도 있을 것이다.
```python
>>> for f in fruits:
...     print(f, end=' ')
...
lemon pear watermelon tomato
```
이때 * 로 언패킹해주면 다음과 같이 매우 간편하게 출력할 수 있다.
```python
>>> print(*fruits)
lemon pear watermelon tomato
```
이외에도 * 는 활용도가 많다.<br>
언패킹뿐만 아니라 함수의 파라미터가 되었을 때는 반대로 패킹(Packing)도 가능하다.<br>
앞서 언급했던 zip()에 파라미터를 여러 개 쓸 수 있다는 얘기 또한 내부적으로 zip() 함수 정의에서 * 로 패킹하고 있다는 얘기이기도 하다.
```python
>>> def f(*params):
...     print(params)
...
>>> f('a', 'b', 'c')
('a', 'b', 'c')
```
이처럼 하나의 파라미터를 받는 함수에 3개의 파라미터를 전달했지만, params 변수 하나로 패킹되어 처리된다.<br>
눈치가 빠른 분들이라면 짐작하겠지만, 이는 파이썬 3+에서 print() 함수의 기본 동작 원리이기도 하다.
```python
>>> print('a')
a
>>> print('a','b')
a b
>>> print('a','b','c')
a b c
```
몇 개의 파라미터를 넘기든, 모두 처리가 된다.<br>
이번에는 다음과 같이 또 다른 활용 예를 살펴보자.
```python
>>> a, *b = [1,2,3,4]
>>> a
1
>>> b
[2,3,4]
>>> *a, b = [1,2,3,4]
>>> a
[1,2,3]
>>> b
4
```
변수의 할당 또한 이렇게 * 로 묶어서 처리할 수 있다.<br>
일반적인 변수는 값을 하나만 취하지만 * 로 처리하게 되면 나머지 모든 값을 취하게 된다.<br>
이외에도 활용 방법은 상당히 많다. * 는 파이썬에서 매우 중요하며 자주 쓰이는 기능 중 하나이므로, 반드시 숙지하기 바란다.<br>
마지막으로 하나가 아닌 2개를 쓰는 경우도 있다.<br>
** 인데 마찬가지로 C의 더블 포인터와 동일하게 생겼지만, 전혀 다른 동작을 수행한다.<br>
파이썬에서 * 1개는 튜플 또는 리스트 등의 시퀀스 언패킹이고, ** 2개는 다음과 같이 키/값 페어를 언패킹하는 데 사용된다.
```python
>>> date_info = {'year': '2020', 'month': '01', 'day': '7'}
>>> new_info = {**date_info, 'day': "14"}
>>> new_info
{'year': '2020', 'month': '01', 'day': '14'}
```
이처럼 ** dete_info에 모든 요소를 언패킹할 수 있으며, 여기서는 'day': "14"의 새로운 값으로 업데이트도 시도했다.<br>
그 결과 day의 값은 변경된 것을 확인할 수 있다.
<br><br>























