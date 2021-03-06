# 목차
* [13장 최단 경로 문제](#13장-최단-경로-문제)
  + [다익스트라 알고리즘](#다익스트라-알고리즘)
* [리트코드](#리트코드)
  + [문제 40 네트워크 딜레이 타임](#문제-40-네트워크-딜레이-타임)
  + [문제 40 네트워크 딜레이 타임 풀이](#문제-40-네트워크-딜레이-타임-풀이)
     - [풀이1. 다익스트라 알고리즘 구현](#풀이1-다익스트라-알고리즘-구현)
  + [문제 41 K 경유지 내 가장 저렴한 항공권](#문제-41-k-경유지-내-가장-저렴한-항공권)
  + [문제 41 K 경유지 내 가장 저렴한 항공권 풀이](#문제-41-k-경유지-내-가장-저렴한-항공권-풀이)
    - [풀이1. 다익스트라 알고리즘 응용](#풀이1-다익스트라-알고리즘-응용)
<br><br><br>

# 13장 최단 경로 문제
> 최단 경로 문제는 각 간선의 가중치 합이 최소가 되는 두 정점(또는 노드) 사이의 경로를 찾는 문제다.

최단 경로를 지도 상의 한 지점에서 다른 지점으로 갈 때 가장 빠른 길을 찾는 것과 비슷한 문제다.<br>
쉽게 말해 내비게이션에서 목적지로 이동할 때, 경로 탐색을 하면 나오는 최적의 경로 문제가 바로 최소 비용이 되는 최단 경로 문제다.

**정점(Vertex)** 은 교차로에 해당하고 **간선(Edge)** 은 길에 해당한다. **가중치(Weight)** 는 거리나 시간과 같은 이동 비용에 해당한다.<br>
이처럼 잘 알려지고 직관적인 문제에는 단 하나의 가장 좋은 풀이 알고리즘이 있을 거라고 생각하기 쉽다.<br>
하지만 실제로는 그래프의 종류와 특성에 따라 각각 최적화된 다양한 최단 경로 알고리즘이 존재한다.<br>
이 중에서 아마 가장 유명한 것인 **다익스트라(Dijkstra) 알고리즘**일 것이다.<br>
오컴의 면도날(Occam's Razor)처럼 단순한 알고리즘이 가장 훌륭한 알고리즘이라는 걸 증명하는 대표적인 알고리즘이기도 하다.
<br><br>

* **오컴의 면도날**<br>
  ```
  과학자들이 쓰는 용어 중에 '오컴의 면도날'이라는 표현이 있다.<br>
  14세기 영국의 논리학자였던 오컴의 이름에서 탄생한 이 용어는 어떤 현상을 설명할 때 필요 이상의 가정과 개념들은 면도날로 베어낼 필요가 있다는 권고로 쓰인다.<br>
  사고의 절약을 요구하는 이 원리는 과학 분야에서 널리 응용되는 일반적인 지침이다.<br>
  대개 과학 분야에서는, 같은 것을 설명할 때 복잡한 것보다는 단순한 과학적 설명을 선호하고, 적은 과정으로 자연 현상을 많이 설명할 수 있는 이론을 복잡한 이론보다 더 선호한다.<br>
  최소 비용, 최대 만족이라는 경제적 원리가 여기에도 적용된다.
  ```
<br><br>

## 다익스트라 알고리즘
다익스트라 알고리즘은 항상 노드 주변의 최단 경로만을 택하는 **그리디(Greedy) 알고리즘** 중 하나로, 단순할 뿐만 아니라 실행 속도 또한 빠르다.<br>
다익스트라 알고리즘은 노드 주변을 탐색할 때 **BFS(너비 우선 탐색)을 이용**하는 대표적인 알고리즘이기도 하다.
<br><br>

DFS(깊이 우선 탐색)는 한 사람이 미로를 찾아 헤매는 과정과 비슷한 반면, BFS는 여러 명의 사람이 각기 서로 다른 갈림길로 흩어져서 길을 찾는 것과 비슷하다.<br>
이때 각자 실뭉치를 가지고 풀어 놓았다가 되감으면서 새로운 길을 탐색한다.<br>
길을 따라가다 갈림길이 나오면 사람들을 나눠 다시 각자 새로운 길을 탐색하고, 다시 갈림길을 만나 새로 탐색할 길이 하나로 모이면, 나뉘어 탐색하던 사람들이 다시 모여서 함께 탐색한다.<br>
다익스트라 알고리즘은 이때 먼저 도착한 사람의 실뭉치를 사용한다.<br>
하지만 가중치가 음수인 경우는 처리할 수 없다.

* **NOTE : 에지 가중치가 모두 같을 땐 BFS, 에지 가중치가 같지 않을 땐 다익스트라 사용**
<br><br>

다익스트라 알고리즘은 임의의 정점을 출발 집합에 더할 때, 그 정점까지의 최단거리는 계산이 끝났다는 확신을 갖고 더한다.<br>
만일 이후에 더 짧은 경로가 존재한다면 다익스트라 알고리즘의 논리적 기반이 무너진다.<br>
이때는 모두 값을 더해서 양수로 변환하는 방법이 있으며, 이마저도 어렵다면 벨만-포드(Bellman-Ford) 알고리즘 같은, 음수 가중치를 계산할 수 있는 다른 알고리즘을 사용해야 한다.<br>
같은 이유로 최장 거리를 구하는 데에는 다익스트라 알고리즘을 사용할 수 없다.

다익스트라의 최초 구현에서는 시간 복잡도가 **O(V^2)** 였으나 현재는 너비 우선 탐색(BFS)시 가장 가까운 순서를 찾을 때 우선순위 큐를 적용하여 이 경우 시간 복잡도는 **O((V+E)log V)** , 모든 정점이 출발지에서 도달이 가능하다면 최종적으로 **O(E log V)** 가 된다.

이제 최단 경로를 찾는 문제를 직접 풀어보면서 다익스트라 알고리즘의 구체적인 구현과 응용하는 방법을 한번 살펴보자.
<br><br>

## 리트코드
### 문제 40 네트워크 딜레이 타임
> 373p

* **내가 짠 코드**<br>
예전에 배운 내용과 구현한 코드를 참고해서 코드를 짰다.
```python
import collections, math

def delay(times,n,k):
  dic = collections.defaultdict(list)
  dist = [math.inf for _ in range(n)]
  parent = [math.inf for _ in range(n)]

  def solution():
    start = k
    queue = collections.deque()
    dist[k-1] = 0  # 시작 노드의 dist값은 0
    queue.append(start)  # 큐에 시작 정점 삽입

    while queue:
      cur = queue.popleft()
      for w in dic[cur]:
        next = w[0]
        weight = w[1]

        queue.append(next)
        
        if dist[next-1] > dist[cur-1] + weight:
          dist[next-1] = dist[cur-1] + weight


  def compare(max):
    for d in dist:
      if max < d:
        max = d
    return max


  # 그래프 형성(출발지, 도착지, 소요 시간)
  for u,v,w in times:
    dic[u] += [[v,w]]  # {2: [[1, 1], [3, 1]], 3: [[4, 1]]}

  solution()
  max = 0  # 최대 거리
  return compare(max)


times = [[2,1,1],[2,3,1],[3,4,1]]
N = 4  # 노드 개수
K = 2  # 출발 노드
print(delay(times,N,K))
```
<br><br>

### 문제 40 네트워크 딜레이 타임 풀이
#### 풀이1. 다익스트라 알고리즘 구현
이 문제에서는 다음과 같은 2가지 사항을 판별해야 한다.<br>

1. 모든 노드가 신호를 받는 데 걸리는 시간
2. 모든 노드에 도달할 수 있는지 여부

우선 첫 번째로 판별해야 하는, 모든 노드가 신호를 받는 데 걸리는 시간이란, 가장 오래 걸리는 노드까지의 시간이라 할 수 있다.<br>
즉 가장 오래 걸리는 노드까지의 최단 시간을 말하며, 이는 앞서 설명한 다익스트라 알고리즘으로 추출할 수 있다.<br>
여기서는 다익스트라 알고리즘을 직접 구현해본다.

두 번째로 모든 노드에 도달할 수 있는지 여부다.<br>
이는 모든 노드의 다익스트라 알고리즘 계산 값이 존재하는지 유무로 판별할 수 있다.<br>
만약 노드가 8개인데 다익스트라 알고리즘 계산은 7개밖에 할 수 없다면, 나머지 한 노드에는 도달할 수 없다는 의미다.<br>
이 경우 모든 노드에 도달할 수 없는 경우이므로 -1을 리턴한다.

다익스트라 알고리즘을 좀 더 효율적으로 구현하기 위해 우선순위 큐를 적용하는 방식을 사용한다.<br>
여기서는 구체적으로 파이썬에서 우선순위 큐를 최소 힙(Min Heap)으로 구현한 모듈인 heapq를 사용하는 형태로 구현해본다.<br>
일단 이 알고리즘을 모른다고 가정하고, 위키피디아에 공개되어 있는, 우선순위 큐를 이용한 다익스트라 알고리즘 수도코드(리스트 13-1)를 살펴보자.<br>
그리고 이와 동일한 알고리즘으로 실제로 실행되는 파이썬 코드로 구현해보자.<br>
이러한 연습들은 향후 실무에서 연구논문 등을 실제 코드로 구현하는 데에도 많은 도움이 될 것이다.
```
function Dijkstra(Graph, source):
    dist[source] ← 0
    
    create vertex priority queue Q
    
    for each vertex v in Graph:
        if != source
            dist[v] ← INFINITY
            prev[v] ← UNDEFINED
            
        Q.add_with_priority(v, dist[v])
        
    while Q is not empty:
        u ← Q.extract_min()
        for each neighbor v of u:
            alt ← dist[u] + length(u, v)
            if alt < dist[v]
                dist[v] ← alt
                prev[v] ← u
                Q.decrease_priority(v, alt)
                
    return dist, prev
```
그래프에서 각 정점과 거리를 우선순위 큐에 삽입하는 부분 Q.add_with_priority(v, dist[v]), 그리고 우선순위 큐에서 최소 값 추출(u ← Q.extract_min())을 통해 이웃을 살펴보는(for each neighbor v of u) 수도코드를 확인할 수 있다. <br>
이제 이 수도코드 알고리즘을 실제로 실행 가능한 파이썬 코드로 구현해보자.<br>
먼저, 다음과 같이 그래프부터 구성한다.
```python
graph = collections.defaultdict(list)
for u, v, w in times:
    graph[u].append((v,w))
```
위키피디아의 수도코드는 이미 그래프가 구성된 상태라 가정하고 입력값을 받았지만, 이 문제에서 입력값은 (u, v, w) 아이템 목록으로 구성된 리스트 형태며, 이를 키/값 구조로 조회할 수 있는 그래프 구조(파이썬에서는 딕셔너리로 구현하는 인접 리스트)는 아니다.<br>
따라서 먼저 그래프를 인접 리스트로 표현하는 딕셔너리부터 만들어준다.<br>
딕셔너리 생성을 쉽게 하기 위해 존재 유무를 판별하지 않아도 항상 디폴트로 생성해주는 collections.defaultdict를 활용했다.<br>
다음은 우선순위 큐를 위한 큐 변수를 정의한다.
```python
Q = [(0, K)]
dist = collections.defaultdict(int)
```
큐 변수 Q는 '(소요 시간, 정점)' 구조로 구성한다.<br>
즉 시작점에서 '정점'까지의 소요 시간을 담아둘 것이다.<br>
초깃값은 시작점 K부터이므로, 소요 시간은 0이고, 따라서 (0, K)가 된다.<br>
거리를 의미하는 dist 변수는 아직 아무것도 셋팅하지 않는다.<br>
수도코드를 그대로 구현한다고 했지만 dist의 초깃값을 무한대로 모두 설정해두는 것과 달리, 우리의 파이썬 구현은 조금 다르다.<br>
이는 뒤에서 다시 언급하겠지만 우선순위 큐 구현에 사용한 heapq 모듈의 기능상 제약 때문이다.

수도코드에는 큐에 add_with_priority(), decrease_priority(), extract_min() 3번의 연산을 내리도록 구현되어 있다.<br>
여기서 add_with_priority()와 extract_min()은 문제 없지만 decrease_priority()가 문제다.<br>
이 연산은 수도코드에서 우선순위를 조정하라는 의미로 쓰였는데, 우리가 사용하는 heapq 모듈은 우선순위 업데이트를 지원하지 않기 때문이다.<br>
우선순위를 업데이트하려면 먼저 해당되는 키를 찾아야 하고 이는 결국 O(n) 시간 복잡도가 필요한 일로, heapq 모듈은 이 기능을 지원하지 않는다.<br>
따라서 여기서는 수도코드를 최대한 비슷하게 구현하되, decrease_priority() 연산이 필요 없도록 다음과 같이 알고리즘을 살짝 변경해 구현해본다.
```python
while Q:
    time, node = heapq.heappop(Q)
    if node not in dist:
        dist[node] = time
        for v, w in graph[node]:
            alt = time + w
            heapq.heappush(Q, (alt, v))
```
큐 순회를 시작하자마자 최솟값을 추출하는 건 수도코드와 동일하다.<br>
그러나 바로 for each 구문으로 이웃 노드를 순회했던 수도코드와 달리, 그 작업은 잠시 뒤에 진행하고, 먼저 dist에 node의 포함 여부부터 체크한다.<br>
수도코드는 dist를 이미 무한대 값으로 채우고 시작했지만, 우리는 dist에 아무 값도 셋팅하지 않았기 때문에 처음에는 이 값이 비어 있을 것이고, 이 경우에만 힙에 푸시(Push)하는 형태로 조금 다르게 구현할 것이다.<br>
이렇게 하면 큐의 우선순위를 업데이트할 필요 없이 존재 유무로만 진행할 수 있으며, dist에는 항상 최솟값이 셋팅될 것이다.<br>
정말로 그런지 그림으로 나타내 확인해보자.

이 문제의 예제 입력값은 너무 단순하기 때문에 좀 더 복잡한 입력값 [[3,1,5],[3,2,2],[2,1,2],[3,4,1],[4,5,1],[5,6,1],[6,7,1],[7,8,1],[8,1,1]] 정도로 해서 그림으로 나타내보자.<br>
여기서 N = 8이고 K = 3이다.<br>
지금까지의 파이썬 코드를 이용해 진행한 과정은 그림 13-1(376p)에서 볼 수 있다.

> 그림 13-1

그림 13-1의 1)은 입력값을 그래프로 표현한 것이다.<br>
시작점인 3을 중심으로 세 방향으로 뻗어나가는 구조다.<br>
2)의 Q와 dist는 위에서부터 차례로, 변수에 삽입되는 순서대로 값을 나열했다.<br>
여기서 Q는 우선순위 큐이므로 값이 계속 쌓이다가 낮은 값부터 하나씩 추출되면서(최소힙이다) 제거된다.<br>
dist에 존재하지 않는다면 바로 dist값으로 time과 node의 순서가 바뀌면서 입력되고, 이미 dist에 키가 존재한다면 그 값은 버리게 된다.<br>
앞서 설명한 것처럼 dist에는 항상 최솟값부터 셋팅되기 때문에 이미 값이 존재한다면 그 값은 이미 최단 경로이고, 새롭게 큐에서 삽입되는 값은 더 오래 걸리는 경로이기 때문이다.<br>
따라서 이 값은 버리게 된다.<br>
이 그림의 2)에서는 [5,1],[6,1] 두 값이 버려지게 된다.

실제로 노드 1은 3->2->1 순서로 방문하면 소요 시간 4에 도달할 수 있으며, 이후에 삽입되려고 하는 5와 6은 모두 이보다 더 오래 걸리는 경로다.<br>
따라서 가장 먼저 입력되었던 경로가 최단 경로이며, 이후에 삽입되는 경로는 이보다 더 오래 걸리므로 모두 버린다.<br>
마지막으로 모든 노드에 도달할 수 있는지 여부를 다음과 같이 판별한다.
```python
if len(dist) == N:
    return max(dist.values())
return -1
```
여기서는 dist 딕셔너리의 키 개수가 N과 동일한지 체크한다.<br>
전체 노드 개수만큼이 모두 dist에 있다면 모든 노드의 최단 경로를 구했다는 의미고, 이는 모두 시작점에서 도달이 가능하다는 의미기도 하다.<br>
만약 노드 개수가 하나라도 모자란다면 그 노드는 시작점에서 도다할 수 없다는 뜻이며, 앞서 2가지 판별 조건 중 두 번째 조건으 만족할 수 없기때문에 -1을 리턴한다.<br>
모든 노드가 신호를 받을 수 없기 때문이다.

이제 전체 코드를 정리하면 다음과 같다.
```python
def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
    graph = collections.defaultdict(list)
    # 그래프 인접 리스트 구성
    for u, v, w in times:
        graph[u].append((v, w))
        
    # 큐 변수: [(소요 시간, 정점)]
    Q = [(0, K)]
    dist = collections.defaultdict(int)
    
    # 우선순위 큐 최솟값 기준으로 정점까지 최단 경로 삽입
    while Q:
        time, node = heapq.heappop(Q)
        if node not in dist:
            dist[node] = time
            for v, w in graph[node]:
                alt = time + w
                heapq.heappush(Q, (alt, v))
                
    # 모든 노드의 최단 경로 존재 여부 판별
    if len(dist) == N:
        return max(dist.values())
    return -1
```
위키피디아의 수도코드를 참고해서 파이썬에 적합하도록 알고리즘을 살짝 변경하여 효율적으로 다익스트라 알고리즘 구현을 마무리해봤다.<br>
참고로 수도코드의 변수 중 prev[u]는 이전 노드의 값이다.<br>
이 문제는 이전 노드가 무엇인지 여부는 전혀 필요하지 않기 때문에 구현에서 제외했다.
<br><br>

### 문제 41 K 경유지 내 가장 저렴한 항공권
> 379p

* **내가 짠 코드**<br>
```python
import collections, math

def flights(n, edges, start, last, k):
  dist = [math.inf for _ in range(n)]
  dic = collections.defaultdict(list)

  def solution():
    dist[start] = 0  # 시작노드의 dist값은 0
    cnt = -1  # 거치는 경유지 개수를 셈
    queue = collections.deque()
    queue.append(start)

    while queue:
        cur = queue.popleft()
        cnt += 1
        for v in dic[cur]:
            next = v[0]
            weight = v[1]
            queue.append(next)
            
            if dist[next] > dist[cur] + weight and cnt <= k:
                dist[next] = dist[cur] + weight


  def compare():
    for d in dist:
      if d == math.inf:
        return -1
    return max(dist)


  # 그래프 구현하기
  for u,v,w in edges:
    dic[u].append([v,w])  # {0: [[1, 100], [2, 500]], 1: [[2, 100]]}

  solution()
  return compare()


n = 3  # 노드 개수
edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0  # 시작 노드
dst = 2  # 종료 노드
k = 0  # 거칠 수 있는 경유지 수
print(flights(n, edges, src, dst, k))
```
<br><br>

### 문제 41 K 경유지 내 가장 저렴한 항공권 풀이
#### 풀이1. 다익스트라 알고리즘 응용
이 문제 또한 다익스트라 알고리즘을 응용한 풀이를 연습할 수 있는 좋은 문제다.<br>
가격을 시간이라고 가정한다면 최단 시간을 계산하는 경로는 앞서 다익스트라 알고리즘으로 동일하게 구현할 수 있다.<br>
다만, 여기에는 한 가지 제약사항이 추가되었는데 K개의 경유지 이내에 도착해야 한다는 점이다.<br>
따라서 다익스트라 알고리즘의 구현을 위해 우선순위 큐에 추가할 때 K 이내일 때만 경로를 추가하여 K를 넘어서는 경로는 더 이상 탐색되지 않게 하면 될 것 같다.<br>
앞서 풀이했던 다익스트라 알고리즘 코드를 그래도 가져와보자.
```python
def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
    graph = collections.defaultdict(list)
    for u, v, w in times:
        graph[u].append((v, w))
        
    Q = [(0, K)]
    dist = collections.defaultdict(int)
    
    while Q:
        time, node = heapq.heappop(Q)
        if node not in dist:
            dist[node] = time
            for v, w in graph[node]:
                alt = time + w
                heapq.heappush(Q, (alt, v))
                
    if len(dist) == N:
        return max(dist.values())
    return -1
```
이 문제 또한 다익스트라 알고리즘으로 풀이할 것이므로, 앞서 구현했던 다익스트라 알고리즘 풀이에서 문제도 맞도록 살짝 변형해서 풀이를 시도해볼 것이다.
```python
def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
    graph = collections.defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
        
    Q = [(0, K)]
    
    while Q:
        price, node = heapq.heappop(Q)
        ...
        for v, w in graph[node]:
                alt = price + w
                heapq.heappush(Q, (alt, v))
```
우선 networkDelayTime()이라는 함수 명부터 이 문제에 맞게 findCheapestPrice()로 교체하고, time이라는 변수 명도 문제에 맞게 price로 변경했다.<br>
기존에는 시간을 구하므로 time이었다면 여기서는 최저가를 계산해야 하므로 price가 된다.<br>
또한 더 이상 전체 거리를 보관할 필요가 없기 때문에, dist 딕셔너리는 삭제했다.<br>
도착점까지의 최단 경로만 계산하면 된다.<br>
마지막으로, 전체 경로의 개수도 체크할 필요가 없기 때문에 여기서는 모두 삭제했다.<br>
이제 남은 코드에서 문제 풀이를 위한 몇몇 알고리즘을 추가해보면 다음과 같다.
```python
def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
    graph = collections.defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    
    k = 0
    Q = [(0, K)]
    
    while Q:
        price, node, k = heapq.heappop(Q)
        if node == dst:
            return price
        if k <= K:
            k += 1
            for v, w in graph[node]:
                alt = price + w
                heapq.heappush(Q, (alt, v))
    return -1
```
우선순위 큐에는 경유 횟수를 k로 설정하여 0부터 함께 기입한다.<br>
이전 문제에서는 dist에 노드가 존재하는지 여부로 판별했으나 여기서는 K 이내일 때만(k <= K) 우선순위 큐에 경로를 추가하고 K를 넘어서는 경로는 더 이상 탐색되지 않도록 'if k <= K:' 부분을 추가했다.<br>
계속 탐색하다가 현재 노드가 도착지라면, 결과를 리턴하고 종료한다.<br>
그러나 큐를 끝까지 순회해도 찾지 못한다면 도착지까지 K 이내에 도달하는 경로는 존재하지 않는다는 얘기이므로 이 경우에는 -1을 리턴한다.

k가 K를 넘어서게 된다면 더 이상 큐에 추가하는 일도 없다.<br>
따라서 K 값이 작을수록 빠르게 실행되고 탐색 또한 금방 종료할 것이다.<br>
그런데 여기서 한 가지, 변수명을 k와 K로 너무 비슷하게 설정해서 다소 혼동이 된다.<br>
if k <= K: 라는 비교 구문도 혼란스럽다.<br>
이 부분을 혼동이 덜 되도록 좀 더 직관적으로, 다음과 같이 개선해보자.
```python
Q = [(0, src, K)]
...
if k <= 0:
    for v, w in graph[node]:
        alt = price + w
        heapq.heappush(Q, (alt, v, k - 1))
```
처음부터 입력값의 최대 경우지 값인 K를 우선순위 큐에 추가하고 경유지가 하나씩 늘 때마다 k - 1 하는 형태로 변경해봤다.<br>
이렇게 하면 비교 구문도 if k >= 0: 와 같이 훨씬 더 직관적으로 처리할 수 있으며, K를 미리 선언할 필요도 없기 때문에 훨씬 더 알아보기 쉬운 간결한 코드가 됐다.<br>
이제 전체 코드는 다음과 같다.
```python
def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
    graph = collections.defaultdict(list)
    # 그래프 인접 리스트 구성
    for u, v, w in flights:
        graph[u].append((v, w))
    
    # 큐 변수: [(가격, 정점, 남은 가능 경유지 수)]
    Q = [(0, src, K)]
    
    # 우선순위 큐 최솟값 기준으로 도착점까지 최소 비용 판별
    while Q:
        price, node, k = heapq.heappop(Q)
        if node == dst:
            return price
        if k >= 0:
            for v, w in graph[node]:
                alt = price + w
                heapq.heappush(Q, (alt, v, k - 1))
    return -1
```
<br><br>


























