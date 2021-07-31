# 목차
* [chapter 17. 정렬](#17장-정렬)
  + [문제 64 원점에 K번째로 가까운 점](#문제-64-원점에-k번째로-가까운-점)
  + [문제 64 원점에 K번째로 가까운 점 풀이](#문제-64-원점에-k번째로-가까운-점-풀이)
    - [풀이1. 유클리드 거리의 우선순위 큐 순서](#풀이1-유클리드-거리의-우선순위-큐-순서)

<br><br><br>

# 17장. 정렬
## 문제 64 원점에 K번째로 가까운 점
> 511p

* 평면상에 points 목록이 있을 때, 원점 (0, 0)에서 K번 가까운 점 목록을 순서대로 출력하라. 평면상 두 점의 거리는 유클리드 거리로 한다.
* 입력
```
points = [[1,3], [-2,2]], K = 1
```
* 출력
```
[[-2,2]]
```
* 설명<br>
  (1,3)과의 거리는 root(10)이고, (-2,2)와의 거리는 root(8)이다. 두 번째가 더 가까우며, K=1로 가장 가까운 거리 K개는 [[-2,2]]이다.

<br>

* 입력
```
points = [[3,3], [5,-1], [-2,4]], K = 2
```
* 출력
```
[[3,3], [-2,4]]
```
* 설명<br>
  가장 가까운 거리 K=2개는 [[3,3], [-2,4]]이다.

<br><br>

* **내가 짠 코드**<br>
```python

```

<br><br>

## 문제 64 원점에 K번째로 가까운 점 풀이
### 풀이1. 유클리드 거리의 우선순위 큐 순서
유클리드 거리(Euclidean Distance)는 유클리드 공간에서 두 점 사이의 거리를 계산하는 가장 '일반적인' 방법 중 하나로 수식은 다음과 같다.

<img src="https://user-images.githubusercontent.com/55045377/127101918-41c54972-aae7-4619-b34c-429db7f6b75a.png" width=40% height=40%>

예를 들어 원점 (0, 0) 을 x 좌표라고 할 때 (1, 3) 에 있는 y 좌표와의 유클리드 거리는 root((0 - 1)^2 + (0 - 3)^2) 이고 이 값은 root(10), 약 3.16 정도가 된다.<br>
이 값을 크기가 작은 순으로 K번 추출하면 되는, 어렵지 않게 풀 수 있는 문제다.

**K번 추출이라는 단어에서 바로 우선순위 큐를 떠올릴 수 있다.**<br>
즉 유클리드 거리를 계산하고 이 값을 우선순위 큐로 K번 출력하면 쉽게 문제를 풀이할 수 있다.<br>
math 모듈을 사용해 위의 유클리드 거리 수식을 구현해 계산하고 힙에 삽입하는 코드는 다음과 같다.

```python
heap = []
for (x, y) in points:
    dist = math.sqrt((0 - x) ** 2 + (0 - y) ** 2)
    heapq.heappush(heap, (dist, x, y))
```
우선순위 큐는 주로 힙으로 구현한다고 이미 여러 차례 설명했다.<br>
파이썬의 heapq 모듈은 최소 힙(Min Heap)이기 때문에 거리가 가까운 순을 출력해야 하는 이 문제 풀이에 더욱 적합하다. <br>
만약 가장 먼 거리를 출력해야 한다면, 음수로 변환하여 -dist를 삽입하는 형태로 풀이할 수 있을 것이다.

이제 결과를 추출할 차례다. 다음과 같이 구현할 수 있다.
```python
result = []
for _ in range(K):
    (dist, x, y) = heapq.heappop(heap)
    result.append((x, y))
return result
```
결과 리스트에 힙에서 추출한 결과를 K번 삽입해 리턴하면 된다. 

이제 모두 정리해본 코드는 다음과 같다.
```python
def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
    heap = []
    for (x, y) in points:
        dist = math.sqrt((0 - x) ** 2 + (0 - y) ** 2)  # 1
        heapq.heappush(heap, (dist, x, y))
        
    result = []
    for _ in range(K):
        (dist, x, y) = heapq.heappop(heap)
        result.append((x, y))
    return result
```
그런데 여기서 거리를 계산해 다른 데서 활용할 게 아니라면 수식을 좀 더 간략하게 만들 수 있을 것 같다.<br>
어차피 순서만 동일하면 되기 때문이다. 따라서 여기서 math.sqrt()는 생략해도 무방하다.<br>
이 경우 불필요한 계산을 줄일 수 있으므로 수행 속도를 더욱 높일 수 있다. <br>
계산하는 "# 1" 부분의 수식을 줄여 최적화를 적용한 전체 코드는 다음과 같다.
```python
def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
    heap = []
    for (x, y) in points:
        dist = x ** 2 + y ** 2
        heapq.heappush(heap, (dist, x, y))
        
    result = []
    for _ in range(K):
        (dist, x, y) = heapq.heappop(heap)
        result.append((x, y))
    return result
```

<br><br><br>


  
  
  
  
  
  
 
  
  
  
  
  
  
  
  
  

  
  
  
  
  
  
  
  
  
  
  
  
