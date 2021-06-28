# 목차

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
11장의 31번 문제 ‘상위 K 빈도 요소’와 비슷한 문제다.<br>
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
전체 코드는 전보다 훨씬 더 단순해 졌다.
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





























