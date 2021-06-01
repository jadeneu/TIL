# 목차
* [chapter 14. 트리](#14장-트리)
* [리트코드](#리트코드)
  + [문제 48 균형 이진 트리](#문제-48-균형-이진-트리)
  + [문제 48 균형 이진 트리 풀이](#문제-48-균형-이진-트리-풀이)
    - [풀이1. 재귀 구조로 높이 차이 계산](#풀이1-재귀-구조로-높이-차이-계산)
  + [문제 49 최소 높이 트리](#문제-49-최소-높이-트리)
  + [문제 49 최소 높이 트리 풀이](#문제-49-최소-높이-트리-풀이)
    - [풀이1. 단계별 리프 노드 제거](#풀이1-단계별-리프-노드-제거)
<br><br><br>

# 14장 트리
## 리트코드
### 문제 48 균형 이진 트리
> 413p

* 이진트리가 높이 균형(Height-Balanced)인지 판단하라.<br>
  (높이 균형은 모든 노드의 서브 트리 간의 높이 차이가 1 이하인 것을 말한다.)<br>

* **예제1**<br>
    * 입력값<br>
      ```
      [3,9,20,null,null,15,7]
      ```
      
      <img src="https://user-images.githubusercontent.com/55045377/120096679-d8525a00-c167-11eb-938b-30a9b93c34ee.png" width=20% height=20%>
      
      서브 트리 간 높이 차이가 1 이하이므로 높이 균형이다. 따라서 true를 리턴한다.
      
* **예제2**<br>
    * 입력값<br>
      ```
      [1,2,2,3,3,null,null,4,4]
      ```
      
      <img src="https://user-images.githubusercontent.com/55045377/120096784-5e6ea080-c168-11eb-941a-12a6fe736bd5.png" width=20% height=20%>
      
      1의 왼쪽 서브 트리 2와 오른쪽의 2는 높이 차이가 2다. 따라서 false를 리턴한다.<br><br>
      
* **내가 짠 코드**<br>
```python
import collections

class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  def balanced(self, tree):
    if not tree:
      return True

    queue = collections.deque([tree])
    lcnt = 0
    rcnt = 0

    while queue:
      node = queue.popleft()
      if node.left and node.right:
        lcnt += 1
        rcnt += 1
        queue.append(node.left)
        queue.append(node.right)
      elif node.left and not node.right:
        lcnt += 1
        queue.append(node.left)
      elif not node.left and node.right:
        rcnt += 1
        queue.append(node.right)

    if abs(lcnt-rcnt) > 1:
      return False
    else:
      return True


solution = Solution()
n1 = TreeNode(7)
n2 = TreeNode(15)
n3 = TreeNode(20,n2,n1)
n4 = TreeNode(9)
n5 = TreeNode(3,n4,n3)
print(solution.balanced(n5))
```
구현 못함. 왼쪽, 오른쪽 서브트리의 높이를 구분지어서 구하지 못했다.
<br><br>

### 문제 48 균형 이진 트리 풀이
#### 풀이1. 재귀 구조로 높이 차이 계산
높이 균형은 매우 중요한 개념이다.<br>
균형이 맞아야 효율적으로 트리를 구성할 수 있으며, 탐색 또한 훨씬 더 효율적으로 처리할 수 있기 때문이다.<br>
높이 균형을 매번 맞추는 AVL 트리(고안자인 G.M. 아렐슨 벨스키(Adelson-Velskii)와 E.M. 랜디스(Landis)의 이 름 첫 글자를 따서 AVL(Adelsons-Velsky Landis) 트리라고 부른다)는 대표적인 자가 균형 이진 탐색 트리이기도 하다.

여기서는 높이 균형이 맞는지 여부를 판별하는 코드를 구현해본다.<br>
먼저, 다음과 같이 판별 함수 isBalanced( )의 뼈대를 만들어보자.
```python
def isBalanced(self, root: TreeNode) -> bool:
    def check(root):
        if not root:
            return 0
            
        left = check(root.left)
        right = check(root.right)
```
재귀 호출로 리프 노드까지 내려간다.<br>
맨 마지막 노드에 이르면 각각 left = 0, right = 0을 리턴하도록 구성했다.<br>
check() 함수의 비즈니스 로직은 다음과 같다.
```python
def check(root):
    ...
    if left == -1 or \
            right == -1 or \
            abs(left - right) > 1:
        return -1
    return max(left, right) + 1
```
left와 right가 모두 0이라면, 차이가 1보다 크지 않으므로 max(left , right) + 1로 l을 리턴하게 된다.<br>
이런 식으로 점점 1 씩 증가하는 형태가 리턴된다.<br>
예제에서 false로 처리된 입력값 [1,2,2,3,3,null,null,4,4]를 대상으로 표현해본 그림 14-13을 살펴보자.

<img src="https://user-images.githubusercontent.com/55045377/120130715-d0df8f00-c201-11eb-8a5c-1ef02a9d5541.png" width=35% height=35%>

그림 14-13에서 리프 노드인 4는 1, 그 부모 노드인 3은 2, 이런 식으로 높이에 따라 1씩 점점 증가하는 형태를 띤다.<br>
하지만 루트 1의 오른쪽 자식 노드인 2는 더 이상 자식이 없으므로 값은 1을 받게 된다.<br>
그 왼쪽, 즉 형제 노드는 3이므로 높이 차이가 1 을 초과하므로, -1을 리턴받는다.<br>
즉 루트는 -1이 된다. 이렇게 한번 -1이 되면 더 이상 회복이되지 않는다.

만약 부모 노드가 또 있다 해도 그 오른쪽 노드는 어떠한 경우에도 1 이상의 값을 갖게 될 것이므로 계속해서 높이차이가 1을 초과한다.<br>
때문에 마찬가지로 -1을 리턴받는다.<br>
즉 양쪽 자식 노드 중 어느 하나가 -1이 되는 경우에는 계속해서 -1을 리턴하게 되며, 각 서브트리의 높이 차이가 한 번이라도 1을 초과하는 경우 -1이 할당되며 계속해서 부모 노드로 -1을 리턴
하다 최종적으로 False를 리턴하게 된다.

전체 코드는 다음과 같다.
```python
def isBalanced(self, root: TreeNode) -> bool:
    def check(root):
        if not root:
            return 0
    
    left = check(root.left)
    right = check(root.right)
    # 높이 차이가 나는 경우 -1, 이외에는 높이에 따라 1 증가
    if left == -1 or \
            right == -1 or \
            abs(left - right) > 1:
        return -1
    return max(left, right) + 1
    
  return check(root) != -1
```
<br><br>

### 문제 49 최소 높이 트리
> 416p

* 노드개수와 무방향 그래프를 입력받아 트리가 최소 높이가 되는 루트의 목록을 리턴하라.
* 예제1
  * 입력
    ```
    n = 4, edges = [[1, 0], [1, 2], [1, 3]]
    ```
    
    <img src="https://user-images.githubusercontent.com/55045377/120139148-d2fe1980-c212-11eb-90d9-52f821ce9f33.png" width=50% height=50%>
    
  * 출력
    ```
    [1]
    ```
* 예제2
  * 입력
    ```
    n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]
    ```
    
    <img src="https://user-images.githubusercontent.com/55045377/120139231-ffb23100-c212-11eb-857f-cd9581c400ea.png" width=50% height=50%>
    
  * 출력
    ```
    [3, 4]
    ```
<br><br>
그림 참고<br>
https://leetcode.com/problems/minimum-height-trees/
<br><br>

* **내가 짠 코드**<br>
```python
import collections
from math import inf

def solution(n, edges):
  dic = collections.defaultdict(list)
  # 그래프 형성
  for pair in edges:
    dic[pair[0]] += [pair[1]]
    dic[pair[1]] += [pair[0]]

  def search(root):
    queue = collections.deque()
    queue.append(root)
    visited = [0 for _ in range(len(dic))]
    dist = [inf for _ in range(len(dic))]
    visited[root] = 1
    dist[root] = 0
    
    while queue:
      digit = queue.popleft()
      # 인접한 숫자를 큐에 삽입
      for i in dic[digit]:
        if visited[i] == 0:
          queue.append(i)
          visited[i] = 1  # 방문 표시

          if dist[i] > dist[digit]:
            dist[i] = dist[digit] + 1
          
    return dist
 
  result = []
  for key in sorted(dic):
    result.append(max(search(key)))  # 인덱스는 루트, 값은 높이

  answer = []
  for i in range(len(result)):
    if result[i] == min(result):
      answer.append(i)

  return answer

n = 6
edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]
print(solution(n,edges))
```
시간복잡도가 어마어마한 풀이인 것 같다. 정답을 알 순 있지만 개선이 필요하다.
<br><br>

### 문제 49 최소 높이 트리 풀이
#### 풀이1. 단계별 리프 노드 제거
최소 높이를 구성하려면 가장 가운데에 있는 값이 루트여야 한다.<br>
* **참고(예제1,예제2)**
  
  <img src="https://user-images.githubusercontent.com/55045377/120269246-66eae680-c2e2-11eb-9483-862ae2699358.png" width=30% height=30%> <img src="https://user-images.githubusercontent.com/55045377/120269302-808c2e00-c2e2-11eb-927e-93a7df221a21.png" width=25% height=25%>
  
이 말은 리프 노드를 하나씩 제거해 나가면서 남아 있는 값을 찾으면 이 값이 가장 가운데에 있는 값이 될 것이고, 이 값을 루트로 했을 때 최소 높이를 구성할 수있다는 뜻이기도 하다.<br>
문제에 제시된 입력값은 너무 단순하므로 [[1, 3], [2, 3], [3, 4], [3, 5], [4, 6], [6, 10], [5, 7], [5, 8], [8, 9]] 정도로해서 좀 더 복잡한 그래프를 다음과 같이 구성해보자.

<img src="https://user-images.githubusercontent.com/55045377/120268099-3013d100-c2e0-11eb-9a21-20e35328829b.png" width=25% height=25%>

입력값을 그래프로 구성해보면 그림 14-14와 같은 형태가 된다.<br>
이제 여기서부터 리프 노드를 제거해보자.<br>
가장 가운데에 있는, 가장 마지막에 남을 루트가 될 노드는 어떤 노드가 될지 한번 상상해보자.

<img src="https://user-images.githubusercontent.com/55045377/120268372-a4e70b00-c2e0-11eb-95dd-af2d909c997f.png" width=30% height=30%>

그림 14-15에서 1, 2, 7, 9, 10인 리프 노드를 한 번 제거했다. <br>
아직 몇 번 더 제거해야 될 것 같다.

<img src="https://user-images.githubusercontent.com/55045377/120269548-014b2a00-c2e3-11eb-951a-8fe9b11cbe6e.png" width=30% height=30%>

그림 14-16에서 6, 8 인 두 번째 리프 노드를 제거했다.<br>
이제 어떤 값이 남아서 루트가 될지 어렴풋이 판단할 수 있을 것 같다.

<img src="https://user-images.githubusercontent.com/55045377/120269673-3788a980-c2e3-11eb-9d16-df94da2aca3f.png" width=40% height=40%>

그림 14-17에서 리프 노드를 세 번째 제거했고 결과는 하나가 남았다.<br>
최종 결과는 3이고, 3을 루트로 했을 때 최소 높이 트리를 구성할 수 있을 것이다.<br>
이제 이 과정을 코드로 구현해보자.
```python
def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
    ...
    graph = collections.defaultdict(list)
    for i, j in edges:
        graph[i].append(j)
        graph[j].append(i)
```
이 문제에서 그래프는 무방향(Undirected)이므로, 트리의 부모와 자식은 양쪽 노드 모두 번갈아 가능하다.<br>
따라서 양쪽 모두 graph라는 이름의 그래프 덕셔너리 변수에 양방향으로 삽입하여 구성한다.
```python
leaves = []
for i in range(n + 1):
    if len(graph[i]) == 1:
        leaves.append(i)
```
리프 노드를 찾아서 leaves에 추가한다.<br>
리프 노드는 그래프에서 해당 키의 값이 1개뿐인 것을 말한다.<br>
실제로 graph의 값이 그렇게 구성되어 있는지 다음과 같이 graph의 값을 한번 출력해보자.
```python
>>> graph
defaultdict(<class 'list'>, {
    1: [3],
    3: [1, 2, 4, 5],
    2: [3],
    4: [3, 6],
    5: [3, 7, 8],
    6: [4, 10],
    10: [6],
    7: [5],
    8: [5, 9],
    9: [8]
})
```
입력값 [[1, 3], [2, 3], [3, 4], [3, 5], [4, 6], [6, 10], [5, 7], [5, 8], [8, 9]]을 기준으로 덕셔너리로 표현한 그래프의 값은 이런 형태가 되고 이 중에서 값이 1개 뿐인 [1, 2, 10, 7, 9]가 첫 번째 리프 노드로 leaves 리스트 변수에 담기게 된다.<br>
그림 14-15 에서 리프 노드를 한 번 제거한 모습과 leaves에 담긴 리프 노드들이 동일함을 확인할 수 있다.<br>
이제 다음과 같이 루트가 남을 때까지 반복해서 계속 제거한다.
```python
while n > 2:
    n -= len(leaves)
    new_leaves = []
    for leaf in leaves:
        neighbor = graph[leaf].pop()
        graph[neighbor].remove(leaf)
        
        if len(graph[neighbor]) == 1:
            new_leaves.append(neighbor)
            
    leaves = new_leaves
```
n은 전체 노드의 개수이므로 여기서 leaves, 즉 리프 노드의 개수만큼 계속 빼나가면서 최종 2개 이하가 남을 때까지 반복한다.<br>
마지막에 남은 값이 홀수 개일 때는 루트가 최종 1개가 되지만, 짝수 개일 때는 2개가 될 수 있다.<br>
따라서 while 반복문은 2개까지는 계속 반복한다.<br>
리프 노드는 반복하면서 제거한다.<br>
그래프 딕셔너리에서 pop()으로 제거하고, 연결된 값도 찾아서 제거한다.<br>
즉 무방향 그래프라 그래프를 각각 두 번씩 만들었으므로 제거 또한 두 번씩 진행한다.<br>
이제 마찬가지로 값이 1개뿐일 때는 리프 노드라는 의미이므로, 새로운 리프 노드를 new_leaves로 구성하여 교체한다.
```python
return leaves
```
계속 반복하면서 leaves에 최종적으로 2개 이하의 노드가 남게 되었을 때, 이 노드들이 루트가 되며 최종 결과로 리턴한다.<br>
전체 코드는 다음과 같다.
```python
def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
    if n <= 1:
        return [0]
    
    # 양방향 그래프 구성
    graph = collections.defaultdict(list)
    for i, j in edges:
        graph[i].append(j)
        graph[j].append(i)
        
    # 첫 번째 리프 노드 추가
    leaves = []
    for i in range(n + 1):
        if len(graph[i]) == 1:
            leaves.append(i)
            
    # 루트 노드만 남을 때까지 반복 제거
    while n > 2:
        n -= len(leaves)
        new_leaves = []
        for leaf in leaves:
            neighbor = graph[leaf].pop()
            graph[neighbor].remove(leaf)

            if len(graph[neighbor]) == 1:
                new_leaves.append(neighbor)

        leaves = new_leaves
        
    return leaves
```
<br><br>


























