# 목차

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
Counter에는 most_commun()이라는 빈도 수가 높은 순서대로 아이템을 추출하는 기능을 제공한다.

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






















