# 목차
* [chapter 12. 그래프](#12장-그래프)
* [리트코드](#리트코드)
  + [문제 37 부분 집합](#문제-37-부분-집합)
  + [문제 37 부분 집합 풀이](#문제-37-부분-집합-풀이)
    - [풀이1. 트리의 모든 DFS 결과](#풀이1-트리의-모든-dfs-결과)
  + [문제 38 일정 재구성](#문제-38-일정-재구성)
  + [문제 38 일정 재구성 풀이](#문제-38-일정-재구성-풀이)
    - [풀이1. DFS로 일정 그래프 구성](#풀이1-dfs로-일정-그래프-구성)
    - [풀이2. 스택 연산으로 큐 연산 최적화 시도](#풀이2-스택-연산으로-큐-연산-최적화-시도)
    - [풀이3. 일정 그래프 반복](#풀이3-일정-그래프-반복)
  + [문제 39 코스 스케줄](#문제-39-코스-스케줄)
  + [문제 39 코스 스케줄 풀이](#문제-39-코스-스케줄-풀이)
    - [풀이1. DFS로 순환 구조 판별](#풀이1-dfs로-순환-구조-판별)
      - [list(graph)로 for문을 도는 이유](#listgraph로-for문을-도는-이유)
    - [풀이2. 가지치기를 이용한 최적화](#풀이2-가지치기를-이용한-최적화)
    - [파이썬. defaultdict 순회 문제](#파이썬-defaultdict-순회-문제)
<br><br><br>

# 12장 그래프
## 리트코드
### 문제 37 부분 집합
> 355p

* **내가 짠 코드**<br>
```python
def subset(nums):
  result = []

  def dfs(chosen,cnt):
    result.append(chosen[:])

    for i in range(cnt, len(nums)):
      chosen.append(nums[i])
      dfs(chosen,i+1)
      chosen.pop()
  
  dfs([],0)
  return result

nums = [1,2,3]
print(subset(nums))
```
조합을 구현하여 풀었다.
<br><br>

### 문제 37 부분 집합 풀이
#### 풀이1. 트리의 모든 DFS 결과
이 문제는 다음 그림 12-16(355p)과 같이 트리를 구성하고 트리를 DFS하는 문제로 풀이할 수 있다.

> 그림 12-16 부분 집합 트리

입력값이 [1,2,3] 일 때 그림 12-16(355p)과 같은 간단한 트리를 구성한다면 이를 DFS하여 원하는 결과를 출력할 수 있다.

코드는 다음과 같다.
```python
def dfs(index, path):
    ...
    for i in range(index, len(nums)):
        dfs(i + 1, path + [nums[i]])
```
경로 path를 만들어 나가면서 인덱스를 1씩 증가하는 형태로 깊이 탐색한다.<br>
별도의 종료 조건 없이 탐색이 끝나면 저절로 함수가 종료되게 한다.<br>
그렇다면 결과는 언제 만들게 될까?<br>
곰곰이 생각해보면 부분 집합은 모든 탐색의 경로가 결국 정답이 되므로, 다음과 같이 탐색할 때마다 매번 결과를 추가하면 된다.
```python
def dfs(index, path):
    ...
    result.append(path)
```
<br>

모두 정리하면 전체 코드는 다음과 같다.
```python
def subsets(self, nums: List[int]) -> List[List[int]]:
    result = []
    
    def dfs(index, path):
        # 매번 결과 추가
        result.append(path)
        
        # 경로를 만들면서 DFS
        for i in range(index, len(nums)):
            dfs(i + 1, path + [nums[i]])
            
    dfs(0, [])
    return result
```
<br><br>

### 문제 38 일정 재구성
> 357p

* [from, to]로 구성된 항공권 목록을 이용해 JFK에서 출발하는 여행 일정을 구성하라. 여러 일정이 있는 경우 사전 어휘 순으로 방문한다.<br><br>
* **내가 짠 코드**<br>
사전 어휘 순으로 방문하는 것까진 구현하지 못했다.
```python
def reconstruct(flight):
  result = []
  visited = [0 for _ in range(len(flight))]
  
  def dfs(visited,fromto):
    result.append(fromto)  # 방문한 장소 표시

    for i in range(len(flight)):
      if flight[i][0] == fromto and visited[i] == 0:
        visited[i] = 1  # 방문 표시
        dfs(visited, flight[i][1])

  dfs(visited,"JFK")
  return result

# flight = [["MUC", "LHR"],["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
flight = [["JFK", "SFO"],["JFK", "ATL"], ["SFO", "ATL"], ["ATL", "JFK"],["ATL", "SFO"]]
print(reconstruct(flight))
```
<br><br>

### 문제 38 일정 재구성 풀이
#### 풀이1. DFS로 일정 그래프 구성
여행 일정을 그래프로 구성하면 DFS로 문제를 풀이할 수 있다.<br>
여기서 한 가지 주의할 점은, 중복된 일정인 경우 어휘 순으로 방문한다는 점이다.<br>
예를 들어 ["ATL", "JFK"], ["ATL", "SFO"]인 경우 J가 S보다 빠르므로 JFK를 먼저 방문한다.

먼저, 그래프를 구성하는 작업이 필요하다. 다음과 같이 구현할 수 있다.
```python
graph = collections.defaultdict(list)
for a, b in tickets:
    graph[a].append(b)
for a in graph:
    graph[a].sort()
```
어휘 순으로 방문해야 하기 때문에 일단 그래프를 구성한 후에 다시 꺼내 정렬하는 방식을 택했다.<br>
이 경우 두 번째 예제 입력값 [["JFK", "SFO"],["JFK", "ATL"], ["SFO", "ATL"], ["ATL", "JFK"],["ATL", "SFO"]]일 때 그래프 형태는 다음과 같다.
```python
defaultdict(list,
            {'ATL': ['JFK', 'SFO'], 'JFK': ['ATL', 'SFO'], 'SFO': ['ATL']})
```
이처럼 그래프를 순서대로 구성하기 위해 매번 sort()를 호출했다.<br>
그러나 이렇게 매번 하지 않고 애초에 tickets를 한 번만 정렬해도 결과는 동일하다.<br>
파이썬의 sorted() 함수로 딱 한 번만 정렬하는 형태로 다음과 같이 개선해보자.
```python
graph = collections.defaultdict(list)
for a, b in sorted(tickets):
    graph[a].append(b)
```
처음에 비해 for 문도 한 번으로 줄고 코드도 많이 깔끔해졌다.<br>
이제 그래프에서 하나씩 꺼낼 차례다.<br>
pop()으로 재귀 호출하면서 모두 꺼내 결과에 추가한다.<br>
결과 리스트에는 역순으로 담기게 될 것이며, pop()으로 처리했기 때문에 그래프에서는 해당 경로는 사라지게 되어 재방문하게 되지는 않을 것이다.
```python
def dfs(a):
    while graph[a]:
        dfs(graph[a].pop(0))
    route.append(a)
```
앞서도 강조했지만 여기서 중요한 점은 어휘 순으로 방문해야 한다는 점이다.<br>
따라서 어휘 순으로 그래프를 생성했기 때문에 pop(0)으로 첫 번째 값을 읽어야 한다.<br>
굳이 따지자면 큐의 연산을 수행한다.<br>
마지막에는 다시 뒤집어서 어휘 순으로 맨 처음 읽어들였던 값이 처음이 되게 한다.

이제 전체 코드는 다음과 같다.
```python
def findItinerary(self, tickets: List[List[str]]) -> List[str]:
    graph = collections.defaultdict(list)
    # 그래프 순서대로 구성
    for a, b in sorted(tickets):
        graph[a].append(b)
        
    route = []
    def dfs(a):
        # 첫 번째 값을 읽어 어휘 순 방문
        while graph[a]:
            dfs(graph[a].pop(0))
        route.append(a)
        
    dfs('JFK')
    # 다시 뒤집어 어휘 순 결과로
    return route[::-1]
```
<br><br>

#### 풀이2. 스택 연산으로 큐 연산 최적화 시도
앞서 풀이에서 큐의 연산을 수행한다고 언급한 바 있다.<br>
파이썬 리스트의 경우 파라미터를 지정하지 않은 값, 그러니까 마지막 값을 꺼내는 pop()은 O(1)이지만, 첫 번째 값을 꺼내는 pop(0)은 O(n)이다.<br>
따라서 좀 더 효율적인 구현을 위해서는 pop()으로, 즉 스택의 연산으로 처리될 수 있도록 개선이 필요하다.
```python
for a, b in sorted(tickets, reverse=True):
    graph[a].append(b)
    
...
def dfs(a):
    while graph[a]:
        dfs(graph[a].pop())
    ...
```
이처럼 애당초 그래프를 역순으로 구성하면 추출 시에는 pop()으로 처리가 가능하다.<br>
이렇게 하면 추출 시 시간 복잡도를 O(1)으로 개선할 수 있다.<br>

전체 코드는 다음과 같다.
```python
def findItinerary(self, tickets: List[List[str]]) -> List[str]:
    graph = collections.defaultdict(list)
    # 그래프를 뒤집어서 구성
    for a, b in sorted(tickets, reverse=True):
        graph[a].append(b)
        
    route = []
    def dfs(a):
        # 마지막 값을 읽어 어휘 순 방문
        while graph[a]:
            dfs(graph[a].pop())
        route.append(a)
        
    dfs('JFK')
    # 다시 뒤집어 어휘 순 결과로
    return route[::-1]
```
<br><br>

#### 풀이3. 일정 그래프 반복
이번에는 재귀가 아닌 동일한 구조를 반복으로 풀이해본다.<br>
대부분의 재귀 문제는 반복으로 치환할 수 있으며, 스택으로 풀이가 가능하다.<br>
먼저, 다음과 같이 그래프를 구성하는 부분은 동일하다.
```python
graph = collections.defaultdict(list)
for a, b in sorted(tickets):
    graph[a].append(b)
```
이처럼 동일한 형태로 그래프를 구성하되, 끄집어낼 때 재귀가 아닌 스택을 이용한 반복으로 처리한다.<br>
두 번째 예제의 입력값을 그래프로 구성한 형태 또한 다음과 같이 동일하다.
```python
defaultdict(list,
            {'ATL': ['JFK', 'SFO'], 'JFK': ['ATL', 'SFO'], 'SFO': ['ATL']})
```
이제 하나씩 추출하는 형태를 도식화해보면 다음 그림 12-17(361p)의 순서로 진행된다.

> 그림 12-17

출발지는 항상 JFK이므로, 가장 먼저 JFK가 오게 되고, 다음은 ATL이 어휘순으로 먼저 방문한다.<br>
ATL에서 어휘순 첫 번째 위치는 JFK이며, 다시 JFK로 돌아갔을 때 지난번에 이미 ATL을 방문했기 때문에 다음 방문지인 SFO를 가게 된다.

한번 방문했던 곳을 다시 방문하지 않으려면, 별도로 마킹하여 다음번에 방문하지 않거나 아예 스택의 pop()으로 값을 제거하는 방법이 있는데, 여기서는 스택을 이용하므로 다음과 같이 pop()으로 아예 값을 제거한다.
```python
stack = ['JFK']
while stack:
    while graph[stack[-1]]:
        stack.append(graph[stack[-1]].pop(0))
```
그래프에 값이 있다면 pop(0)으로 맨 처음의 값을 추출하여 스택 변수 stack에 넣게 했다.<br>
큐의 연산이다. 이렇게 하면 그래프의 값들은 점점 제거될 것이며 마지막 방문지가 남지 않을 때까지 while 문이 계속 돌면서 그림 12-17(361p)과 같은 순서대로 처리가 된다.

이 예제는 경로가 끊어지는 경우가 없기 때문에 스택에 모든 경로가 한 번에 담긴다.<br>
그런데 만약 경로가 끊어지는 경우가 있다면 스택에 모든 경로가 한 번에 담길 수 없다.<br>
예를 들어 입력값이 [["JFK", "KUL"], ["JFK", "NRT"], ["NRT", "JFK"]]라면 그래프는 다음과 같은 형태가 된다.
```python
defaultdict(list, {'JFK': ['KUL', 'NRT'], 'NRT': ['JFK']})
```
이 경우 JFK → KUL에서 더 이상 진행할 수 없게 된다.<br>
따라서 스택의 값을 다시 pop()하여 거꾸로 풀어낼 수 있는 또 다른 변수가 필요하다.
```python
route, stack = [], ['JFK']
while stack:
    while graph[stack[-1]]:
        stack.append(graph[stack[-1]].pop(0))
    route.append(stack.pop())
```
DFS 재귀 풀이와 달리 반복으로 풀이하려면 이처럼 한 번 더 풀어낼 수 있는 변수가 필요하다.<br>
여기서는 최종 결과가 되는 변수이므로 DFS 재귀 풀이와 동일한 route로 변수명을 정했고, 여기에 스택을 pop()하여 담게 했다.<br>
즉 경로가 풀리면서 거꾸로 담기게 될 것이다.<br>
따라서 마찬가지로, 최종 결과는 다시 뒤집어야 한다.<br>
어쨌든 앞서 막혔던 부분인 JFK → KUL에서 결과 변수 route에 pop()하여 KUL부터 담게 되고, JFK에서 다시 탐색을 시도해 NRT를 탐색하게 된다.<br>
NRT는 JFK로 이동하므로, 최종 결과는 다시 뒤집어서 JFK → NRT → JFK → KUL이 된다.

이제 전체 코드는 다음과 같다.
```python
def findItinerary(self, tickets: List[List[str]]) -> List[str]:
    graph = collections.defaultdict(list)
    # 그래프를 뒤집어서 구성
    for a, b in sorted(tickets):
        graph[a].append(b)
        
    route, stack = [], ['JFK']
    while stack:
        # 반복으로 스택을 구성하되 막히는 부분에서 풀어내는 처리
        while graph[stack[-1]]:
            stack.append(graph[stack[-1]].pop(0))
        route.append(stack.pop())
        
    # 디시 뒤집어 어휘 순 결과로
    return route[::-1]
```
<br><br>

---

이 문제의 경우 재귀, 스택 연산, 반복 모두 속도 차이가 거의 없다.<br>
이 중에서도 재귀에 반복에 집중해서 좀 더 살펴보자면, 재귀는 좀 더 깔끔하게 풀이가 가능하고, 반복은 의식의 흐름대로 자연스럽게 풀이가 가능하다는 차이가 있다.<br>
어느 쪽이든 좋은 풀이 방법이므로, 본인이 선호하는 방식으로 풀이하면 된다.
<br><br>

### 문제 39 코스 스케줄
> 364p

* 0을 완료하기 위해서는 1을 끝내야 한다는 것은 [0,1] 쌍으로 표현하는 n개의 코스가 있다. 코스 개수 n과 이 쌍들을 입력으로 받았을 때 모든 코스가 완료 가능한지 판별하라.<br><br>
* **내가 짠 코드**<br>
```python

```
<br><br>

### 문제 39 코스 스케줄 풀이
#### 풀이1. DFS로 순환 구조 판별
이 문제는 그래프가 순환(Cycle) 구조인지를 판별하는 문제로 풀이할 수 있다.<br>
순환 구조라면 계속 뱅글뱅글 맴돌게 될 것이고, 해당 코스는 처리할 수 없기 때문이다.<br>
따라서 순환 판별 알고리즘을 차례대로 구현해보자.
```python
graph = collections.defaultdict(list)
for x, y in prerequisites:
    graph[x].append(y)
```
먼저 여기서는 페어들의 목록인 prerequisites 변수를 풀어서 그래프로 표현한다.<br>
페어의 첫 번째 값을 x, 두 번째 값을 y로 하되 y는 복수 개로 구성할 수 있게 한다.<br>
이렇게 하면 'x': ['y1', 'y2'] 같은 구조가 될 것이다.
```python
traced = set()
...
for x in graph:
    if not dfs(x):
        return False
        
return True
```
순환 구조를 판별하기 위해 앞서 이미 방문했던 노드를 traced 변수에 저장한다.<br>
이미 방문했던 곳을 중복 방문하게 된다면 순환 구조로 간주할 수 있고, 이 경우 False를 리턴하고 종료할 수 있다.<br>
traced는 중복 값을 갖지 않으므로 set() 집합 자료형으로 정한다.<br>
아울러 순환 구조를 찾기 위한 탐색은 다음과 같이 DFS로 진행한다.
```python
def dfs(i):
    if i in traced:
        return False
        
    traced.add(i)
    for y in graph[i]:
        if not dfs(y):
            return False
    traced.remove(i)
    
    return True
```
여기서 DFS 함수인 dfs()에서는 현재 노드가 이미 방문했던 노드 집합인 traced에 존재한다면 순환 구조이므로 False를 리턴한다.<br>
False는 계속 상위로 리턴되어 최종 결과도 False를 리턴하게 될 것이다.<br>
탐색은 재귀로 진행하되, 해당 노드를 이용한 모든 탐색이 끝나게 된다면 traced.remove(i)로 방문했던 내역을 반드시 삭제해야 한다.<br>
그렇지 않으면 형제 노드가 방문한 노드까지 남게 되어, 자식 노드 입장에서는 그림 12-18(366p)과 같이 순환이 아닌데 순환이라고 잘못 판단할 수 있기 때문이다.

> 그림 12-18

이런 그림이 바로 잘못 판단하는 경우인데, 여기서는 입력값이 [[0,1], [0,2], [1,2]]일 때 이 그래프는 순환이 존재하지 않으므로 결과는 True가 되어야 한다.<br>
그러나 만약 이미 방문한 노드를 삭제하지 않고 그대로 둔다면 1)번 경로로 탐색할 때 방문했던 노드들이 그대로 남게 될 것이고(즉 [0, 1, 2]) 2)번 경로로 탐색할 때 2 노드가 이미 방문했던 노드라고 잘못 판단할 것이다.<br>
1)번 경로와 2)번 경로는 형제 노드 간의 탐색이기 때문에 서로 관련성이 없어야 하므로 1)번 경로로 탐색이 끝난 이후에는 traced.remove(i)로 1, 2 노드가 모두 삭제되어야 한다.

이제 전체 코드는 다음과 같다. 920밀리초에 풀린다.
```python
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
    graph = collections.defaultdict(list)
    # 그래프 구성
    for x, y in prerequisites:
        graph[x].append(y)
        
    traced = set()
    
    def dfs(i):
        # 순환 구조이면 False
        if i in traced:
            return False

        traced.add(i)
        for y in graph[i]:
            if not dfs(y):
                return False
        # 탐색 종료 후 순환 노드 삭제
        traced.remove(i)

        return True
        
    # 순환 구조 판별
    for x in list(graph):
        if not dfs(x):
            return False

    return True
```
<br>

#### list(graph)로 for문을 도는 이유
순환구조를 판별할 때 list(graph)로 for문을 돈다.
```python
for x in list(graph):
    if not dfs(x):
        return False
```
딕셔너리 자체인 graph로 for문을 돌지 않는 이유는 중간에 딕셔너리의 크기가 달라지면서 제대로 for문이 동작하지 않기 때문이다.

오류 내용 ↓<br>
**RuntimeError: dictionary changed size during iteration**
<br><br><br>

#### 풀이2. 가지치기를 이용한 최적화
좀 더 효율적으로 풀이할 수 있는 방법은 없을까?<br>
앞서 DFS 풀이는 순환이 발견될 때까지 모든 자식 노드를 탐색하는 구조로 되어 있다.<br>
만약 순환이 아니더라도 복잡하게 서로 호출하는 구조로 그래프가 구성되어 있다면, 불필요하게 동일한 그래프를 여러 번 탐색하게 될 수도 있다.<br>
따라서 한 번 방문했던 그래프는 두 번 이상 방문하지 않도록 무조건 True로 리턴하는 구조로 개선한다면, 탐색 시간을 획기적으로 줄일 수 있을 것이다.<br>
그리고 원래 DFS는 이런 식으로 가지치기하도록 구현하는 게 올바른 구현 방법이다.<br>
풀이1은 지나치게 곧이곧대로 구현한 면이 있다.
```python
visited = set()

def dfs(i):
    if i in traced:
        return False
    if i in visited:
        return True
        
    ...
    
    return True
```
이처럼 한 번 방문했던 노드를 저장하기 위한 visited라는 별도의 set() 집합 변수를 만든다.<br>
이미 방문했던 노드라면 다음과 같이 더 이상 진행하지 않고 True를 리턴한다.
```python
if i in visited:
    return True
```
여기서 visited는 모든 탐색이 끝난 후에 노드를 추가하는 형태로 구현한다.<br>
만약 탐색 도중 순환 구조가 발견된다면 False를 리턴하면서 visited 추가는 하지 않음은 물론, 모든 함수를 빠져나가며 종료하게 될 것이다.<br>
이렇게 한 번 방문한 노드를 더 이상 탐색하지 않는 형태, 즉 가지치기로 최적화한 풀이의 전체 코드는 다음과 같다.
```python
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
    graph = collections.defaultdict(list)
    # 그래프 구성
    for x, y in prerequisites:
        graph[x].append(y)
        
    traced = set()
    visited = set()
    
    def dfs(i):
        # 순환 구조이면 False
        if i in traced:
            return False
        # 이미 방문했던 노드이면 True
        if i in visited:
            return True
            
        traced.add(i)
        for y in graph[i]:
            if not dfs(y):
                return False
        # 탐색 종료 후 순환 노드 삭제
        traced.remove(i)
        # 탐색 종료 후 방문 노드 추가
        visited.add(i)

        return True
        
    # 순환 구조 판별
    for x in list(graph):
        if not dfs(x):
            return False

    return True
```
그렇다면 이렇게 가지치기로 최적화한 풀이의 실행 속도는 얼마나 될까?<br>
96밀리초로, 풀이1에 비해 거의 10배 더 빠른 속도로 풀이됐다.<br>
이처럼 간단한 최적화로도 매우 좋은 성능을 낼 수 있다.
<br><br>

#### 파이썬. defaultdict 순회 문제
앞서 이 39번 문제의 풀이1에서 맨 처음에 다음과 같이 그래프를 순회하면 된다고 설명한 바 있다.
```python
for x in graph:
    ...
```
그러나 전체 코드에서는. DFS 호출을 위한 for 문 코드가 조금 다른 형태로 구현되어 있다.
```python
for x in list(graph):
    ...
```
graph 변수 앞을 list()로 감쌌는데, 이는 RuntimeError: dictionary changed size during iteration 에러가 발생했기 때문이다.<br>
에러 메시지에서는, graph 딕셔너리가 순회 중에 변경됐다고 지적한다.<br>
for 구문에서 반복하는 graph 딕셔너리 변수는 최초 생성 후 변경된 적이 없는데 왜 이런 에러가 발생할까?<br>
그런데 곰곰이 살펴보면 graph를 다은과 같은 형태로 생성한 적이 있다.
```python
graph = collections.defaultdict(list)
```
collections 모듈의 defaultdict를 사용해 키가 없는 딕셔너리에 대해서 빈 값 조회시 널(NULL) 오류가 발생하지 않도록 처리한 바 있는데, 문제는 여기에 있다.<br>
여기서 사용한 defaultdict가 존재하지 않는 키를 조회할 때 오류를 내지 않기 위해 항상 디폴트를 생성하는 구조로 되어 있다는 점이다.<br>
따라서 다음과 같은 반복문에서 graph 값이 변경된다는 오류가 발생한다.
```python
for x in graph:
    ...
------------------------------------------------------------------
RuntimeError: dictionary changed size during iteration
```
해당 반복문이 제대로 실행되려면 graph 변수를 defaultdict()에서 분리해서 고정시킬 필요가 있다.<br>
이 부분은 파이썬 2와 3의 해결 방법이 다른데, 이 책의 기준인 파이썬 3.7+의 해결 방식을 보면 list()로 묶어서 해결하라는 답변을 스택오버플로에서 찾을 수 있다.<br>
즉 새로운 복사본을 만들라는 얘기다.<br>
새로운 복사본의 여부는 다음과 같이 간단히 ID 조회로 확인할 수 있다.
```python
>>> id(graph)
4511667296
>>> id(list(graph))
4511666816
```
원래 graph의 ID는 4511667296이지만 list()로 묶을 경우 새로운 복사본이 생성되면서 다른 ID를 갖게 된다.<br>
따라서 for 문에서도 다음과 같이 반복문을 수정해주면 에러 없이 정상적으로 실행된다.
```python
for x in list(graph):
    ...
```
이런 문제는 좀처럼 해결책을 찾기가 쉽지 않다.<br>
그러나 온라인 코딩 테스트에 임하기 전에는 풀이하고자 하는 언어와 컴파일러에 대해 충분히 숙지하여 이런 문제를 최소화해야 한다.<br>
적어도 파이썬으로 풀이하기로 마음먹었다면 파이썬의 버전별 특징과 파이썬의 공식 인터프리터인 CPython의 동작 원리에 대해 충분히 익혀둬야 한다.
<br><br>























