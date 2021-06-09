# 목차

# 14장 트리
## 트리 순회
> 트리 순회란 그래프 순회의 한 형태로 트리 자료구조에서 각 노드를 정확히 한 번 방문하는 과정을 말한다.

그래프 순회와 마찬가지로 트리 순회(Tree Traversals) 또한 DFS 또는 BFS로 탐색하는데, 특히 이진 트리에서 DFS는 노드의 방문 순서에 따라 다음과 같이 크게 3가지 방식으로 구분된다.

  **1. 전위(Pre-Order) 순회(NLR)<br>
    2. 중위(In-Order) 순회(LNR)<br>
    3. 후위(Post-Order) 순회(LRN)**

순회 방식의 영문 두문자어를 구성하는 3가지 문자 L,R,N 중 L은 현재 노드의 왼쪽 서브트리, R은 현재 노드의 오른쪽 서브트리, N은 현재 노드 자신을 의미한다.<br>
즉 전위 순회는 NLR이므로 현재 노드를 먼저 순회한 다음 왼쪽과 오른쪽 서브트리를 순회하고, 중위 순회는 LNR이므로 왼쪽 서브트리를 순회한 다음 현재 노드, 후위 순회는 LRN이므로 왼쪽과 오른쪽 서브트리를 순회한 다음 현재 노드를 순회한다.

이제 3가지 방식을 그림으로 표현해보면 다음 그림 14-26과 같다.

<img src="https://user-images.githubusercontent.com/55045377/121292740-6aefb780-c925-11eb-9ca7-ffe6325e31f2.png" width=50% height=50%>

이 그림에서 각 원의 둘레에 놓인 세 점 중 왼편의 짙은 회색 점은 **전위**, 아래편 하얗게 빈 점은 **중위**, 오른편의 옅은 회색 점은 **후위 순회**를 나타내며, 각각의 순회 결과는 다음과 같다.

* 왼쪽 점(전위): F, B, A, D, C, E, G, I, H
* 아래쪽 점(중위): A, B, C, D, E, F, G, H, I
* 오른쪽 점(후위): A, C, E, D, B, H, I, G, F
<br>

그렇다면 이제 각 순회 방식을 코드를 통해 좀 더 구체적으로 살펴보자.<br>
트리의 순회 방식은 재귀 또는 반복, 모두 구현이 가능하지만 트리의 재귀적 속성으로 인해 재귀 쪽이 훨씬 더 구현이 간단하다. <br>
전위 순회의 재귀 구현 수도코드는 리스트 14-1과 같다.
* **리스트 14-1** DFS 전위 순회의 재귀 구현 수도코드
  ```
  preorder(node)
      if (node == null)
          return
      vislt(node)
      preorder(node.left)
      preorder(node.right)
  ```
반면, 전위 순회의 반복 구현 수도코드는 리스트 14-2와 같다.
* **리스트 14-2** DFS 전위 순회의 반복 구현 수도코드
  ```
  iterativePreorder(node)
      if (node == null)
          return
      s ← empty stack
      s.push(node)
      while (not s.isEmpty())
          node ← s.pop()
          vislt(node)
          
          if node.right != null
              s.push(node.right)
          if node.left != null
              s.push(node.left)
  ```
리스트 14-1과 14-2를 비교해볼 수 있듯, 재귀 구현과 반복 구현은 수도코드의 구성이나 직관성에서 많은 차이를 보인다.<br>
트리의 재귀적인 특성 탓에, 재귀 구현이 훨씬 더 간단하고 직관적이다. <br>
따라서 여기서도 모두 재귀로 구현하여 실행 가능한 코드를 작성해보겠다.

먼저 연결 리스트를 담을 Node 클래스를 정의하고 트리의 전체 입력값을 root 변수로 다음과 같이 정의해보자.

```python
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
root = Node('F',
            Node('B',
                  Node('A'),
                  Node('D',
                        Node('C'), Node('E'))
                  ),
            Node('G',
                  None,
                  Node('I', Node('H'))
                  )
            )
```
이제 각각의 순회 방식을 구현해보자.
<br><br>

### 전위 순회
```python
def preorder(node):
    if node is None:
        return
    print(node.val)
    preorder(node.left)
    preorder(node.right)
```
재귀로 구현하면 코드가 매우 간결하고 보기 쉽다.<br>
실행 결과는 다음과 같다.
```python
>>> preorder(root)
F, B, A, D, C, E, G, I, H
```
<br><br>

### 중위 순회
```python
def inorder(node):
    if node is None:
        return
    inorder(node.left)
    print(node.val)
    inorder(node.right)
```


























