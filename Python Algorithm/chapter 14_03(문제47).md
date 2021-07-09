# 목차
* [chapter 14. 트리](#14장-트리)
* [리트코드](#리트코드)
  + [문제 47 이진 트리 직렬화 & 역직렬화](#문제-47-이진-트리-직렬화--역직렬화)
  + [문제 47 이진 트리 직렬화 & 역직렬화 풀이](#문제-47-이진-트리-직렬화--역직렬화-풀이)
    - [풀이1. 직렬화 & 역직렬화 구현](#풀이1-직렬화--역직렬화-구현)
      - [직렬화](#직렬화)
      - [역직렬화](#역직렬화)
<br><br><br>

# 14장 트리
## 리트코드
### 문제 47 이진 트리 직렬화 & 역직렬화
> 406p

* 이진 트리를 배열로 직렬화하고, 반대로 역직렬화하는 기능을 구현하라. 즉 다음과 같은 트리는 [1,2,3,null,null,4,5] 형태로 직렬화할 수 있을 것이다.

<img src="https://user-images.githubusercontent.com/55045377/120095130-231ba400-c15f-11eb-9192-dc4b51df00e0.png" width=40% height=40%>

<br><br>
* **내가 짠 코드**<br>
```python
import collections

class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Solution(object):
  def serialize(self, tree):
    queue = collections.deque([tree])
    result = []

    while queue:
      node = queue.popleft()   
      if node:
        result.append(node.val) 
        queue.append(node.left)
        queue.append(node.right)
      else:
        result.append(None)
    return result

  def deserialize(self, tree):
    root = TreeNode(tree[0])
    queue = collections.deque([root])
    index = 1

    while queue:
      node = queue.popleft()
      if tree[index] != None:
        node.left = TreeNode(tree[index])
        queue.append(node.left)
      index += 1

      if tree[index] != None:
        node.right = TreeNode(tree[index])
        queue.append(node.right)
      index += 1

    return root


solution = Solution()
n1 = TreeNode(5)
n2 = TreeNode(4)
n3 = TreeNode(3,n2,n1)
n4 = TreeNode(2)
n5 = TreeNode(1,n4,n3)
result1 = solution.serialize(n5)
result2 = solution.deserialize(result1)
print(result1)
print(result2)
```
serialize()의 결과가 [1, 2, 3, None, None, 4, 5, None, None, None, None]으로 나오는 걸로 마무리 지었다.<br>
deserialize()는 풀이를 보고 따라서 구현했다.
<br><br>

### 문제 47 이진 트리 직렬화 & 역직렬화 풀이
#### 풀이1. 직렬화 & 역직렬화 구현
직렬화를 제대로 구현하기 위해서는 우선 이진 트리의 특징과 표현에 대해 정확히 알아야 한다.<br>
'이진 트리' 데이터 구조는 논리적인 구조다.<br>
이를 파일이나 디스크에 저장하기 위해서는 물리적인 형태로 바꿔줘야 하는데, 이를 직렬화(Serialize)라고 한다. 반대는 역직렬화(Deserialize)다.<br>
파이썬에는 pickle이라는 직렬화 모듈을 기본으로 제공한다.<br>
이 모듈의 이름으로 인해 파이썬 객체의 계층(Hierarchy) 구조를 바이트 스트림(Byte Stream)으로 변경하는 작업은 피클링(Pickling)이라고도 한다.<br>
이외에도 마샬링(Marshaling), 플래트닝(Flattening) 등으로 표현하기도 한다.

먼저, 그림 14-11(407p)과 같이 이진 힙을 배열로 표현하는 경우를 살펴보자.<br>
그림에서 파란 글씨는 인덱스를 의미한다.

<img src="https://user-images.githubusercontent.com/55045377/120095195-8e657600-c15f-11eb-9bbb-4ae19eb2d39e.png" width=60% height=60%>

이 그림의 1) 이진 힙은 완전 이진 트리(Complete Binary Tree)로서, 배열로 표현하기 매우 좋은 구조다.<br>
높이 순서대로 순회하면 모든 노드를 배열에 낭비 없이 배치할 수 있기 때문이다.<br>
이 그림의 2)처럼 완전 이진 트리는 배열에 빈틈없이 배치가 가능하며, 대개 트리의 배열 표현의 경우 계산을 편하게 하기 위해 인덱스는 1부터 사용한다.

해당 노드의 인덱스를 알면 깊이가 얼마인지, 부모와 자식 노드가 배열 어디에 위치하는지 바로 알아낼 수 있다.<br>
깊이는 1, 2, 4, 8, ... 순으로 2배씩 증가하며, **인덱스는 1부터 시작했기 때문에** 부모/자식 노드의 위치는 각각 부모 i/2, 왼쪽 자식 2i, 오른쪽 자식 2i+1의 간단한 수식으로 계산할 수 있다.<br>
이처럼 해당되는 배열의 인덱스를 금방 찾아낼 수 있다.<br>
물론 꼭 완전 이진 형태가 아니어도 비어 있는 위치는 얼마든지 널(Null)로 표현할 수 있기 때문에, 사실상 모든 트리는 배열로 표현이 가능하다.
<br><br>

#### 직렬화
우선 직렬화에 대한 과정을 정리해보자.

이 문제는 직렬화 알고리즘에 제약이 없으므로, BFS로 구현하든 DFS로 구현하든 아무런 상관이 없다.<br>
하지만 가능하면 그림 14-12(408p)와 같이 BFS 탐색 결과로 표현해서, 배열만 봐도 트리의 형태를 직관적으로 떠올릴 수 있도록 이해하기 쉽게 구현해보자.<br>
간편한 계산을 위해, 배열은 1번 인덱스부터 시작되는 형태로 표현했다.

<img src="https://user-images.githubusercontent.com/55045377/120095244-d4bad500-c15f-11eb-88a7-292a3c15281b.png" width=60% height=60%>

이진 트리를 BFS로 표현하면 순서대로 배치되기 때문에, DFS에 비해 매우 직관적으로 알아볼 수 있다.<br>
바로 우측 2)번 그림에 제시한 배열과 동일한 형태가 된다.<br>
여기서 파란 글씨는 인덱스며, 회색 글씨는 비어 있는 인덱스를 의미한다.<br>
1)번 그림에서 트리의 비어 있는 노드들은 마찬가지로 2)번 그림의 배열에서도 공간을 비워 두었는데, 여기서는 널(Null) 대신 코드에서는 #으로 표현하기로 한다.<br>
이 문제의 경우 리턴 값을 문자열로 받게 했는데, 파이썬의 널인 None은 문자열로 만들 수 없기 때문이다.<br>
따라서 0번 인덱스와 4, 5번은 모두 #으로 표현한다.

그렇다면 직렬화 결과는 어떻게 될까?<br>
-> [#, A, B, C, #, #, D, E]

이제 다음과 같이 코드를 살펴보자.<br>
BFS 탐색을 할 것이므로, 먼저 앞선 45번 '이진 트리 반전' 문제에서 풀이했던 BFS 반복 풀이를 그대로 가져와보겠다.
```python
def invertTree(self, root: TreeNode) -> TreeNode:  # 1
    queue = collections.deque([root])
    
    while queue:
        node = queue.popleft()
        # 부모 노드부터 하향식 스왑
        if node:
            node.left, node.right = node.right, node.left  # 2
            
            queue.append(node.left)
            queue.append(node.right)
        # 결과 변수를 처리하는 부분  # 3
    return root
```
<br>

가장 먼저 함수명과 리턴 타입부터 변경해보자.<br>
앞서 이 문제는 리턴 값을 문자열로 받는다고 설명했다.<br>
'# 1'코드부를 다음과 같이 변경해보자.
```python
def serialize(self, root: TreeNode) -> str:
    ...
```
<br>

다음으로 수정해야 할 부분은 node.left, node.right = node.right, node.left 코드부를 제거하는 것이다.<br>
스왑하는 부분인데, 이 문제는 스왑을 사용할 일이 없으므로 필요가 없다.<br>
따라서 '# 2' 코드부와 주석을 모두 제거해보자.<br>
또한 이 함수는 리턴 타입이 문자열인 만큼 리턴에 사용할 변수를 다음과 같이 별도로 설정한다.
```python
def serialize(self, root: TreeNode) -> str:
    queue = collections.deque([root])
    result = ['#']
    
    while queue:
        node = queue.popleft()
        if node:
            queue.append(node.left)
            queue.append(node.right)
        ...
    return result
```
불필요한 기능을 모두 제거하고 이제 BFS 탐색 코드의 뼈대가 갖춰졌다.<br>
바로 위 코드에서는 ...부분, 맨 앞의 가져온 코드에서는 '# 3'부분에 result 변수를 처리할 비즈니스 로직을 구현해준다면 구현이 모두 끝날 것이다.<br>
다음과 같이 그 부분을 구현해보자.
```python
def serialize(self, root: TreeNode) -> str:
    ...
    while queue:
        node = queue.popleft()
        if node:
            queue.append(node.left)
            queue.append(node.right)
            
            result.append(str(node.val))
        else:
            result.append('#')
    return result
```
result에는 현재 노드의 값을 추가했다.<br>
이렇게 하면 큐에는 BFS 탐색 결과가 계속 추가되면서 마지막 노드까지 차례대로 배열로 표현될 것이다.<br>
노드가 존재하지 않을 경우, 널이라는 의미로 #을 추가한다.

이제 마지막으로 result는 다음과 같이 리스트가 아닌 배열로 바꿔준다.
```python
return ' '.join(result)
```
이렇게 구현한 코드로 예제 입력값을 기준으로 직렬화한 출력 결과는 다음과 같다.

**# A B C # # D E # # # #**

뒷 부분에 #이 몇 개 더 붙어 있어서 우리가 기대한 결과와 조금 다르긴 하다.<br>
그러나 특별히 문제는 없다.<br>
각각 D,E의 왼쪽, 오른쪽 자식 노드로 존재하지 않기 때문에 #이 붙어 있는 것인데, 이렇게 붙어 있어도 아무런 문제가 없다.<br>
깔끔하게 정리하려면 당연히 뒤에 붙는 #은 모두 제거해야겠지만, 여기서는 더 이상 코드를 복잡하게 만들지 않기 위해 굳이 별도의 예외 처리는 추가로 진행하지 않는다.<br>
추후 스스로 직접 제거하는 코드를 만들어 볼 수 있을 것이다.

이제 이 결과를 역직렬화하는 함수를 구현해보자.
<br><br>

#### 역직렬화
동일하게 큐를 이용해 역직렬화를 진행해본다.<br>
먼저 문자열 형태인 입력값을 다음과 같이 처리할 수 있도록 준비한다.
```python
def deserailize(self, data: str) -> TreeNode:
    nodes = data.split()
    
    root = TreeNode(int(nodes[1]))
    queue = collections.deque([root])
    ...
```
split()으로 공백 단위로 문자열을 끊어서 nodes라는 리스트 변수로 만든다.<br>
그다음, 트리로 만들어줄 노드 변수 root부터 셋팅하고 큐 변수도 만들어준다.

이제 큐를 순회하면서 처리하면 되는데, 왼쪽 자식과 오른쪽 자식은 각각 별도의 인덱스를 부여받아 다음과 같이 nodes를 먼저 탐색해 나간다.<br>
마치 런너 기법에서 빠른 런너가 먼저 노드를 탐색해 나가는 것과 유사하다.
```python
def deserailize(self, data: str) -> TreeNode:
    ...
    index = 2
    while queue:
        node = queue.popleft()
        if nodes[index] is not '#':
            node.left = TreeNode(int(nodes[index]))
            queue.append(node.left)
        index += 1
        
        if nodes[index] is not '#':
            node.right = TreeNode(int(nodes[index]))
            queue.append(node.right)
        index += 1
    ...
```
#인 경우에는 큐에 삽입하지 않고, 아무런 처리도 하지 않는다.<br>
예를 들어 앞서 직렬화 되었던 입력값 # A B C # # D E # # # #은 E 이후에 더 이상 큐에 삽입되지 않으며, 빠른 런너처럼 훨씬 더 앞의 #은 읽어들이긴 하지만 노드의 값이 #이므로 아무런 처리도 하지 않을 것이다.<br>
이렇게 끝까지 순회하고 나면 원래의 트리 구조로 복원된다.<br>
예외 처리를 포함해 모두 정리하면 전체 코드는 다음과 같다.
```python
class Codec:
    # 직렬화
    def serialize(self, root: TreeNode) -> str:
        queue = collections.deque([root])
        result = ['#']
        # 트리 BFS 직렬화
        while queue:
            node = queue.popleft()
            if node:
                queue.append(node.left)
                queue.append(node.right)

                result.append(str(node.val))
            else:
                result.append('#')
        return ' '.join(result)
        
    # 역직렬화
    def deserailize(self, data: str) -> TreeNode:
        # 예외 처리
        if data == '# #':
            return None
            
        nodes = data.split()

        root = TreeNode(int(nodes[1]))
        queue = collections.deque([root])
        index = 2
        # 빠른 런너처럼 자식 노드 결과를 먼저 확인 후 큐 삽입
        while queue:
            node = queue.popleft()
            if nodes[index] is not '#':
                node.left = TreeNode(int(nodes[index]))
                queue.append(node.left)
            index += 1

            if nodes[index] is not '#':
                node.right = TreeNode(int(nodes[index]))
                queue.append(node.right)
            index += 1
        return root
```
<br><br>

* 실제로 구현할 수 있는 코드<br>
```python
import collections

class TreeNode(object):
  def __init__(self, val, left=None, right=None):
    self.val = val
    self.left = left
    self.right = right

class Codec:
    # 직렬화
    def serialize(self, root: TreeNode) -> str:
        queue = collections.deque([root])
        result = ['#']
        # 트리 BFS 직렬화
        while queue:
            node = queue.popleft()
            if node:
                queue.append(node.left)
                queue.append(node.right)

                result.append(str(node.val))
            else:
                result.append('#')
        return ' '.join(result)
        
    # 역직렬화
    def deserialize(self, data: str) -> TreeNode:
        # 예외 처리
        if data == '# #':
            return None
            
        nodes = data.split()

        root = TreeNode(int(nodes[1]))
        queue = collections.deque([root])
        index = 2
        # 빠른 런너처럼 자식 노드 결과를 먼저 확인 후 큐 삽입
        while queue:
            node = queue.popleft()
            if nodes[index] is not '#':
                node.left = TreeNode(int(nodes[index]))
                queue.append(node.left)
            index += 1

            if nodes[index] is not '#':
                node.right = TreeNode(int(nodes[index]))
                queue.append(node.right)
            index += 1
        return root
        
solution = Codec()
n1 = TreeNode(5)
n2 = TreeNode(4)
n3 = TreeNode(3,n2,n1)
n4 = TreeNode(2)
n5 = TreeNode(1,n4,n3)
result1 = solution.serialize(n5)
result2 = solution.deserialize(result1)
print(result1)
print(result2)
```
<br><br>



























