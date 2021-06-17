# 목차
* [chapter 14. 트리](#14장-트리)
  + [트리의 각 명칭](#트리의-각-명칭)
  + [그래프 VS 트리](#그래프-vs-트리)
  + [이진 트리](#이진-트리)
* [리트 코드](#리트-코드)
  + [문제 42 이진 트리의 최대 깊이](#문제-42-이진-트리의-최대-깊이)
  + [문제 42 이진 트리의 최대 깊이 풀이](#문제-42-이진-트리의-최대-깊이-풀이)
    - [풀이1. 반복 구조로 BFS 풀이](#풀이1-반복-구조로-bfs-풀이)
  + [문제 43 이진 트리의 직경](#문제-43-이진-트리의-직경)
  + [문제 43 이진 트리의 직경 풀이](#문제-43-이진-트리의-직경-풀이)
    - [풀이1. 상태값 누적 트리 DFS](#풀이1-상태값-누적-트리-dfs)
    - [문법. 중첩 함수에서 클래스 변수를 사용한 이유](#문법-중첩-함수에서-클래스-변수를-사용한-이유)
<br><br><br>


# 14장 트리
> 트리는 계층형 트리 구조를 시뮬레이션 하는 추상 자료형(ADT)으로, 루트 값과 부모-자식 관계의 서브트리로 구성되며, 서로 연결된 노드의 집합이다.

트리(Tree)는 하나의 뿌리에서 위로 뻗어 나가는 형상처럼 생거서 '트리(나무)'라는 명칭이 붙었는데, 트리 구조를 표현할 때는 나무의 형상과는 반대 방향으로 표현한다.<br>
트리 구조는 우리 주변 일상에서 쉽게 볼 수 있는 위아래 개념을 컴퓨터에서 표현한 구조다.<br>
한 가족의 계보를 나타내는 족보나 회사의 조직도 등을 보면 보통 트리 구조 형태로 되어 있다.

좀 더 중요한 트리의 속성 중 하나는 **재귀로 정의된(Recursively Defined) 자기 참조(Self-Referential) 자료구조**라는 점이다.<br>
쉽게 말하면, 트리는 자식도 트리고 또 그 자식도 트리다.<br>
즉 여러 개의 트리가 쌓아 올려져 큰 트리가 된다. 흔히 서브트리(Subtrees)로 구성된다고 표현한다.<br>
이러한 재귀적 특성 때문에 441페이지에서 살펴보게 될 순회에서도 트리에서는 재귀 순회가 좀 더 자연스러운 편이다.
<br><br>

## 트리의 각 명칭
트리를 본격적으로 살펴보기에 앞서 트리 각 부분에 대한 명칭을 살펴보자.

그림 14-1(384p)에서 트리는 항상 루트(Root)에서부터 시작된다.<br>
루트는 자식(Child) 노드를 가지며, 간선(Edge)으로 연결되어 있다.<br>
**자식 노드의 개수는 차수(Degree)** 라고 하며, **크기(Size)는 자신을 포함한 모든 자식 노드의 개수**다.<br>
높이(Height)는 **현재 위치에서부터 리프(Leaf)까지의 거리**, 깊이(Depth)는 **루트에서부터 현재 노드까지의 거리**다.

트리는 그 자식도 트리인 서브트리(Subtree) 구성을 띤다고 앞서 언급한 바 있다.

<img src="https://user-images.githubusercontent.com/55045377/122329995-533fb100-cf6d-11eb-9178-473ebc2e8550.png" width=60% height=60%>

레벨(Level)은 0에서부터 시작한다.<br>
논문에 따라 1에서부터 시작하는 경우도 있으니 현재 대부분의 문서에서는 0에서부터 시작하는 것이 좀 더 일반적이며 여기서도 0부터 시작한다.<br>
트리는 항상 단방향(Uni-Directional)이기 때문에, 간선의 화살표는 생략 가능하다.<br>
그림 14-1(384p)에서도 마찬가지로 생략해서 표현했고 일반적으로 방향은 위에서 아래로 향한다.
<br><br>

## 그래프 VS 트리
그래프와 트리의 차이점은 무엇일까?<br>
무엇보다도 가장 두드러지는 큰 차이점은 다음과 같다.

***"트리는 순환 구조를 갖지 않는 그래프이다."***

핵심은 순환 구조(Cyclic)가 아니라는 데 있다.<br>
트리는 특수한 형태의 그래프의 일종이며,크게 그래프의 범주에 포함된다.<br>
하지만 **트리는** 그래프와 달리 어떠한 경우에도 **1) 한번 연결된 노드가 다시 연결되는 법이 없다.**<br>
이외에도 단방향(Uni-Directional), 양방향(Bi-Directional)을 모두 가리킬 수 있는 그래프와 달리, **2) 트리는 부모 노드에서 자식 노드를 가리키는 단방향뿐이다.**<br>
그뿐만 아니라 **3) 트리는 하나의 부모 노드를 갖는다**는 차이점이 있으며 **4) 루트 또한 하나**여야 한다.<br>
이처럼 트리와 그래프는 서로 다른 점이 많다.
<br><br>

그렇다면 그림 14-2(385p)에서 트리가 아닌 예를 한번 살펴보자.

<img src="https://user-images.githubusercontent.com/55045377/122330148-98fc7980-cf6d-11eb-9884-76de0ca5db1f.png" width=65% height=65%>

이 그림에서 1)은 순환 구조이기 때문에, 앞서 그래프와 트리의 차이점에서 첫 번째로 언급한, 트리는 순환 구조를 갖지 않아야 한다는 정의에 부합하지 않는다.<br>
2)는 C 노드의 부모가 A, D 이렇게 둘이다. 부모 노드는 단 하나여야 한다.<br>
3)은 A->B와 D<-C->E가 서로 연결되어 있지 않으며, 루트가 둘이므로 트리가 아니다. 루트 또한 하나여야 한다.
<br><br>

## 이진 트리
트리 중에서도 가장 널리 사용되는 트리 자료구조는 **이진 트리**와 **이진 탐색 트리(Binary Search Tree)(이하 BST)** 다.<br>
먼저, 각 노드가 m개 이하의 자식을 갖고 있으면, m-ary 트리(다항 트리, 다진 트리)라고 한다.<br>
여기서 m=2일 경우, 즉 모든 노드의 차수가 2 이하일 때는 특별히 이진 트리(Binary Tree)라고 구분해서 부른다.<br>
이진 트리는 왼쪽, 오른쪽, 최대 2개의 자식을 갖는 매우 단순한 형태로, 다진 트리에 비해 훨씬 간결할 뿐만 아니라 여러 가지 알고리즘을 구현하는 일도 좀 더 간단하게 처리할 수 있어서, 대체로 트리라고 하면 특별한 경우가 아니고서는 대부분 이진 트리를 일컫는다.

이제 이진 트리 유형(Types of Binary Trees)을 살펴보자.<br>
여기서는 되도록 널리 쓰이는 형태로 언급하되, 위키피디아를 기준으로 정리해본다.<br>
먼저, 이진 트리에서는 대표적으로 그림 14-3(386p)과 같은 3가지 유형을 들 수 있다.

<img src="https://user-images.githubusercontent.com/55045377/122330251-cc3f0880-cf6d-11eb-974a-593d36048282.png" width=75% height=75%>

* 정 이진 트리(Full Binary Tree): 모든 노드가 0개 또는 2개의 자식 노드를 갖는다.
* 완전 이진 트리(Complete Binary Tree): 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 모든 노드는 가장 왼쪽부터 채워져 있다.
* 포화 이진 트리(Perfect Binary Tree): 모든 노드가 2개의 자식 노드를 갖고 있으며, 모든 리프 노드가 동일한 깊이 또는 레벨을 갖는다. 문자 그대로, 가장 완벽한(Perfect) 유형의 트리다.
<br>

그렇다면 지금부터는 트리에 관한 다양한 내용들을 문제를 통해 하나씩 풀이해보면서 살펴보자.
<br><br>

## 리트 코드
### 문제 42 이진 트리의 최대 깊이
> 387p

* 이진 트리의 최대 깊이를 구하라.<br>
* **내가 짠 코드**<br>
```python
def solution(nodes):
  root = 0
  depth = 1

  # 예외처리
  if not nodes:
    return 0

  while nodes:
    lchild = (root*2) + 1
    if lchild < len(nodes):
      depth += 1
      root = lchild
    else:
      break
  return depth


nodes = [3,9,20,None,None,15,7]
print(solution(nodes))
```
노드처리 하지 않고 주어진 리스트대로 풀었다.
<br><br>

```python
import collections

class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  def binary(self, tree):
    if tree is None:
      return 0
    queue = collections.deque([tree])
    cnt = 0

    while queue:
      cnt += 1
      for _ in range(len(queue)):
        node = queue.popleft()
        if node.left:
          queue.append(node.left)
        if node.right:
          queue.append(node.right)

    return cnt


solution = Solution()
n1 = TreeNode(7)
n2 = TreeNode(15)
n3 = TreeNode(20,n2,n1)
n4 = TreeNode(9)
n5 = TreeNode(3,n4,n3)
print(solution.binary(n5))
```
풀이를 보고 다시 풀어 보았다.
<br><br>

### 문제 42 이진 트리의 최대 깊이 풀이
#### 풀이1. 반복 구조로 BFS 풀이
깊이(Depth)는 어떻게 측정할 수 있을까?<br>
여러 가지 방법이 있겠지만 여기서는 BFS(너비 우선 탐색)로 풀이해보자.<br>
다시 한번 복기하자면, DFS는 스택, BFS는 큐를 사용하여 구현한다.<br>
여기서는 BFS로 풀이할 것이므로, 다음과 같이 큐를 선언하고 풀이할 준비를 해보자.
```python
def maxDepth(self, root: TreeNode) -> int:
    ...
    queue = collections.deque([root])
    depth = 0
    
    while queue:
        ...
        
    return depth
```
큐를 선언하고 반복 구조도 구성하여, BFS 반복을 이용해 풀이할 구조를 잡았다.<br>
파이썬에서 큐는 일반적인 리스트로도 모든 연산이 가능하지만, 데크 자료형을 사용하면 이중 연결 리스트로 구성되어 있기 때문에 큐와 스택 연산을 모두 자유롭게 활용 가능할 뿐만 아니라 양방향 모두 O(1)에 추출할 수 있어 좋은 성능을 보인다는 점을 이미 여러 번 언급한 바 있다.(자세한 내용은 10장 참고)

실제로도 양방향 추출이 빈번할 경우, 리트코드의 실행 속도 또한 데크가 리스트에 비해 훨씬 더 빠르다.
```python
while queue:
    depth += 1
    for _ in range(len(queue)):
        cur_root = queue.popleft()
        ...
        if cur_root.has_child():
            queue.append(cur_root.child)
```
큐 변수에는 현재 깊이 depth에 해당하는 모든 노드가 들어 있고, queue.popleft()로 하나씩 끄집어 내면서 cur_root.has_child()로 자식 노드가 있는지 여부를 판별한 후 자식 노드를 다시 큐에 삽입한다.<br>
참고로 여기서 .has_child()와 .child는 모두 이해를 돕기 위한 수도코드다.<br>
실제 동작하는 코드는 바로 이어서 다시 살펴본다.

그렇다면 동일한 큐에 삽입하다 보니 행여나 자식 노드가 부모 노드와 섞이진 않을까?<br>
아마 섞일 것이다. 그러나 이 for 반복문에서는 자식 노드가 추출되는 일은 없을 것이다.<br>
왜냐면 처음 for 문 진입 시 부모 노드의 길이 len(queue)만큼만 반복하도록 선언했기 때문이다.

따라서 부모 노드가 모두 추출된 이후에는 for 문을 빠져 나가게 되고, 다시 한 바퀴 돌아 while queue 구문에서 이번에는 다음 번 깊이의 노드 반복이 진행될 것이다.

이해를 돕기 위해 queue.append(cur_root.child)으로 깊이별 큐에 삽입되는 자식 노드를 도식화해보면 그림 14-4(389p)와 같다.

<img src="https://user-images.githubusercontent.com/55045377/122328977-90a33f00-cf6b-11eb-8bba-4efa0185a359.png" width=55% height=55%>

이 그림처럼 깊이별로 노드가 추가되는 BFS 구조를 나타낼 수 있다.<br>
이 문제 예제의 입력값이 [3,9,20,null,null,15,7]일 때, depth가 1이면 queue에는 [3], 2이면 [9,20], 3이면 [15,7]이 삽입되어 처리된다.

깊이 depth는 while 구문의 반복 횟수이므로 BFS에서 반복 횟수는 곧 높이가 된다.<br>
이제 반복 횟수를 리턴하면 최종 결과를 구할 수 있다.<br>
수도코드를 실제로 동작하는 코드로 바꾸고, 전체 코드를 정리해보면 다음과 같다.
```python
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

def maxDepth(self, root: TreeNode) -> int:
    if root is None:
        return 0
    queue = collections.deque([root])
    depth = 0
    
    while queue:
      depth += 1
      for _ in range(len(queue)):
          cur_root = queue.popleft()
          if cur_root.left:
              queue.append(cur_root.left)
          if cur_root.right:
              queue.append(cur_root.right)
    # BFS 반복 횟수 == 깊이
    return depth
```
<br><br>

실제 입출력하는 풀이1 코드
```python
import collections

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
        
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root is None:
            return 0
        queue = collections.deque([root])
        depth = 0

        while queue:
          depth += 1
          for _ in range(len(queue)):
              cur_root = queue.popleft()
              if cur_root.left:
                  queue.append(cur_root.left)
              if cur_root.right:
                  queue.append(cur_root.right)
        # BFS 반복 횟수 == 깊이
        return depth
        
root = [3,9,20,None,None,15,7]
solution = Solution()
n1 = TreeNode(7)
n2 = TreeNode(15)
# n3 = TreeNode(None)
# n4 = TreeNode(None)
n5 = TreeNode(20,n2,n1)
n6 = TreeNode(9)
n7 = TreeNode(3,n6,n5)
print(solution.maxDepth(n7))
```
<br><br>

### 문제 43 이진 트리의 직경
> 390p

* 이진 트리에서 두 노드 간 가장 긴 경로의 길이를 출력하라.<br>
* **내가 짠 코드**<br>
```python
class diameter(object):
  cnt = 0

  def solution(self,nodes):
    leaf = nodes.index(nodes[-1])
    visited = [0 for _ in range(len(nodes))]
    
    def dfs(leaf):
      parent = (leaf-1)//2  # 부모 노드
      rchild = (leaf*2) + 2  # 오른쪽 자식 노드
      
      while leaf >= 0:
        visited[leaf] = 1  # 방문 표시
        if parent >= 0 and visited[parent] == 0:
          dfs(parent)
          self.cnt += 1
        else:
          if rchild < len(nodes) and visited[rchild] == 0:
            dfs(rchild)
            self.cnt += 1
        break

    dfs(leaf)
    return self.cnt

nodes = [1,2,3,4,5]
Solution = diameter()
print(Solution.solution(nodes))
```
처음에는 cnt를 부모 함수인 solution()에 선언해서 구현했는데, 그렇게 하니까 백트래킹하면서 cnt가 0이 되었다. 그래서 아래의 문법 부분을 보고 수정했더니 구현이 잘 됐다.<br>
연결 리스트로 풀려고 했던 건 완성하지 못했다. 
<br><br>

### 문제 43 이진 트리의 직경 풀이
#### 풀이1. 상태값 누적 트리 DFS
가장 긴 경로를 찾는 방법은 먼저 가장 말단, 즉 리프 노드까지 탐색한 다음 부모로 거슬러 올라가면서 각각의 거리를 계산해 상태값을 업데이트 하면서 다음과 같이 누적해 나가면 될 것 같다.

<img src="https://user-images.githubusercontent.com/55045377/122332637-b3d0ed00-cf71-11eb-950b-1141dffa4168.png" width=55% height=55%>

그림 14-5(390p)에서는 존재하지 않는 노드에도 -1이라는 값을 부여한다.<br>
나중에 보면 알겠지만, 정 이진 트리(Full Binary Tree)가 아닌 대부분의 경우에는 존재하지 않는 자식 노드에 -1을 부여해 페널티를 주기 위함이다.<br>
이렇게 거슬러 올라가 최종 루트에서 상태값은 2, 거리는 3이 된다.<br>
정답은 거리인 3이다.<br>
상태값은 리프 노드에서 현재 노드까지의 거리다.<br>
거리는 왼쪽 자식 노드의 상태값과 오른쪽 자식 노드의 상태값의 합에 2를 더한 값이다.

다시 정리하면, 최종적으로 거리는 왼쪽 자식 노드의 리프노드에서 현재 노드까지의 거리(상태값)와, 오른쪽 자식 노드의 리프 노드에서 현재 노드까지의 거리(상태값)의 합에 2(현재 노드와 왼쪽, 오른쪽 자식 노드와의 거리)를 더한 값과 같다.<br>
구체적인 계산은 지금부터 풀이를 이어가보면서 살펴본다.<br>
다음과 같이 탐색 함수부터 살펴보자.
```python
def dfs(node: TreeNode) -> int:
    ...
    left = dfs(node.left)
    right = dfs(node.right)
```
이처럼 계속 재귀 호출을 통해 왼쪽, 오른쪽의 각 리프 노드까지 DFS로 탐색한다.
```python
def dfs(node: TreeNode) -> int:
    ...
    self.longest = max(self.longest, left + right + 2)
    return max(left, right) + 1
```
이후에는 2개의 값을 계산하는데, 하나는 최종 결과가 될 가장 긴 경로 self.longest, 나머지 하나는 앞서 얘기한 상태값 max(left, right) + 1을 말한다.<br>
편의상 a, b로 치환해 표현해보면 다음과 같다.
```python
a = left + right + 2      # 거리
b = max(left, right) + 1  # 상태값
```
자식 노드가 하나도 없는 경우 left, right는 모두 -1이고, 이 경우 거리는 0, 상태값도 0이 된다.<br>
자식 노드가 모두 존재하는 경우에는, 그리고 자식 노드가 둘 다 상태값이 0이라면, 거리인 a는 2, 상태값인 b는 1이 된다.<br>
즉 거리는 왼쪽, 오른쪽 자식 사이의 경로이므로 2를 더하게 되고, 상태값은 양쪽 자식 중 최대 상태값과 부모까지의 거리인 1을 더하게 된다.<br>
그래서 그림 14-5(390p)에서도 루트의 상태값은 2, 거리값은 3이 된다.<br>
정답은 거리값인 3이다.

전체 코드는 다음과 같다.
```python
class Solution:
    longest: int = 0
    
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        def dfs(node: TreeNode) -> int:
            if not node:
                return -1
            # 왼쪽, 오른쪽의 각 리프 노드까지 탐색
            left = dfs(node.left)
            right = dfs(node.right)
            
            # 가장 긴 경로
            self.longest = max(self.longest, left + right + 2)
            # 상태값
            return max(left, right) + 1
            
        dfs(root)
        return self.longest
```
<br><br>

#### 문법. 중첩 함수에서 클래스 변수를 사용한 이유
앞서 풀이에서 중첩 함수(Nested Function)를 사용할 때, 왜 longest 변수를 함수 내에서 선언해 사용하지 않고 바깥에 클래스 변수로 선언해서 번거롭게 self.longest 형태로 사용했을까?

중첩 함수는 부모 함수의 변수를 자유롭게 읽어들일 수 있다.<br>
그러나 **중첩 함수에서 부모함수의 변수를 재할당하게 되면 참조 ID가 변경되며 별도의 로컬 변수로 선언된다.**<br>
앞서 풀이의 경우 self.longest = max(self.longest, left + right + 2) 라는 부분이 있다.<br>
longest 변수에 값을 재할당하는 부분인데 여기서는 self.longest를 사용했다.<br>
왜냐면 재할당을 해야 하기 때문이다.<br>
따라서 부모 함수의 변수를 그대로 사용할 수 없었고, 함수 바깥에서 클래스 변수로 선언 후 사용했다.

만약 longest의 값이 숫자나 문자가 아니라 리스트나 딕셔너리 같은 자료형이라면 append()등의 메소드를 이용해 재할당 없이 조작이 가능하다.<br>
중첩 함수 내에서도 변수의 값을 조작할 수 있다.<br>
그러나 숫자나 문자인 경우 불변 객체이기 때문에 중첩 함수 내에서는 값을 변경할 수 없다.<br>
이 때문에 클래스 변수를 사용했다.
<br><br>

























