# 목차
* [chapter 14. 트리](#14장-트리)
  + [이진 탐색 트리(BST)](#이진-탐색-트리bst)
  + [자가 균형 이진 탐색 트리](#자가-균형-이진-탐색-트리)
* [리트코드](#리트코드)
  + [문제 50 정렬된 배열의 이진 탐색 트리 변환](#문제-50-정렬된-배열의-이진-탐색-트리-변환)
  + [문제 50 정렬된 배열의 이진 탐색 트리 변환 풀이](#문제-50-정렬된-배열의-이진-탐색-트리-변환-풀이)
    - [풀이1. 이진 검색 결과로 트리 구성](#풀이1-이진-검색-결과로-트리-구성)<br><br>
  + [문제 51 이진 탐색 트리(BST)를 더 큰 수 합계 트리로](#문제-51-이진-탐색-트리bst를-더-큰-수-합계-트리로)
  + [문제 51 이진 탐색 트리(BST)를 더 큰 수 합계 트리로 풀이](#문제-51-이진-탐색-트리bst를-더-큰-수-합계-트리로-풀이)
    - [풀이1. 중위 순회로 노드 값 누적](#풀이1-중위-순회로-노드-값-누적)<br><br>
  + [문제 52 이진 탐색 트리(BST) 합의 범위](#문제-52-이진-탐색-트리bst-합의-범위)
  + [문제 52 이진 탐색 트리(BST) 합의 범위 풀이](#문제-52-이진-탐색-트리bst-합의-범위-풀이)
    - [풀이1. 재귀 구조 DFS로 브루트 포스 탐색](#풀이1-재귀-구조-dfs로-브루트-포스-탐색)
    - [풀이2. DFS 가지치기로 필요한 노드 탐색](#풀이2-dfs-가지치기로-필요한-노드-탐색)
    - [풀이3. 반복 구조 DFS로 필요한 노드 탐색](#풀이3-반복-구조-dfs로-필요한-노드-탐색)
    - [풀이4. 반복 구조 BFS로 필요한 노드 탐색](#풀이4-반복-구조-bfs로-필요한-노드-탐색)<br><br>
  + [문제 53 이진 탐색 트리(BST) 노드 간 최소 거리](#문제-53-이진-탐색-트리bst-노드-간-최소-거리)
  + [문제 53 이진 탐색 트리(BST) 노드 간 최소 거리 풀이](#문제-53-이진-탐색-트리bst-노드-간-최소-거리-풀이)
    - [풀이1. 재귀 구조로 중위 순회](#풀이1-재귀-구조로-중위-순회)
    - [풀이2. 반복 구조로 중위 순회](#풀이2-반복-구조로-중위-순회)
<br><br><br>

# 14장 트리
## 이진 탐색 트리(BST)
앞서 이진 트리는 정렬 여부와 관계 없이 모든 노드가 둘 이하의 자식을 갖는 단순한 트리 형태를 말했다.<br>
그렇다면 이진 탐색 트리(Binary Search Tree(이하 BST)) 란 무엇일까?

**이진 ‘탐색’ 트리란** 정렬된 트리를 말하는데 노드의 왼쪽 서브트리에는 그 노드의 값보다 작은 값들을 지닌 노드들로 이뤄져 있는 반면, 노드의 오른쪽 서브트리에는 그 노드의 값과 같거나 큰 값들을 지난 노드들로 이루어져 있는 트리를 뜻한다.<br>
이렇게 왼쪽과 오른쪽의 값들이 각각 값의 크기에 따라 정렬되어 있는 트리를 이진 탐색 트리라 한다.<br>
이 트리의 가장 훌륭한 점은 탐색 시 시간 복잡도가 O(log n)이라는 점이다.

로그는 정말 마법과도 같은 수식인데(애초에 수학에서 로그의 존재 자체가 마법 같다) 1억 개의 아이템도 단 27번이면 모두 찾아낼 수 있다.<br>
이산수학에서도 매번 등장하는 수학 함수이기도 하며, 심지어 카드 마술에까지 이진 탐색이(트리가 아닌 이진 탐색(Binary Search)만을 일컬을 때는 흔히 ‘이진 검색’(18장 참조)이라고도 부른다) 등장한다.<br>
1부터 100까지 관객이 마음 속에 생각해둔 숫자를 맞춰 보겠다고 하면, 단 7번의 카드만 제시하면 반드시 맞출 수 있기 때문이다.<br>
이처럼 마술 같은 힘을 보이는 **로그의 강력함을 잘 표현한 트리가 바로 이진 탐색 트리다.**<br>
이진 탐색 트리를 이용해 원하는 값(여기서는 8)을 찾는(탐색하는) 과정을 그림 14-18에서 볼 수 있다.

<img src="https://user-images.githubusercontent.com/55045377/120411230-65c3c300-c38f-11eb-885b-eb10f705e958.png" width=45% height=45%>

이 그림에서 먼저, 루트는 15이며, 8은 15보다 작다. 따라서 왼쪽 자식 노드를 탐색한다.<br>
10 또한 마찬가지로 8보다 크므로, 왼쪽을 택한다.<br>
5는 8보다 작으므로, 오른쪽으로 탐색한다.<br>
그다음, 7은 8보다 작으므로 마지막으로 오른쪽을 탐색해 정답 8을 찾아낸다. 이처럼 **단 4번 만에** 정답을 찾을 수 있었다.

만약 6을 찾는다면(여기서는 정답이 없는) 5 이후에 오른쪽을 탐색하게 될 것이고, 그다음에는 7, 이후에 다시 왼쪽을 탐색하려 하는데, 더 이상 왼쪽 노드가 없으므로 탐색을 중단하고 ‘정답 없음’을 출력하게 될 것이다.
<br><br>

<img src="https://user-images.githubusercontent.com/55045377/120411758-4b3e1980-c390-11eb-956f-9020f0fa9a98.png" width=65% height=65%>

그림 14-19는 로버트 세지윅 교수의『알고리즘 개정 4판』에서 256개의 키를 랜덤으로 생성해 BST로 표현한 실험을 진행한 결과를 표현한 그림이다.<br>
BST는 이처럼 랜덤하게 생성해도 대부분의 경우 균형이 잘 맞는 아름다운 형태로 트리를 표현할 수 있지만, 운이 나쁘면 트리의 모양이 균형을 제대로 이루지 못할 수 있다.<br>
균형이 많이 깨지면 탐색 시에 O(log n)이 아니라 O(n)에 근접한 시간이 소요될 수 있다.<br>
그림 14-20은 균형이 깨진 대표적인 모습이다.

<img src="https://user-images.githubusercontent.com/55045377/120412379-6cebd080-c391-11eb-8672-e0aa7e0de442.png" width=30% height=30%>

이 그림은 운이 나쁘게 비효율적으로 구성된 경우인데, 이렇게 되면 연결 리스트와 다르지 않다.<br>
7을 찾기 위해서는 루트부터 맨 끝까지 차례대로 모두 탐색해야 하므로 전혀 효율적이지 않다.<br>
강력한 로그 계산을 기반으로 하는 이진 탐색 트리의 장점을 전혀 살릴 수 없다.<br>
따라서 이런 트리는 균형을 맞춰 줄 필요가 있다.<br>
그래서 고안해낸 것이 바로 **‘자가균형 이진 탐색 트리’** 다.
<br><br>

## 자가 균형 이진 탐색 트리
> 자가 균형(또는 높이 균형) 이진 탐색 트리는 삽입, 삭제 시 자동으로 높이를 작게 유지하는 노드 기반의 이진 탐색 트리다.

자가 균형 이진 탐색 트리(Self-Balancing Binary Search Tree)는 최악의 경우에도 이진 트리의 균형이 잘 맞도록 유지한다.<br>
즉 높이를 가능한 한 낮게 유지하는 것이 중요하다는 얘기다.<br>
다음 그림과 같이 불균형과 균형의 차이는 크게 높이로 구분할 수 있다.<br>
기존의 불균형 트리인 그림 14-20과 함께 비교한 결과는 다음 그림 14-21과 같다.

<img src="https://user-images.githubusercontent.com/55045377/120420209-e1793c00-c39e-11eb-815f-910c84240ace.png" width=60% height=60%>

균형이 잘 잡힌 트리를 나타낸 그림 2)는 루트의 높이가 3으로, 높이를 가능한 한 작게 유지한 트리임을 확인할 수 있다.<br>
균형이 맞지 않은 그림 1)의 트리는 루트의 높이가 6으로 매우 높다.<br>
이처럼 루트의 높이로 불균형과 균형을 구분할 수 있다.

그림 14-21에서 원하는 값을 찾으려면 1)의 비효율적인 불균형 트리에서는 7을 찾기 위해 7번의 연산이 실행되어야 한다.<br>
그러나 2)의 균형 잡힌 트리에서는 불과 두 번 만에 7을 찾는게 가능하다.<br>
여기서는 전체가 7개의 노드에 불과하지만 만약 노드가 100만 개가 있다고 가정해보자.<br>
1)처럼 불균형할 경우에는, 가장 끝에 위치한 아이템을 찾기 위해 100만 번의 연산이 필요하다.<br>
2)처럼 균형이 잡혀 있을 경우에는, 균형이 완벽히 잡혀 있다고 가정할 때 최대 19번([log(2)1,000,000])이면 어떤 값이든 반드시 찾아내는 게 가능하다.

이처럼 불균형과 균형의 성능 차이는 꽤 크다.<br>
따라서 트리의 균형, 즉 높이의 균형을 맞추는 작업은 매우 중요하다.<br>
이와 같이 높이 균형을 맞춰주는 자가 균형 이진 탐색 트리의 대표적인 형태로는 AVL 트리와 레드-블랙 트리 등이 있으며, 특히 레드-블랙 트리는 높은 효율성으로 인해 실무에서도 매우 빈번하게 쓰이는 트리 형태이기도 하다.<br>
자바의 해시맵 또한 효율적인 저장 구조를 위해 해시 테이블의 개별 체이닝(Separate Chaining) 시 연결 리스트와 함께 레드-블랙 트리를 병행해 저장하는 구조로 구현되어 있다.
<br><br>

## 리트코드
### 문제 50 정렬된 배열의 이진 탐색 트리 변환
> 425p

* 오름차순으로 정렬된 배열을 높이 균형(Height Balanced) 이진 탐색 트리로 변환하라.

<img src="https://user-images.githubusercontent.com/55045377/120420825-146fff80-c3a0-11eb-8d70-a843e3a95a45.png" width=20% height=20%>

이 문제에서 높이 균형이란 모든 노드의 두 서브 트리(Subtree) 간 갚이 차이가 1 이하인 것을 말한다. 정렬된 배열 [-10,-3,0 ,5,9]이 주어졌을 때, 가능한 답변은 [0,-3,9,-10,null,5]이며 이와 같은 높이 균형 BST(이진 탐색 트리)로 나타낼 수 있다.
<br><br>

* **내가 짠 코드**<br>
풀이를 보고 코드를 짜봤다.
```python
from typing import List

class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  def binary(self, lists: List[int]) -> TreeNode:
    if not lists:
      return None

    mid = len(lists) // 2

    node = TreeNode(lists[mid])
    node.left = self.binary(lists[:mid])
    node.right = self.binary(lists[mid+1:])

    return node

solution = Solution()
lists = [-10,-3,0,5,9]
print(solution.binary(lists))
```

<br><br>

### 문제 50 정렬된 배열의 이진 탐색 트리 변환 풀이
#### 풀이1. 이진 검색 결과로 트리 구성
이 문제를 제대로 풀이하기 위해서는 먼저 이진 탐색 트리, 그러니까 BST의 특징에 대해 정확하게 파악하고 있어야 한다.<br>
앞서 BST를 설명할 때 이미 이야기했지만, BST는 이진 검색의 마법을 적용한 이진 트리고, 따라서 BST를 만들기 위해서는 정렬된 배열을 이진 검색으로 계속 쪼개 나가기만 하면 된다.<br>
당연한 얘기지만 정렬되어 있지 않으면 사용할 수 없다.<br>
이진 검색 자체가 정렬된 배열에서는 어떤 값이든지 간에 log(n)에 찾아낼 수 있고, 동일한 이름의 BST 또한 당연히 정렬된 배열을 기준으로 한다.

예제에서 제시된 입력값 외에 완전 이진 트리(Complete Binary Tree)형태가 될 수 있도록 -7,7 두 값을 추가해 좀 더 이해가 쉽도록 그림 14-22와 같이 구성해봤다.

<img src="https://user-images.githubusercontent.com/55045377/120422499-5babbf80-c3a3-11eb-9893-9eaaffd8acc7.png" width=40% height=40%>

이 그림에서는 정확히 한 번의 이진 검색 결과가 각각의 노드로 구성된다.<br>
즉 0이 가장 먼저, 그다음은 -7, 7... 이런 식으로 구성되면서 내려간다.<br>
이진 검색을 할 때마다 그 값이 노드로 구성됨을 알 수 있다.

이제 이 알고리즘을 코드로 구현해보자.
```python
def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
    ...
    mid = len(nums) // 2
    
    node = TreeNode(nums[mid])
    node.left = self.sortedArrayToBST(nums[:mid])
    node.right = self.sortedArrayToBST(nums[mid + 1:])
    ...
```
정확히 중앙값을 갖도록 내림값을 리턴하는 // 연산자를 사용했다.<br>
즉 len(nums)가 3 이라면, 2를 나눈 결과인 1.5에서 내림하여 1이 된다.<br>
인텍스는 0부터 시작하기 때문에 1은 [0, 1, 2]에서 정확히 중앙값이 된다.<br>
왼쪽 자식은 남은 절반을 재귀 호출로 계속 처리하고, 오른쪽 자식 또한 마찬가지로 처리한다.

파이썬의 슬라이스 기능을 이용하면 매우 간단하게 코드로 구현할 수 있으며, 절반씩 분할해 처리되는 분할 정복 구조(22장 참조)로 처리된다.<br>
끝까지 처리되면 배열은 정확히 높이 균형 BST가 될 것이다.<br>
전체 코드는 다음과 같다.
```python
def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
    if not nums:
        return None
        
    mid = len(nums) // 2
    
    # 분할 정복으로 이진 검색 결과 트리 구성
    node = TreeNode(nums[mid])
    node.left = self.sortedArrayToBST(nums[:mid])
    node.right = self.sortedArrayToBST(nums[mid + 1:])
    
    return node
```
<br><br>

실제 실행 코드
```python
from typing import List

class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
        
class Solution(object):
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if not nums:
            return None
            
        mid = len(nums) // 2
        
        # 분할 정복으로 이진 검색 결과 트리 구성
        node = TreeNode(nums[mid])
        node.left = self.sortedArrayToBST(nums[:mid])
        node.right = self.sortedArrayToBST(nums[mid + 1:])
        
        return node
    
solution = Solution()
lists = [-10,-3,0,5,9]
print(solution.sortedArrayToBST(lists))
```
<br><br>

### 문제 51 이진 탐색 트리(BST)를 더 큰 수 합계 트리로
> 428p

* BST의 각 노드를 현재값보다 더 큰 값을 가진 모든 노드의 합으로 만들어라.

<img src="https://user-images.githubusercontent.com/55045377/120584660-59ad3380-c46b-11eb-8978-a5dc19d3c48a.png" width=30% height=30%>

* 입력
  ```
  [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
  ```
* 출력
  ```
  [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
  ```
* 설명<br>
  자신보다 더 큰 값을 가진 모든 노드의 합이 출력이 된다. 6의 경우 더 큰 값을 지닌 노드는 7,8 이며 이 값을 모두 합한 6+7+8=21 이 출력 노드가 된다.
<br><br>

* **내가 짠 코드**<br>
풀 수 있을 것 같아서 주어진 입력값 그대로 리스트로 풀어보려고 하고, 연결 리스트로 풀어보려고도 했지만 잘 풀리지 않았다. 풀이를 보고 감을 잡은 후에 다시 풀어야 할 것 같다.

풀이를 보고 코드를 짜봤다.
```python
class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  val = 0
  def binary(self, nodes: TreeNode) -> TreeNode:
    if nodes:
      self.binary(nodes.right)
      self.val += nodes.val
      nodes.val = self.val
      self.binary(nodes.left)

    return nodes


# nodes = [4,1,6,0,2,5,7,None,None,None,3,None,None,None,8]
solution = Solution()
n1 = TreeNode(8)
n2 = TreeNode(3)
n3 = TreeNode(7,None,n1)
n4 = TreeNode(5)
n5 = TreeNode(2,None,n2)
n6 = TreeNode(0)
n7 = TreeNode(6,n4,n3)
n8 = TreeNode(1,n6,n5)
n9 = TreeNode(4,n8,n7)
print(solution.binary(n9))
```
<br><br>

### 문제 51 이진 탐색 트리(BST)를 더 큰 수 합계 트리로 풀이
#### 풀이1. 중위 순회로 노드 값 누적
자신보다 같거나 큰 값을 구하려면 자기 자신을 포함한 우측 자식 노드의 합을 구하면 된다.<br>
앞서 BST 구조에 대해서 충분히 설명했듯이, BST의 우측 자식 노드는 항상 부모 노드보다 큰 값이기 때문이다.

예제 입력값을 그림 14-23과 같이 파란색은 출력 노드, 화살표는 계산 순서로 나타냈다.

<img src="https://user-images.githubusercontent.com/55045377/120588822-d263be00-c472-11eb-8eea-904c56a0f72f.png" width=40% height=40%>

root를 입력받았을 때 먼저 맨 오른쪽까지 내려가고, 그다음 부모 노드, 다시 왼쪽 노드 순으로 이동하면서 자신의 값을 포함해 누적한다.<br>
'오른쪽-부모-왼쪽' 순으로 이어지며, 오른쪽 자식부터 운행하는 중위 순회(In-Order)에 해당됨을 알 수 있다.<br>
중위 순회에 대해서는 440페이지 ‘트리 순회’에서 다시 자세히 소개하겠다.
<br><br>

```python
def bstToGst(self, root: TreeNode) -> TreeNode:
    ...
    self.bstToGst(root.right)
    self.val += root.val
    root.val = self.val
    self.bstToGst(root.left)
    ...
```
self.val은 지금까지 누적된 값이고, root.val은 현재 노드의 값이다.<br>
즉 중위 순회를 하면서 현재 노드의 값을 자기 자신을 포함한 지금까지의 누적된 값과 합한다.<br>
전체 코드는 다음과 같다.
```python
class Solution:
    val: int = 0
    
    def bstToGst(self, root: TreeNode) -> TreeNode:
        # 중위 순회 노드 값 누적
        if root:
            self.bstToGst(root.right)
            self.val += root.val
            root.val = self.val
            self.bstToGst(root.left)
            
        return root
```
최초 self.val은 클래스 멤버 변수로 선언하고 0이 되도록 직관적으로 선언했다.<br>
아울러 root가 있을 때만 처리되게 했으며, root.val을 조작한 이후에는 다시 root를 리턴한다.<br>
사실 리턴을 하지 않아도 관계 없다.<br>
이 코드에서 보듯이, 재귀 호출 시 리턴 값은 사용하지 않기 때문이다.

또한 파이썬은 모든 변수가 참조이기 때문에, 객체 내부의 값을 변경하면 해당 객체를 가리키는 모든 변수는 자연스럽게 따라서 값이 변한다.<br>
그러나 이 문제는 리트코드의 동작 방식으로 추측컨대, 리트코드에서는 아마 최초 호출 시 root = solution.bstToGst(root)와 같은 형태로 리턴 값을 받아오도록 구현되어 있을 것이다.<br>
이때 리턴 값을 돌려주지 않으면 root는 None이 되어 버린다.<br>
리트코드에서 리턴 값을 주지 않으면 정답이 출력되지 않는 것도 그 때문일 것이다.<br>
따라서 반드시 return root를 명시한다.
<br><br>

### 문제 52 이진 탐색 트리(BST) 합의 범위
> 431p

* 이진 탐색 트리(BST)가 주어졌을 때 L 이상 R 이하의 값을 지닌 노드의 합을 구하라.
* 입력
  ```
  root = [10,5,15,3,7,null,18], L = 7, R = 15
  ```
* 출력
  ```
  32
  ```
* 설명<br>
  7 이상 15 이하인 또 다른 노드는 10이 있으며 따라서 결과는 7+10+15=32가 된다.
  <br><br>
  
* **내가 짠 코드**<br>
```python
####### 리스트로 풀기 #######
def solution(root, L, R):
  answer = 0
  for num in root:
    if num and (num >= L and num <= 15):
      answer += num

  return answer


root = [10,5,15,3,7,None,18]
L = 7
R = 15
print(solution(root,L,R))
```
```python
####### 연결리스트로 풀기 #######
class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  cnt: int = 0

  def bst(self, root, L, R):
    if root:
      if root.val >= L and root.val <= R:
        self.cnt += root.val
      self.bst(root.left,L,R)
      self.bst(root.right,L,R)

    return self.cnt


solution = Solution()
n1 = TreeNode(18)
n2 = TreeNode(7)
n3 = TreeNode(3)
n4 = TreeNode(15,None,n1)
n5 = TreeNode(5,n3,n2)
n6 = TreeNode(10,n5,n4)
L = 7
R = 15
print(solution.bst(n6,L,R))
```
<br><br>
시간복잡도 개선 코드
```python
####### 시간복잡도 줄이기 #######
class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  def bst(self, root, L, R):
    stack, cnt = [root], 0

    while stack:
      node = stack.pop()
      if node:
        if L <= node.val <= R:
          cnt += node.val
          stack.append(node.left)
          stack.append(node.right)
        elif node.val < L:
          stack.append(node.right)
        elif node.val > R:
          stack.append(node.left)

    return cnt


solution = Solution()
n1 = TreeNode(18)
n2 = TreeNode(7)
n3 = TreeNode(3)
n4 = TreeNode(15,None,n1)
n5 = TreeNode(5,n3,n2)
n6 = TreeNode(10,n5,n4)
L = 7
R = 15
print(solution.bst(n6,L,R))
```
<br><br>

### 문제 52 이진 탐색 트리(BST) 합의 범위 풀이
#### 풀이1. 재귀 구조 DFS로 브루트 포스 탐색
이 문제의 예제 입력값을 그림 14-24에 나타냈다.

<img src="https://user-images.githubusercontent.com/55045377/120742823-a60f7680-c532-11eb-88f4-103f61fe532d.png" width=40% height=40%>

이 그림을 보면, L = 7, R = 15일 때 L 이상, R 이하인 노드는 자기 자신을 포함해 7, 10, 15이며, 이들의 합 sum()은 32이다.<br>
어떻게 하면 이 노드들을 쉽게 찾아내 합을 계산할 수 있을까?<br>
이 문제는 DFS로 전체를 탐색한 다음, 노드의 값이 L과 R 사이일 때만 값을 부여하고, 아닐 경우에는 0을 취해 계속 더해 나가면 쉽게 구할 수 있다.

전체 코드는 다음과 같다.
```python
def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
    if not root:
        return 0
        
    return (root.val if L <= root.val <= R else 0) + \
            self.rangeSumBST(root.left, L, R) + \
            self.rangeSumBST(root.right, L, R)
```
그러나 이 방식은 모든 노드를 탐색하는 브루트 포스 풀이다. 좀 더 최적화를 진행해보자.
<br><br>

#### 풀이2. DFS 가지치기로 필요한 노드 탐색
이번에는 DFS로 불필요한 노드는 가지치기를 통해 최적화를 진행해보자.<br>
DFS로 탐색하되 L, R의 조건에 해당되지 않는 가지(Branch)를 쳐내는(Pruning) 형태로 탐색에서 배제하도록 다음과 같이 구현해보자.
```python
def dfs(node: TreeNode) -> int:
    ...
    if node.val < L:
        return dfs(node.right)
    elif node.val > R:
        return dfs(node.left)
```
이진 탐색 트리는 왼쪽이 항상 작고, 오른쪽이 항상 크다.<br>
즉 현재 노드 node가 L보다 작을 경우, 더 이상 왼쪽 가지는 탐색할 필요가 없기 때문에 오른쪽만 탐색하도록 재귀 호출을 리턴한다.<br>
마찬가지로 R보다 클 경우, 오른쪽은 더 이상 탐색할 필요가 없으므로 왼쪽만 탐색하도록 재귀 호출을 리턴한다.<br>
이렇게 불필요한 탐색을 줄여 최적화할 수 있다.<br>
전체 코드는 다음과 같다.
```python
def rangeSumBST(self , root: TreeNode, L: int, R: int) -> int:
    def dfs(node: TreeNode):
        if not node:
            return 0
            
        if node.val < L:
            return dfs(node.right)
        elif node.val > R:
            return dfs(node.left)
        return node.val + dfs(node.left) + dfs(node.right)
        
    return dfs(root)
```
이 경우 불필요한 탐색은 배제하게 되므로 탐색 효율이 매우 높다.<br>
필요한 노드만 탐색하여 해당 노드의 값들을 더해 나가게 된다.
<br><br>

#### 풀이3. 반복 구조 DFS로 필요한 노드 탐색
대부분의 재귀 풀이는 반복으로 변경할 수 있다.<br>
이 문제 또한 다음과 같이 반복으로 풀이할 수 있다.<br>
일반적으로 반복 풀이가 재귀 풀이에 비해 좀 더 직관적으로 이해가 쉽다.<br>
바로 전체 코드를 살펴보자.
```python
def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
    stack, sum = [root] , 0
    # 스택 이용 필요한 노드 DFS 반복
    while stack:
        node = stack.pop()
        if node:
            if node.val > L:
                stack.append(node.left)
            if node.val < R:
                stack.append(node.right)
            if L <= node.val <= R:
                sum += node.val
    return sum
```
마찬가지로 유효한 노드만 스택에 계속 집어 넣으면서, L과 R사이의 값인 경우 값을 더해나간다.<br>
유효한 노드만 삽입하기 때문에 앞서 풀이인 가지치기와 탐색 범위가 유사하며, 스택이므로 DFS와 동일한 탐색 구조를 띤다.
<br><br>

#### 풀이4. 반복 구조 BFS로 필요한 노드 탐색
BFS로 탐색해도 동일하다.<br>
여기서는 스택을 단순히 큐 형태로 바꾸기만 하면, BFS를 구현할 수 있다.<br>
원래는 파이썬의 데크를 사용해야 성능을 높일 수 있지만, 여기서는 편의상 간단히 리스트를 그냥 pop(0) 로 처리하는 정도로 다음과 같이 BFS를 구현하고, 마찬가지로 동일하게 정답을 풀이할 수 있다.<br>
전체 코드는 다음과 같다.
```python
def rangeSumBST(self , root: TreeNode , L: int, R: int) -> int:
    stack, sum = [root], 0
    # 큐 연산을 이용해 반복 구조 BFS로 필요한 노드 탐색
    while stack:
        node = stack.pop(0)
        if node:
            if node.val > L:
                stack.append(node.left)
            if node.val < R:
                stack.append(node.right)
            if L <= node.val <= R:
                sum += node.val
    return sum
```
<br><br>

### 문제 53 이진 탐색 트리(BST) 노드 간 최소 거리
> 435p

* 두 노드 간 값의 차이가 가장 작은 노드의 값의 차이를 출력하라.
* 입력
  ```
  root = [4,2,6,1,3,null,null]
  ```
* 출력
  ```
  1
  ```
* 설명<br>
  [4,2,6,1,3,null,null]은 다음과 같은 트리로 표현할 수 있다.
  
  <img src="https://user-images.githubusercontent.com/55045377/120962037-e91f5300-c799-11eb-9b41-f0b56c136688.png" width=35% height=35%>
  
  노드 3과 노드 4의 값의 차이는 1이다.
<br><br>  
  
* 입력
  ```
  root = [10,4,15,1,8,null,null]
  ```
* 출력
  ```
  2
  ```
* 설명<br>
  [10,4,15,1,8,null,null]은 다음과 같은 트리로 표현할 수 있다.
  
  <img src="https://user-images.githubusercontent.com/55045377/120962337-7d89b580-c79a-11eb-8659-eb05f48e055e.png" width=30% height=30%>
  
  이 경우 노드 간 값의 차이가 가장 작은 노드는 8과 10이다. 1과 4는 거리가 차이가 3이고, 4와 8은 차이가 4이지만 8과 10의 차이는 2로 최솟값이다.
<br><br>

* **내가 짠 코드**<br>
```python
####### 리스트로 풀기 #######
def solution(root):
  result = root[0]

  for i in range(len(root)):
    if root[i] == None:
      continue
    for j in range(i+1,len(root)):
      if root[j] == None:
        continue
      sub = abs(root[i]-root[j])
      if sub == 1:
        return 1
      elif result > sub:
        result = sub

  return result
          

root = [10,4,15,1,8,None,None]
print(solution(root))
```
<br><br>

### 문제 53 이진 탐색 트리(BST) 노드 간 최소 거리 풀이
#### 풀이1. 재귀 구조로 중위 순회
값의 차이가 가장 작은 노드를 찾으려면 어디와 어디 노드를 비교해야 하는지 생각해보자.<br>
BST의 왼쪽 자식은 항상 루트보다 작고, 오른쪽 자식은 항상 루트보다 크다.<br>
그렇다면 먼저, 다음과 같은 BST가 있다고 가정해보자.

<img src="https://user-images.githubusercontent.com/55045377/121119583-d61e8880-c856-11eb-90b4-70c74bcf8a6c.png" width=35% height=35%>

이 경우 루트 D와 가장 차이가 작을 수 있는 노드는 딱 2개 노드만 가능한데, 어디와 어디일까?<br>
정답은 I와 G 이다.<br>
이외에는 항상 이들 노드보다 값의 차이가 클 것이다.<br>
D->B의차이는 D->C보다는 무조건 크다. 왼쪽으로 갈수록 값이 더 작아지기 때문이다.<br>
D->A, D->F도 마찬가지다. D->C보다 작아질 수 없다.<br>
따라서 D->B부터는 제외하고, 그렇다면 D->E 하위 노드들에 가능성이 있는데, 이 중에서도 D->H는 D->E보다 무조건 크기 때문에 마찬가지로 배제한다.<br>
오른쪽 자식은 더 큰 값을 지니기 때문에 이 경우 D->I가 가장 작은 값이 될 수 있다.

물론 항상 정답은 아닐 수도 있다.<br>
왜냐면 D->G가 의외로 가장 작은 값이 될 수 있기 때문이다.<br>
따라서 여기서는 D->I와 D->G 중 작은 값을 택하면 정답이 된다.<br>
이제 이렇게 비교할 수 있는 알고리즘을 찾아내면 된다.

이번에는 직접 값을 입력해 그림으로 나타내보자.<br>
이 문제에서 예제의 입력값은 너무 단순하므로 그림 14-25와 같이 [8,4,12,2,6,null,null,1,3,5,7] 정도로 입력값을 좀 더 복잡하게 정해 트리를 나타내 보자.

<img src="https://user-images.githubusercontent.com/55045377/121120125-edaa4100-c857-11eb-818f-9c51e48e834b.png" width=45% height=45%>

이 그림에는 두 노드 간 값의 차이가 가장 작은 노드를 계산하기 위한 순서를 파란 글씨로 표시했다.<br>
이 순서대로 탐색하면서 각 거리를 계산해 나가면 값의 차이가 가장 작은 노드의 값을 찾을 수 있다.<br>
여기서 루트인 8은 7과 12 두 군데를 비교해보는데, 앞서 설명한 D->I, D->G인 비교 대상과 동일함을 확인할 수 있다. <br>
참고로, 여기서 정답은 1이며 이렇게 값의 차이가 l인 노드가 여릿 있다.<br>
그러나 최솟값을 출력하기만 하면 되므로, 중복된 값인 l을 정답으로 출력하면 된다.

그렇다면 이렇게 비교할 수 있는 탐색 순서는 어떻게 구현하면 될까?<br>
간단하게 중위 순회를 하면서 이전 탐색 값과 현재 값을 비교하면 바로 이 순서대로 비교할 수 있을 것 같다.<br>
간단히 코드로 구현하면 다음과 같은 구조가 된다.
```python
def f():
    if root.left:
        f(root.left)
        
    result = min()
    
    if root.right:
        f(root.right)
```
이 형태가 중위 순회의 기본 뼈대가 될 것이다.<br>
바로 구현해보면 전체 코드는 다음과 같다.
```python
class Solution:
    prev = -sys.maxsize
    result = sys.maxsize
    
    # 재귀 구조 중위 순회 비교 결과
    def minDiffInBST(self, root: TreeNode) -> int:
        if root.left:
            self.minDiffInBST(root.left)
            
        self.result = min(self.result, root.val - self.prev)
        self.prev = root.val
        
        if root.right:
            self.minDiffInBST(root.right)
            
        return self.result
```
<br><br>

#### 풀이2. 반복 구조로 중위 순회
이번에는 동일한 알고리즘을 반복 구조로 구현해보자.<br>
이미 여러 차례 얘기했지만, 반복으로 풀이하면 재귀보다 훨씬 직관적이어서 이해하기도 쉽고 풀이 또한 쉬운 편이다.<br>
이전 풀이인 재귀 구조와 비교 순서는 동일하며 결과 또한 그림 14-25와 동일하다.

여기에 더한 추가 장점은 재귀일 때는 prev와 result를 클래스 멤버 변수로 선언했지만, 반복 구조에서는 한 함수 내에서 처리할 수 있기 때문에 함수 내 변수로 선언이 가능하다는 점이다.<br>
전체 코드는 다음과 같다.
```python
def minDiffInBST(self, root: TreeNode) -> int:
    prev = -sys.maxsize
    result = sys.maxsize
    
    stack = []
    node = root
    
    # 반복 구조 중위 순회 비교 결과
    while stack or node:
        while node:
            stack.append(node)
            node = node.left
            
        node = stack.pop()
        
        result = min(result, node.val - prev)
        prev = node.val
        
        node = node.right
        
    return result
```
DFS 풀이인 만큼 스택을 사용했고, 오른쪽 자식 노드를 택하기 전에 비교하는 형태로 재귀와 동일하게 중위 순회로 풀이했다.<br>
재귀와 달리 클래스 변수를 사용하지 않아도 되므로, 변수 값의 변화를 추적하기가 쉽고 좀 더 직관적이다.
<br><br>
 

























