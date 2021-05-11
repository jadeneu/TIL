# 목차

# 12장 그래프
## 리트코드
### 문제 32 섬의 개수
> 331p

* **내가 짠 코드**
예전에 dfs를 구현했던 내 코드를 보고 다시 구현해봤다. 그러나 예전에 구현할 때는 C++을 써서 파이썬답지 않을 수 있다.
```python
def dfs(graph, row, col, visited, m,n):
    visited[row][col] = 1  # 방문 표시

    # 오른쪽으로 갈 수 있다면
    if col < n-1:
      if graph[row][col+1] == 1 and visited[row][col+1] == 0:
        col = col+1  # 오른쪽으로 이동
        dfs(graph, row, col, visited, m, n)

    # 왼쪽으로 갈 수 있다면
    if col > 0:
      if graph[row][col-1] == 1 and visited[row][col-1] == 0:
        col = col-1  # 왼쪽으로 이동
        dfs(graph, row, col, visited, m, n)

    # 아래로 갈 수 있다면
    if row < m-1:
      if graph[row+1][col] == 1 and visited[row+1][col] == 0:
        row = row+1  # 아래로 이동
        dfs(graph, row, col, visited, m, n)

    # 위로 갈 수 있다면
    if row > 0:
      if graph[row-1][col] == 1 and visited[row-1][col] == 0:
        row = row-1
        dfs(graph, row, col, visited, m, n)

    return



m = 4  # 행
n = 5  # 열
cnt = 0  # 섬의 개수
graph = [[1,1,1,1,0],[1,1,0,1,0],[1,1,0,0,0],[0,0,0,0,0]]
visited = [[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0]]
# graph = [[1,1,0,0,0],[1,1,0,0,0],[0,0,1,0,0],[0,0,0,1,1]]
# visited = [[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0]]

for i in range(m):
  for j in range(n):
    # 섬이 육지이고 아직 방문하지 않았다면
    if graph[i][j] == 1 and visited[i][j] == 0:
      row = i
      col = j
      cnt += 1  # dfs 함수 호출할 때마다 1 증가
      dfs(graph, row, col, visited,m,n)

print(cnt)
```
<br><br>

### 문제 32 섬의 개수 풀이
#### 풀이1. DFS로 그래프 탐색
이 문제는 쾨니히스베르크의 다리 문제처럼 반드시 그래프 모양이 아니더라도 그래프 형으로 변환해서 풀이할 수 있음을 확인해보는 좋은 문제다.<br>
입력값이 정확히 그래프는 아니지만 사실상 동서남북이 모두 연결된 그래프로 가정하고 동일한 형태로 처리할 수 있으며, 네 방향 각각 DFS 재귀를 이용해 탐색을 끝마치면 1이 증가하는 형태로 육지의 개수를 파악할 수 있다.
<br><br>

```python
for i in range(len(grid)):
    for j in range(len(grid[0])):
        if grid[i][j] == '1':
            self.dfs(grid, i, j)
            ...
```
먼저, 여기서는 행렬(Matrix) 입력값인 grid의 행(Rows), 열(Cols) 단위로 육지(1)인 곳을 찾아 진행하다가 육지를 발견하면 그때부터 self.dfs()를 호출해 탐색을 시작한다.
<br><br>

```python
def dfs(self, grid, i, j):
    # 더 이상 땅이 아닌 경우 종료
    if i < 0 or i >= len(grid) or \
        j < 0 or j >] len(grid[0]) or \
        grid[i][j] != '1':
            return
            
    grid[i][j] = '0'
    
    # 동서남북 탐색
    self.dfs(grid, i+1, j)
    self.dfs(grid, i-1, j)
    self.dfs(grid, i, j+1)
    self.dfs(grid, i, j-1)
```
DFS 탐색을 하는 dfs() 함수는 동서남북을 모두 탐색하면서 재귀호출한다.<br>
함수 상단에는 육지가 아닌 곳은 return으로 종료 조건을 설정해둔다.<br>
이렇게 재귀 호출이 백트래킹으로 모두 빠져 나오면 섬 하나를 발견한 것으로 산주한다.

이때 이미 방문했던 곳은 1이 아닌 값으로 마킹한다.<br>
여기서는 grid[i][j] = '0'을 통해 다시 0으로 설정했는데, #으로 설정해도 좋고, 0으로 해도 아무 문제 없다.<br>
즉 육지(1)를 더 이상 육지가 아닌 것으로 만들기만 하면 된다. 그래야 다음에 다시 계산하는 경우가 생기지 않는다. 일종의 가지치기다.

간혹 또 다른 행렬을 생성해 그곳에 방문했던 경로를 저장하는 형태로 풀이하는 경우가 있는데, 이 문제는 그렇게 풀이할 필요가 없다.<br>
곰곰이 생각해보면 현재의 행렬에 방문한 경로를 표시해두는 것으로도 충분하기 때문이다.<br>
아울러 별도의 행렬을 생성할 경우 공간 복잡도가 O(n)이 되기 때문에 공간의 활용 또한 비효율적이다.
<br><br>

```python
...
self.dfs(grid, i, j)
count += 1
```
dfs() 함수를 빠져 나온 후에는 해당 위치에서 탐색할 수 있는 모든 육지를 탐색한 것이므로, 카운트를 1 증가시킨다.<br>
섬을 하나 발견한 것이다.<br>
이제 입력값이 비어 있는 경우에 대해 예외 처리를 포함한 전체 코드는 다음과 같다.
```python
class Solution:
    def dfs(self, grid: List[List[str]], i: int, j: int):
         # 더 이상 땅이 아닌 경우 종료
         if i < 0 or i >= len(grid) or \
              j < 0 or j >= len(grid[0]) or \
              grid[i][j] != '1':
                  return

          grid[i][j] = '0'
          # 동서남북 탐색
          self.dfs(grid, i+1, j)
          self.dfs(grid, i-1, j)
          self.dfs(grid, i, j+1)
          self.dfs(grid, i, j-1)
          
    def numIslands(self, grid: List[List[str]]) -> int:
        # 예외 처리
        if not grid:
            return 0
            
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    self.dfs(grid, i, j)
                    # 모든 육지 탐색 후 카운트 1 증가
                    count += 1
        return count
```
코드를 자세히 살펴보면 '# 동서남북 탐색'에서 dfs() 함수를 호출할 때마다 self.dfs(grid, i+1, j)와 같은 형태로 grid 변수를 매번 넘기는 것을 확인할 수 있다.<br>
그렇다면 이 부분을 좀 더 개선해볼 수 있을까?<br>
원래 리트코드의 풀이는 클래스로 선언되어 있으므로 먼저 다음과 같이 grid를 클래스의 멤버 변수로 처리해보자.
```python
class Solution:
    grid: List[List[str]]  # 클래스 멤버 변수로 선언
    
    def dfs(self, i: int, j: int):
        if i < 0 or i >= len(self.grid) or \
              j < 0 or j >= len(self.grid[0]) or \
              self.grid[i][j] != '1':
                  return
                  
                  
         ...
         
     def numIslands(self, grid: List[List[str]]) -> int:
         self.grid = grid
         ...
                     self.dfs(i, j)
         ...
         return count
```
이렇게 grid를 클래스 멤버 변수로 선언하여 클래스 내에서 모두 공유하게 되면 더 이상 grid를 매번 넘기지 않아도 되어 편리해지지만, 여전히 함수 호출 시 매번 self.가 따라 붙는 등 보기가 좋지 않다.<br>
당연히 불필요한 코드도 늘어난다. 그렇다면 좀 더 깔끔하게 정리할 수 있는 방법이 있을까?<br>
이번에는 파이썬의 중첩함수(Nested Function) 기능을 활용해 전체 코드를 다시 한번 작성해보자.
```python
def numIslands(self, grid: List[List[str]]) -> int:
    def dfs(i, j):
        # 더 이상 땅이 아닌 경우 종료
         if i < 0 or i >= len(grid) or \
              j < 0 or j >= len(grid[0]) or \
              grid[i][j] != '1':
                  return

          grid[i][j] = '0'
          # 동서남북 탐색
          dfs(i + 1, j)
          dfs(i - 1, j)
          dfs(i, j + 1)
          dfs(i, j - 1)
          
    count = 0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == '1':
                dfs(i, j)
                # 모든 육지 탐색 후 카운트 1 증가
                count += 1
    return count
```
numIslands() 함수 내에 dfs() 함수 전체가 중첩 함수로 들어갔다.<br>
물론 이렇게 하면 numIslands() 함수에서만 dfs() 함수 호출이 가능한 제약이 생긴다.<br>
하지만 중첩 함수는 부모 함수에서 선언한 변수도 유효한 범위 내에서 사용할 수 있다.<br>
원래 다른 함수에서 변수를 사용하려면 globals()로 선언하는 등이 필요하지만 여기서 dfs() 함수는 numIslands() 함수에서 선언된 변수를 자유롭게 사용 가능하다.<br>
덕분에 grid 변수뿐만 아니라 지저분하게 매번 따라다니던 self. 구문 또한 제거할 수 있어 dfs() 함수가 깔끔해졌고, 전반적으로 코드 전체가 훨씬 더 깔끔해졌다.
<br><br>

#### 파이썬. 중첩 함수
중첩 함수(Nested Function)란 함수 내에 위치한 또 다른 함수로, 바깥에 위치한 함수들과 달리 부모 함수의 변수를 자유롭게 읽을 수 있다는 장점이 있다.<br>
실무에서 자주 쓰이는 편은 아니지만 단일 함수로 해결해야 하는 경우가 잦은 코딩 테스트에서는 매우 자주 쓰이는 기능이다.<br>
이 책에서도 대부분의 문제 풀이는 중첩 함수를 적극적으로 활용해 풀이한다.<br>
중첩 함수가 부모 함수의 변수를 공유하는 예제는 다음과 같다.
```python
def outer_function(t: str):
    text: str = t
    
    def inner_function():
        print(text)
        
    inner_function()
    
outer_function('Hello!')
-------
Hello!
```
여기서 outer_function()은 inner_function()을 호출했고, 아무런 차라미터도 넘기지 않았지만 부모 함수의 text 변수를 자유롭게 읽어들여 그 값인 Hello!를 출력했다.<br>
이처럼 매번 파라미터를 전달하지 않아도 되기 때문에 구현이 깔끔해진다는 장점이 있다.<br>
아울러 가변 객체인 경우 append(), pop()등 여러 가지 연산으로 조작도 가능하다.<br>
그러나 재할당(=)이 일어날 경우 참조 ID가 변경되어 별도의 로컬 변수로 선언되므로 이 부분은 주의가 필요하다.

* **연산자 조작**<br>
  중첩 함수에서 부모 함수에서 선언한 변수를 연산자로 조작하는 경우를 살펴보자.
  ```python
  def outer_function(a: List[int]):
      b: List[int] = a
      print(id(b), b)
      
      def inner_function1():
          b.append(4)
          print(id(b), b)
          
      def inner_function2():
          print(id(b), b)
          
      inner_function1()
      inner_function2()
      
  outer_function([1,2,3])
  -----------------------
  4598336160 [1, 2, 3]
  4598336160 [1, 2, 3, 4]
  4598336160 [1, 2, 3, 4]
  ```
  리스트는 가변 객체이며, 이처럼 중첩 함수내에서 b.append(4)와 같은 형태로 append() 메소드를 사용해 변수를 조작할 수 있다.<br>
  이렇게 조작된 값은 부모 함수에서도 그대로 동일하게 적용된다.
<br><br>

* **재할당**<br>
  이번에는 재할당으로 참조 ID가 변경되는 경우를 살펴보자.
  ```python
  def outer_function(t: str):
      text: str = t
      print(id(text), text)
      
      def inner_function1():
          text = 'World!'
          print(id(text), text)
          
      def inner_function2():
          print(id(text), text)
          
      inner_function1()
      inner_function2()
      
  outer_function('Hello!')
  -----------------
  4599124144 Hello!
  4599130588 World!
  4599124144 Hello!
  ```
  여기서는 불변 객체인 문자형을 예로 들었다.<br>
  문자열은 불변 객체이기 때문에 조작할 수 없다. 값을 변경하려면 text = 'World!'와 같은 형태로 새롭게 재할당할 수밖에 없다.<br>
  = 연산자로 변수를 새롭게 할당하는 경우, 기존에 4599124144이었던 ID 값이 4599130288로 변경됨을 확인할 수 있다.<br>
  즉 참조 ID가 변경되어 서로 다른 변수가 된다.<br>
  중첩 함수인 경우에는 함수 내에서만 사용 가능한 새로운 로컬 변수로 선언되며, 여기서 수정된 값, 즉 **재할당된 값은 부모 함수에서는 반영되지 않으므로 주의가 필요하다.**
<br><br>
  
### 문제 33 전화 번호 문자 조합
> 338p
 
* **내가 짠 코드**<br>
```python

```
  
























