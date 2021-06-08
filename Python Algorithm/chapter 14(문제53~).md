# 목차

# 14장 트리
## 리트코드
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




























