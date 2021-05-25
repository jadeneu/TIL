# 목차

# 14장 트리
## 리트코드
### 문제 44 가장 긴 동일 값의 경로
> 393p

* 동일한 값을 지닌 가장 긴 경로를 찾아라.<br><br>
* **내가 짠 코드**<br>
리스트 형태로 풀다가 완성하지 못했다. 풀이를 보고 연결리스트로 따라서 구현해봤다.
<br><br>

### 문제 44 가장 긴 동일 값의 경로 풀이
#### 풀이1. 상태값 거리 계산 DFS
이 문제는 바로 이전 문제인 43번 '이진 트리의 직경' 문제와 매우 유사하다.<br>
트리의 말단, 리프 노드까지 DFS로 탐색해 내려간 다음, 값이 일치할 경우 다음과 같이 거리를 차곡차곡 쌓아 올려가며 백트래킹하는 형태로 풀이할 수 있다.

> 그림 14-6

그림 14-6(394p)에는 예제의 첫 번째 입력값을 기준으로 트리를 나타내고, 루트 노드부터 우측 리프 노드까지 이동 거리를 구하는 과정을 표현했다.<br>
이 경우 루트 노드에서부터 DFS로 재귀 탐색을 진행하면서 리프에 도달하면 그때부터 백트래키하면서 거리를 누적해나간다.<br>
먼저, 다음과 같이 DFS 재귀 탐색을 한다.
```python
def dfs(node: TreeNode):
    ...
    left = dfs(node.left)
    right = dfs(node.right)
```
이렇게 재귀 호출로 내려가면 left, right는 각각 리프 노드에 이르러서 값을 리턴받게 된다.<br>
더 이상 존재하지 않는 노드까지 내려가게 되면 다음과 같은 형태로 값을 리턴한다.
```python
if node is None:
    return 0
```
존재하지 않는 노드까지 내려가게 되면 거리 0을 리턴한다.<br>
이제 이 값이 점점 백트래킹 되면서 증가할 것이다.<br>
이 문제는 동일 값(Univalue) 여부를 판별해 거리를 계산하는 문제이기 때문에, 다음과 같이 자식 노드가 동일한 값인지 확인하는 과정이 필요하다.
```python
if node.left and node.left.val == node.val:
    left += 1
else:
    left = 0
if node.right and node.right.val == node.val:
    right += 1
else:
    right = 0
```
왼쪽과 오른쪽 자식 노드를 각각 확인해서 현재 노드, 그러니까 부모 노드와 동일한 경우 각각 거리를 1 증가한다.<br>
이제 다음과 같이 왼쪽 자식과 오른쪽 자식 노드 간 거리의 합을 결과로 한다.
```python
result = max(result, left + right)
```
합이 가장 큰 값을 최종 결과로 한다.<br>
이제 다음번 백트래킹 시 계산을 위해 앞서 문제와 유사하게 상태값을 셋팅해서 부모 노드로 올려야 한다.<br>
다음과 같이 부모 노드를 위해 현재까지의 거리를 리턴해준다.
```python
return max(left, right)
```
지금까지 합의 최댓값을 계산해왔기 때문에 따라서 상태값도 합인 left + right를 리턴해야 할 것 같다.<br>
그러나 잠시 생각해보면, 현재 노드는 양쪽 자식 노드를 모두 연결할 수 있지만 현재 노드의 부모 노드에서는 지금의 양쪽 자식 노드를 동시에 연결할 수 없다.<br>
단방향이므로 양쪽 자식 노드 중 어느 한쪽 자식만 택할 수 있으며, 이는 트리의 특징이기도 하다.<br>
따라서 둘 중 큰 값을 상태값으로 리턴해준다.<br>
어차피 한 군데만 방문할 수 있다면 더 큰 쪽을 방문하는 게 낫기 때문이다.

이제 모두 정리한 전체 코드는 다음과 같다.
```python
class Solution:
    result: int = 0
    
    def longestUnivaluePath(self, root: TreeNode) -> int:
        def dfs(node: TreeNode):
            if node is None:
                return 0
                
            # 존재하지 않는 노드까지 DFS 재귀 탐색
            left = dfs(node.left)
            right = dfs(node.right)
            
            # 현재 노드가 자식 노드와 동일한 경우 거리 1 증가
            if node.left and node.left.val == node.val:
                left += 1
            else:
                left = 0
            if node.right and node.right.val == node.val:
                right += 1
            else:
                right = 0
                
            # 왼쪽과 오른쪽 자식 노드 간 거리의 합 최댓값이 결과
            self.result = max(self.result, left + right)
            # 자식 노드 상태값 중 큰 값 리턴
            return max(left, right)
            
        dfs(root)
        return self.result
```
<br><br>

### 문제 45 이진 트리 반전
> 397p

* 중앙을 기준으로 이진 트리를 반전시키는 문제다.<br><br>
* **내가 짠 코드**<br><br>
```python

```
<br><br>

### 문제 45 이진 트리 반전 풀이
#### 풀이1. 파이썬다운 방식
사실 이 문제는 파이썬다운 방식으로 정말 쉽게 풀 수 있다.<br>
다음과 같이 짧고 간결한 형태로 풀이가 가능하다.
```python
def invertTree(self, root: TreeNode) -> TreeNode:
    if root:
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
    return None
```
그림 14-8(398p)을 보면 좀 더 이해가 쉬울 것 같다.

> 그림 14-8 재귀를 이용한 이진 트리 반전

이 그림에서 루트 노드인 4에서 시작하되, 지금까지와는 달리 여기서는 오른쪽부터 재귀 탐색을 진행한다.<br>
앞에서 보인 전체 코드에서, self.invertTree의 파라미터로 root.right를 먼저 넘겼기 때문이다.<br>
그러나 노드 9에서 첫 번째 스왑이 일어나고(실제로는 그 자식인 None까지 탐색하다가 None을 리턴받은 이후에), 두 번째는 노드 6에서 스왑이 일어난다.<br>
그림 14-8(398p)에는 각각의 스왑 순서에 번호를 부여했으니 순서를 어렵지 않게 파악할 수 있을 것이다.<br>
오른쪽 노드가 다 스왑된 이후에는 왼쪽 노드가 동일한 형태로 스왑된다.

마지막 7)번, 그러니까 루트 노드 4의 자식 노드 간 최종 스왑이 이뤄지기 직전의 상태는 그림 14-9(399p)와 같다.

> 그림 14-9 마지막 스왑이 이뤄지기 직전의 상태

이 그림에서 루트 노드 4의 자식 노드 2와 7이 마지막으로 스왑되면서 전체 트리가 반전된다.<br>
이제 스왑된 이후의 값들을 꼼꼼이 살펴보면 이 문제에서 제시된 예제와 모두 동일함을 확인할 수 있다.

참고로 마지막 return None은 생략이 가능하다.<br>
아무것도 리턴하지 않으면 파이썬은 자연스럽게 None을 할당하기 때문이다.<br>
이 또한 동적 타이핑(Dynamic Typing) 언어인 파이썬의 강력한 기능으로, 초심자는 유의해야 할 기능 중 하나다.<br>
물론 이처럼 의도적으로 생략하는 게 좋은 방법은 아니지만, 적어도 코드를 한 줄 더 없애 좀 더 짧게 만들 수 있다는 정도는 참고하자.<br>
return None을 생략한 전체 코드는 다음과 같다.
```python
def invertTree(self, root: TreeNode) -> TreeNode:
    if root:
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
```
<br><br>

#### 풀이2. 반복 구조로 BFS
이번에는 반복 구조로 BFS를 이용해 풀이해보자.<br>
42번 '이진 트리의 최대 갚이' 문제를 풀었을 때와 거의 유사한 형태의 코드가 될 것 같다.<br>
코드가 길지 않기 때문에 바로 전체 코드를 살펴보자.
```python
def invertTree(self, root: TreeNode) -> TreeNode:
    queue = collections.deque([root])
    
    while queue:
        node = queue.popleft()
        # 부모 노드부터 하향식 스왑
        if node:
            node.left, node.right = node.right, node.left
            
            queue.append(node.left)
            queue.append(node.right)
            
    return root
```
앞서 이진 트리의 최대 깊이 문제는 while 구문 내부에 별도의 for 문이 있어 부모 노드에 대해 따로 반복해야 했는데, 여기서는 그런 제약이 없이 자유롭게 스왑하면서 queue에 추가한다.<br>
먼저 삽입된 노드는 반복 구조로 계속 스왑되면서 자식 노드가 계속해서 큐에 추가되는 구조가 된다.

앞서 재귀 풀이가 가장 말단, 리프 노드까지 내려가서 백트래킹하면서 스왑하는 상향 방식이라면, 이 풀이는 부모 노드부터 스왑하면서 계속 아래로 내려가는 하향 방식 풀이라 할 수 있다.
<br><br>

#### 풀이3. 반복 구조로 DFS
이 풀이를 DFS(깊이 우선 탐색)로 풀이하기 위해 BFS 풀이에서 딱 한 줄만 수정했다.
```python
def invertTree(self, root: TreeNode) -> TreeNode:
    stack = collections.deque([root])
    
    while stack:
        node = stack.pop()  # 바뀐 부분
        # 부모 노드부터 하향식 스왑
        if node:
            node.left, node.right = node.right, node.left
            
            stack.append(node.left)
            stack.append(node.right)
            
    return root
```
<br><br>

#### 풀이4. 반복 구조로 DFS 후위 순회
앞서 풀이는 전위(Pre-Order) 순회 형태로 처리했지만 다음과 같이 후위(Post-Order) 순회로 변경해도 아무런 문제가 없다.<br>
그저 탐색 순서만 달라질 뿐이다.<br>
후위 순회 방식의 구조부터 간단히 살펴보면 다음과 같다.
```python
def invertTree(self, root: TreeNode) -> TreeNode:
    ...
    while stack:
        ...
        if node:
            stack.append(node.left)
            stack.append(node.right)
            
            node.left, node.right = node.right, node.left
    ...
```
스왑 위치만 다를 뿐 모든 게 동일하다.<br>
전체 코드는 다음과 같다.
```python
def invertTree(self, root: TreeNode) -> TreeNode:
    stack = collections.deque([[root])
    
    while stack:
        node = stack.pop()
        
        if node:
            stack.append(node.left)
            stack.append(node.right)
            
            node.left, node.right = node.right, node.left
            
    return root
```
<br><br>





























