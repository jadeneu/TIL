# 목차
* [heapq 모듈](#heapq-모듈)
  + [heap 생성](#heap-생성)
  + [heapq 모듈 활용하기](#heapq-모듈-활용하기)
    - [1. heap에 요소 추가](#1-heap에-요소-추가)
    - [2. heap에서 요소 삭제](#2-heap에서-요소-삭제)
    - [3. heap에서 요소 추가 후 삭제](#3-heap에서-요소-추가-후-삭제)
    - [4. heap에서 요소 삭제 후 추가](#4-heap에서-요소-삭제-후-추가)
    - [5. heap 병합](#5-heap-병합)
    - [6. heap에서 가장 큰 요소들 추출](#6-heap에서-가장-큰-요소들-추출)
    - [7. heap에서 가장 작은 요소들 추출](#7-heap에서-가장-작은-요소들-추출)
---
* [출처](#출처)

<br><br><br>

# heapq 모듈
heapq 모듈은 우선순위 큐(priority queue) 알고리즘을 제공한다.

heapq 모듈은 이진 트리 기반의 **최소 힙(min heap)** 자료구조를 지원하며, 내장 모듈로 별도의 설치 작업 없이 바로 사용할 수 있다.<br>
최소 힙에서 가장 작은 값은 언제나 0번 인덱스(이진 트리의 루트)에 위치한다.<br>
최소 힙 내의 모든 원소(k)는 자식 원소(2k+1, 2k+2)보다 크기가 작거나 같도록 원소가 추가되고 삭제된다.
<br><br>

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
<br><br>

**→ heappush()는 요소 하나씩 힙에 추가하는 동시에 최소 힙으로 만들어지고,**<br>
**heapify()는 주어진 리스트 순서대로 힙이 만들어져 있는 상태에서 최소 힙으로 정렬하여 만든다는 차이가 있다.**
<br><br><br>

## heapq 모듈 활용하기
### 1. heap에 요소 추가
```python
heapq.heappush(heap, item)
```
힙 불변성을 유지하면서, item 값을 heap에 추가한다.<br>
* **힙 불변성은 힙의 특성을 유지하는 것을 말함(부모노드가 자식노드보다 작거나 같고, 루트 노드에는 가장 작은 값이 있는...)**
<br><br>

### 2. heap에서 요소 삭제
```python
heapq.heappop(heap)
```
힙 불변성을 유지하면서, heap에서 **가장 작은 요소**를 팝하고 반환한다.<br>
힙이 비어 있으면, IndexError가 발생한다.<br>
팝 하지 않고 가장 작은 요소에 액세스하려면, heap[0]을 사용해야한다.
<br><br>

### 3. heap에서 요소 추가 후 삭제
```python
heapq.heappushpop(heap, item)
```
힙에 item을 푸시한 다음, heap에서 가장 작은 요소를 팝하고 반환한다.<br>
결합한 액션은 heappush()한 다음 heappop()을 별도로 호출하는 것보다 더 효율적으로 실행한다.
<br><br>

### 4. heap에서 요소 삭제 후 추가
```python
heapq.heapreplace(heap, item)
```
heap에서 가장 작은 요소를 팝하고 반환하며, 새로운 item도 푸시한다.<br>
힙 크기는 변경되지 않는다. 힙이 비어 있으면 IndexError가 발생한다.

이 연산은 heappop()한 다음 heappush()하는 것보다 더 효율적이며 고정 크기 힙을 사용할 때 더 적합 할 수 있다.<br>
팝/푸시 조합은 항상 힙에서 요소를 반환하고 그것을 item으로 대체한다.

반환된 값은 추가된 item보다 클 수 있다.<br>
그것이 바람직하지 않다면, 대신 heappushpop() 사용을 고려할 수 있다.<br>
푸시/팝 조합은 두 값 중 작은 값을 반환하여 힙에 큰 값을 남겨 둔다.
<br><br>

### 5. heap 병합
```python
heapq.merge(*iterables, key=None, reverse=False)
```
여러 정렬된 입력을 단일 정렬된 출력으로 병합한다.
<br><br>

### 6. heap에서 가장 큰 요소들 추출
```python
heapq.nlargest(n, iterable, key=None)
```
iterable에 의해 정의된 데이터 집합에서 n개의 가장 큰 요소로 구성된 리스트를 반환한다.<br>
key가 제공되면 iterable의 각 요소에서 비교 키를 추출하는 데 사용되는 단일 인자 함수를 지정한다.
<br><br>

### 7. heap에서 가장 작은 요소들 추출
```python
heapq.nsmallest(n, iterable, key=None)
```
iterable에 의해 정의된 데이터 집합에서 n개의 가장 작은 요소로 구성된 리스트를 반환한다.<br>
key가 제공되면 iterable의 각 요소에서 비교 키를 추출하는 데 사용되는 단일 인자 함수를 지정한다.(예를 들어, key=str.lower)
<br><br>





















---
# 출처
* https://docs.python.org/ko/3/library/heapq.html
* https://velog.io/@sossont/Python-heapq-%EB%AA%A8%EB%93%88
* https://littlefoxdiary.tistory.com/3
