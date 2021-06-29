# 목차

---
* [출처](#출처)

<br><br><br>

# heapq 모듈
heapq 모듈은 우선순위 큐(priority queue) 알고리즘을 제공한다.

heapq 모듈은 이진 트리 기반의 **최소 힙(min heap)** 자료구조를 지원하며, 내장 모듈로 별도의 설치 작업 없이 바로 사용할 수 있다.<br>
최소 힙에서 가장 작은 값은 언제나 0번 인덱스(이진 트리의 루트)에 위치한다.<br>
최소 힙 내의 모든 원소(k)는 자식 원소(2k+1, 2k+2)보다 크기가 작거나 같도록 원소가 추가되고 삭제된다.

## heap 생성
heapq 모듈은 보통 리스트를 힙처럼 다룰 수 있게 한다.

**1. 빈 리스트를 생성하여 힙처럼 다루는 방법**<br>
```python
heap = []
heapq.heappush(heap, 4) # 힙에 원소 추가하는 함수
heapq.heappush(heap, 1)
heapq.heappush(heap, 7)
heapq.heappush(heap, 3)
print(heap)
--------------------------
[1, 3, 7, 4]	# 실행 결과
```
heappush() 함수는 O(log n)의 시간 복잡도를 갖는다.
<br><br>

**2. 기존에 있던 리스트를 heapify()를 이용해 힙으로 만드는 방법**<br>
```python
heap = [3, 4, 7, 2, 1]
heapq.heapify(heap)
print(heap)
----------------------
[1, 2, 7, 3, 4] # 실행 결과
```
heapify() 함수를 이용하면 리스트 내부의 원소들이 힙 구조에 맞게 재배치되며 최소값이 0번째 인덱스에 위치하게 된다.<br>
heapify() 함수의 성능은 인자로 넘기는 리스트의 원소 수에 비례하며, O(n)의 시간 복잡도를 갖는다.
<

## heapq 모듈 활용하기
### 1. heap에 요소 추가
```
heapq.heappush(heap, item)
```



















