# 목차
* [chapter 15. 힙](#15장-힙)
* [리트코드](#리트코드)
  + [문제 55 배열 K번째 큰 요소](#문제-55-배열-k번째-큰-요소)
  + [문제 55 배열 K번째 큰 요소 풀이](#문제-55-배열-k번째-큰-요소-풀이)
    - [풀이1. heapq 모듈 이용](#풀이1-heapq-모듈-이용)
    - [풀이2. heapq 모듈의 heapify 이용](#풀이2-heapq-모듈의-heapify-이용)
    - [풀이3. heapq 모듈의 nlargest 이용](#풀이3-heapq-모듈의-nlargest-이용)
    - [풀이4. 정렬을 이용한 풀이](#풀이4-정렬을-이용한-풀이)
<br><br><br>

# 15장 힙
## 리트코드
### 문제 55 배열 K번째 큰 요소
> 456p

* 정렬되지 않은 배열에서 k번째 큰 요소를 추출하라.
* 입력
  ```
  [3,2,3,1,2,4,5,5,6], k = 4
  ```
* 출력
  ```
  4
  ```
<br><br>

* **내가 짠 코드**<br>
```python
def largest(arr, k) -> int:
  arr.sort()
  return arr[-k]

arr = [3,2,3,1,2,4,5,5,6]
k = 4
print(largest(arr, k))
```
<br><br>

### 문제 55 배열 K번째 큰 요소 풀이
#### 풀이1. heapq 모듈 이용
11장의 31번 문제 '상위 K 빈도 요소'와 비슷한 문제다.<br>
다른 점이라면 가장 큰(Largest) 값이냐, 가장 빈번한(Most Frequent) 값이냐의 차이 정도라 하겠다.<br>
비슷한 방식으로 풀이가 가능하므로, 이전의 풀이를 먼저 떠올려보자.<br>
그때 풀이했던 코드를 다시 가져와본다.
```python
def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    freqs = collections.Counter(nums)
    freqs_heap = list()
    for f tn freqs:
        heapq.heappush(freqs_heap, (-freqs[f], f))
        
    topk = list( )
    for _ in range(k):
        topk.append(heapq.heappop(freqs_heap)[1])
        
    return topk
```
여기서 함수명은 이 문제에 적합하도록 topKFrequent()에서 findKthLargest로 수정하고, Counter()로 빈도 수를 계산해 삽입했던 예전 풀이 대신 값 자체를 힙에 푸시하고 순서만큼 팝하는 형태로 수정해본다.<br>
전체 코드는 전보다 훨씬 더 단순해졌다.
```python
def findKthLargest(self, nums: List[int], k: int) -> int:
    heap = list()
    for n in nums:
        heapq.heappush(heap, -n)
        
    for _ in range(k):
        heapq.heappop(heap)
        
    return -heapq.heappop(heap)
```
파이썬 heapq 모듈은 최소 힙만 지원하므로, 음수로 저장한 다음 가장 낮은 수부터 추출해 부호를 변환하면 최대 힙처럼 동작하도록 이와 같이 깔끔하게 구현할 수 있다.
<br><br>

#### 풀이2. heapq 모듈의 heapify 이용
모든 값을 꺼내서 푸시(Push)하지 않고도 한 번에 heapify()하여 처리할 수 있다.<br>
heapify()란 주어진 자료구조가 힙 특성을 만족하도록 바꿔주는 연산이며, 이 경우 파이썬의 일반적인 리스트는 힙 특성을 만족하는 리스트로, 값의 위치가 변경된다.<br>
물론 하나라도 값을 추가하면 다시 힙 특성이 깨지지만, 추가가 계속 일어나는 형태가 아니기 때문에 heapify()는 한 번만 해도 충분하다.
```python
def findKthLargest(self, nums: List[int], k: int) -> int:
    heapq.heapify(nums)
    
    for _ in range(len(nums) - k):
        heapq.heappop(nums)
        
    return heapq.heappop(nums)
```
<br><br>

#### 풀이3. heapq 모듈의 nlargest 이용
heapq 모듈은 강력한 기능을 많이 지원한다.<br>
그중에는 n번째 큰 값을 추출하는 기능도 있다.<br>
이 기능을 사용하면, 전체 코드를 다음과 같이 한 줄로 처리할 수 있다.
```python
def findKthLargest(self, nums: List[int], k: int) -> int:
    return heapq.nlargest(k, nums)[-1]
```
k번째만큼 큰 값이 가장 큰 값부터 순서대로 리스트로 리턴된다.<br>
여기서 마지막 인덱스 -1이 k번째 값이 된다.<br>
힙이 아니라도 내부적으로 heapify() 함수도 호출해 처리해주기 때문에, 별도로 힙 처리를 할 필요가 없어 편리하다.<br>
참고로 nsmallest()를 사용하면 동일한 방식으로 n번째 작은 값도 추출이 가능하다.
<br><br>

#### 풀이4. 정렬을 이용한 풀이
이번에는 정렬부터 한 다음, k번째 값을 추출하는 방식으로 풀이해보자.<br>
추가, 삭제가 빈번할 때는 heapq를 이용한 힙 정렬이 유용하지만 이처럼 입력값이 고정되어 있을 때는 그저 한 번 정렬하는 것만으로 충분하다.
```python
def findKthLargest(self, nums: List[int], k: int) -> int:
    nums.sort()
    return nums[-k]
```
sorted()로 큰 값부터 역순으로 정렬하면, 다음과 같이 좀 더 직관적인 풀이도 가능하다.
```python
def findKthLargest(self, nums: List[int], k: int) -> int:
    return sorted(nums, reverse=True)[k - 1]
```
<br>

모든 방식은 실행 속도에 큰 차이가 없으나 '정렬' 방식이 가장 빠르다.<br>
파이썬의 정렬 함수는 팀소트(Timsort)를 사용하며 C로 매우 정교하게 구현되어 있기 때문에, 대부분의 경우에는 파이썬의 내부 정렬 함수를 사용하는 편이 가장 성능이 좋다.
<br><br>



























