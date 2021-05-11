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
              j < 0 or j >] len(grid[0]) or \
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




























