# 목차
* [chapter 14. 트리](#14장-트리)
  + [트리 순회](#트리-순회)
    - [전위 순회](#전위-순회)
    - [중위 순회](#중위-순회)
    - [후위 순회](#후위-순회)
* [리트코드](#리트코드)
  + [문제 54 전위, 중위 순회 결과로 이진 트리 구축](#문제-54-전위-중위-순회-결과로-이진-트리-구축)
  + [문제 54 전위, 중위 순회 결과로 이진 트리 구축 풀이](#문제-54-전위-중위-순회-결과로-이진-트리-구축-풀이)
    - [풀이1. 전위 순회 결과로 중위 순회 분할 정복](#풀이1-전위-순회-결과로-중위-순회-분할-정복)
<br><br><br>

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
어떤 차이인지 금방 알아차릴 수 있을 것이다.<br>
마찬가지로 실행 결과는 다음과 같다.
```python
>>> inorder(root)
A, B, C, D, E, F, G, H, I
```
<br><br>

### 후위 순회
```python
def postorder(node):
    if node is None:
        return
    postorder(node.left)
    postorder(node.right)
    print(node.val)
```
실행 결과는 다음과 같다.
```python
>>> postorder(root)
A, C, E, D, B, H, I, G, F
```
여기까지 트리의 순회 방식을 각각 살펴봤다.<br>
재귀로 구현하면 매우 직관적이므로 각각의 순회가 어떤 구조로 구현되는지 어렵지 않게 이해할 수 있을 것이다.<br>
이제 순회의 특징을 이용한 문제를 한번 풀이해보자.
<br><br>

## 리트코드
### 문제 54 전위, 중위 순회 결과로 이진 트리 구축
> 444p

* 트리의 전위, 중위 순회 결과를 입력값으로 받아 이진 트리를 구축하라.<br><br>
  전위 순회 결과: [3,9,20,15,7]<br>
  중위 순회 결과: [9,3,15,20,7]<br>
  결과는 아래과 같은 이진 트리가 된다.
 
  <img src="https://user-images.githubusercontent.com/55045377/121456169-9a172f00-c9e0-11eb-9fd2-252abf4183fd.png" width=20% height=20%>
  
<br><br>

* **내가 짠 코드**<br>
```python

```
<br><br>

### 문제 54 전위, 중위 순회 결과로 이진 트리 구축 풀이
#### 풀이1. 전위 순회 결과로 중위 순회 분할 정복
순회에는 크게 전위, 중위, 후위 순회가 있으며 이 셋 중 2가지만 있어도 이진 트리를 복원할 수 있다.<br>
이 문제는 바로 여기서 2가지 결과인 전위와 중위 결과를 받아 이진 트리를 만들어보는 문제다.<br>
그럼 전위와 중위는 과연 어떤 관계가 있을까.<br>
마찬가지로 그림 14-27에서 관계를 파악해보자.

<img src="https://user-images.githubusercontent.com/55045377/121464117-3b58b200-c9ee-11eb-9e2b-429bd1108c3b.png" width=40% height=40%>


예제의 입력값이 너무 단순하므로, 트리 그림을 1 부터 9까지 좀 더 복잡한 형태로 새로 구성해봤다.<br>
이 그림에서 전위, 중위 순회 결과를 유심히 살펴보자.<br>
전위의 첫 번째 값은 부모 노드다.<br>
즉 전위 순회의 첫 번째 결과는 정확히 중위 순회 결과를 왼쪽과 오른쪽으로 분할시키는 역할을 한다.<br>
중위 순회의 분할 정복(Divide and Conquer)(22장 참조) 문제로 바꾸는 것이다.<br>
두 번째로 왼쪽 노드의 2는 중위 순회 결과를 정확히 반으로 가르고, 각각 왼쪽 자식은 4, 오른쪽 자식은 5로 마무리한다.

오른쪽의 경우 3이 첫 번째 값인데, 마침 중위 순회에서는 맨 오른쪽에 위치해 있다.<br>
이 말은 3의 오른쪽 자식 노드는 존재하지 않는다는 얘기다.<br>
실제로 그림 14-27의 트리를 살펴보면, 3의 오른쪽 노드가 존재하지 않는 것을 확인할 수 있다. <br>
이제 남아 있는 노드들을 이후에도 계속 분할을 시도하면, 이 그림처럼 트리 형태로 최종적으로 구성할 수 있다.

이제 코드로 구현해보자.
```python
index = inorder.index(preorder.pop(0))
```
먼저, 전위 순회 첫 번째 결과를 가져와 중위 순회를 분할하는 인덱스로 한다.<br><br>

```python
node = TreeNode(inorder[index])
node.left = self.buildTree(preorder, inorder[0:index])
node.right = self.buildTree(preorder, inorder[index + 1:])
```
이 값을 현재 노드로 구성하고, 이를 기준으로 중위 순회 결과를 쪼개서 왼쪽, 오른쪽으로 각각 마무리될 때 분할 정복 구조로 재귀 호출하면, 트리를 구성할 수 있다.

전체 코드는 다음과 같다.
```python
def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
    if inorder:
        # 전위 순회 결과는 중위 순회 분할 인덱스
        index = inorder.index(preorder.pop(0))
        
        # 중위 순회 결과 분할 정복
        node = TreeNode(inorder[index])
        node.left = self.buildTree(preorder, inorder[0:index])
        node.right = self.buildTree(preorder, inorder[index + 1:])
        
        return node
```
여기서 전위 순회 결과는 pop(0)으로 값을 꺼내온다.<br>
즉 큐 연산이며, 파이썬에서는 데크(Deque)로 구현할 수 있다.<br>
여기서는 입력값 리스트를 특별히 다른 자료형으로 변환하지 않고 그대로 사용한다.<br>
파이썬의 리스트는 pop()에도 인텍스를 별도로 지정할 수 있어서, 사실상 스택과 큐의 모든 역할을 수행할 수 있다.<br>
하지만 파이썬의 리스트에서 pop(0)은 시간 복잡도가 O(n)이므로 주의가 필요하다.
<br><br>
















