# 목차
* [chapter 12. 그래프](#12장-그래프)<br><br>
* [그래프 이론의 시작](#그래프-이론의-시작)
* [오일러 경로](#오일러-경로)
  + [오일러 경로와 오일러 회로](#오일러-경로와-오일러-회로)
* [해밀턴 경로](#해밀턴-경로)
* [참고. NP 복잡도](#참고-np-복잡도)
* [그래프 순회](#그래프-순회)
  + [DFS(깊이 우선 탐색)](#dfs깊이-우선-탐색)
    - [재귀 구조로 구현](#재귀-구조로-구현)
    - [스택을 이용한 반복 구조로 구현](#스택을-이용한-반복-구조로-구현)
  + [BFS(너비 우선 탐색)](#bfs너비-우선-탐색)
    - [큐를 이용한 반복 구조로 구현](#큐를-이용한-반복-구조로-구현)
    - [재귀 구현 불가](#재귀-구현-불가)
  + [백트래킹](#백트래킹)
  + [제약 충족 문제](#제약-충족-문제)<br><br>
* [출처](#출처)
<br><br><br>


# 12장 그래프
> 수학에서, 좀 더 구체적으로 그래프 이론에서 그래프란 객체의 일부 쌍(Pair)들이 '연관되어' 있는 객체 집합 구조를 말한다.

<br><br>

## 그래프 이론의 시작
지금으로부터 300여 년 전 프로이센 공국의 쾨니히스베르크(Königsberg)에는 프레겔 강이 흐르고 있었다.<br>
프레겔 강에는 2개의 큰 섬이 있었고, 섬과 도시를 연결하는 7개 다리가 놓여 있었다(그림 12-1(318p)은 오일러 시대의 쾨니히스베르크 지도다. 여기서는 6개의 다리만 보이는데, 마지막 1개는 이 그림이 그려지던 시기에는 아직 건설되기 전으로 보인다).<br>
어느날 도시의 시민 한 명이 "이 7개 다리를 한 번씩만 건너서 모두 지나갈 수 있을까?"라는 흥미로운 문제를 냈다.<br>
그러나 쉽게 풀릴 것처럼 보였던 이 문제를 풀 수 있는 이는 아무도 없었다.

'수학의 모차르트'라 불리는 레온하르트 오일러가 '쾨니히스베르크의 다리 문제'를 조사하기 시작했다.<br>
오일러는 이 문제가 도형 문제처럼 보이지만, 당시까지 알려진 기하학으로는 풀 수 없음을 알았다.<br>
그리고 미지의 영역에 그 해법이 있다는 사실을 천재적인 직관으로 간파했다.

이것이 바로 '그래프 이론'의 시작이다.

> 그림 12-1 오일러 시대의 쾨니히스베르크 지도

<br><br>
## 오일러 경로
그렇다면 오일러는 쾨니히스베르크 다리 문제를 풀었을까?<br>
오일러는 이 문제를 다음 그림 12-2(318p)와 같은 형태로 각각의 다리에 a부터 g까지 이름을 부여하고 도식화해 1735년에 논문을 발표했다.

> 그림 12-2 18세기 오일러가 논문에서 직접 그린 쾨니히스베르크 다리 스케치

논문에 포함된 이 스케치는 현대에 이르러 그래프 구조의 원형이 되었다.<br>
오일러의 스케치를 현대식 그래프 구조에 따라 나타낸 그림 12-3(319p)에서는 A부터 D까지를 정점(Vertex), a부터 g까지는 간선(Edge)으로 구성된 그래프라는 수학적 구조를 찾아볼 수 있다.<br>

> 그림 12-3 쾨니리스베르크 다리의 그래프 구조

오일러는 모든 정점이 짝수 개의 차수(Degree)를 갖는다면 모든 다리를 한 번씩만 건너서 도달하는 것이 성립한다고 말했다.<br>
그로부터 100년이 훨씬 더 지난 1873년, 독일의 수학자 칼 히어홀저(Carl Hierholzer)가 이를 수학적으로 증명해낸다.<br>
이를 '오일러의 정리(Euler's Theorem)'라 부른다.

아울러 모든 간선을 한 번씩 방문하는 유한 그래프(Finite Graph)를 일컬어 오일러 경로(Eulerian Trail/Eulerian Path)라 부르며, 어려서부터 즐겨 하던 놀이 중 하나인 '한붓 그리기'라고도 말한다.<br>
글자 그대로 한 번도 붓을 떼지 않고 모든 간선을 한 번씩만 그릴 수 있는지를 의미한다.<br>
그리고 마지막으로 가장 중요한 사항으로, 증명에 따르면 쾨니히스베르크의 다리는 모든 정점이 짝수개의 차수를 갖지 않으므로(심지어 짝수개의 차수를 갖는 정점은 하나도 없다) 오일러 경로가 아니다.
<br><br>

### 오일러 경로와 오일러 회로
**오일러 경로**란 그래프에 존재하는 모든 Edge를 정확히 1번씩만 방문하는 연속된 경로이다.<br>
이때 시작점과 도착점이 같으면 **오일러 회로**가 된다.

<img src="https://user-images.githubusercontent.com/55045377/117661166-fbc55d00-b1d8-11eb-8656-39d70c2a4f84.png" width=50% height=50%>

> 오일러 회로

가장 대표적으로 별 모양 그래프가 있는데 어느 점에서 시작하더라도 모두 출발점으로 되돌아 올 수 있다.<br>
즉 위 그래프는 오일러 회로가 존재한다.<br>
위 그래프를 다시 보면 모든 정점의 차수가 2(짝수)인데 시작점과 끝점을 제외하고서는 들어오는 간선이 있다면 반드시 나가는 간선이 하나 더 마련되어 있어야 하기 때문이다.<br>
그렇게 때문에 항상 차수가 2의 배수꼴로 붙게 된다.

오일러 경로의 존재 여부를 판단하는 방법은, 무향 그래프일 경우 Degree가 홀수인 정점이 2개일 때 오일러 경로가, 0개일 때 오일러 회로가 존재한다.

<img src="https://user-images.githubusercontent.com/55045377/117661604-84dc9400-b1d9-11eb-9d2c-58f7270a134d.png" width=50% height=50%>

> 오일러 경로는 존재하지만 오일러 회로는 존재하지 않는 그래프

오일러 경로는 시작점과 끝점을 차수가 홀수인 정점 2개로 하며, 오일러 회로만 존재한다면 그 어떤 정점을 시작점으로 뽑아도 만드는 것이 가능하다.
<br><br>

## 해밀턴 경로
> 해밀턴 경로는 각 정점을 한 번씩 방문하는 무향 또는 유향 그래프 경로를 말한다.

해밀턴 경로(Hamiltonian Path)와 오일러 경로의 차이점을 들자면, 오일러 경로는 간선을 기준으로 하고 해밀턴 경로는 정점을 기준으로 한다는 점이다.<br>
그러나 이러한 단순한 차이에도 불구하고 놀랍게도 해밀턴 경로를 찾는 문제는 최적 알고리즘이 없는 대표적인 NP-완전 문제다.(NP 문제 중 NP-난해(Hard)인 문제를 NP-완전 문제라 부른다)

원래의 출발점으로 돌아오는 경로는 특별히 해밀턴 순환(Hamiltonian Cycle)이라 하는데, 이중에서도 특히 최단 거리를 찾는 문제는 알고리즘 분야에서는 외판원 문제(Travelling Salesman Problem)(흔히 약어로 간단히 TSP라고 칭한다)로도 더욱 유명하다.<br>
외판원 문제란 각 도시를 방문하고 돌아오는 가장 짧은 경로를 찾는 문제, 즉 최단 거리인 해밀턴 순환을 찾는 문제이며, NP-난해 문제로 이론 컴퓨터과학 분야의 매우 중요한 문제 중 하나이기도 하다.

> 그림 12-4

외판원 문제를 좀 더 살펴보자. 그림 12-4(320p)에 각 도시의 위치가 표시된 미국 지도가 있다.<br>
각 도시를 한 번씩 방문한다고 했을 때, 어떤 순서로 방문해야 가장 짧은 거리가 될까?<br>
만약 도시가 20개라고 할 때 이 문제의 정답을 찾기 위해 다녀야 하는 총 경로의 수는 20!다.<br>
이 값은 얼마나 될까?<br>
20! = 2,432,902,008,176,640,000이다.<br>
그러니까 약 240경 번의 경로를 다녀봐야 가장 짧은 경로를 찾을 수 있다.<br>
문제는 단순하지만 정답은 실로 엄청나다.
<br><br>
* **cf) 포함 관계**<br>
  + 해밀턴 경로: 한 번만 방문하는 경로
  + 해밀턴 순환: 한 번만 방문하여 출발지로 돌아오는 경로
  + 외판원 문제: 한 번만 방문하여 출발지로 돌아오는 경로 중 가장 짧은 경로
  
  => 포함 관계 : 해밀턴 경로 > 해밀턴 순환 > 외판원 문제
<br><br>

## 참고. NP 복잡도
321p 참고
<br><br>

---

외판원 문제는 23장에서 살펴보게 될 다이나믹 프로그래밍 기법을 활용하면 좀 더 최적화할 수 있다.<br>
이 경우 O(n^2 2^n)로 최적화할 수 있는데, 앞서 n=20의 경우 419,430,400로, 여전히 엄청난 수치이긴 하지만 240경 번이었던 이전에 비해서는 훨씬 더 빠르게 계산할 수 있다.
<br><br>

## 그래프 순회
> 그래프 순회란 그래프 탐색(Search)이라고도 불리우며 그래프의 각 정점을 방문하는 과정을 말한다.

그래프의 각 정점을 방문하는 그래프 순회(Graph Traversals)에는 크게 깊이 우선 탐색(Depth-First Search)(이하 DFS)과 너비 우선 탐색(Breadth-First Search)(이하 BFS)의 2가지 알고리즘이 있다.<br>
DFS는 19세기 프랑스의 수학자 찰스 피에르 트레모가 미로 찾기를 풀기 위한 전략을 찾다가 고안한 것으로 알려져 있으며, 일반적으로 BFS에 비해 더 널리 쓰인다.<br>
코딩 테스트 시에도 대부분의 그래프 탐색은 DFS로 구현하게 될 것이다.

DFS는 주로 스택으로 구현하거나 재귀로 구현하며, 이후에 살펴볼 백트래킹을 통해 뛰어난 효용을 보인다.<br>
반면, BFS는 주로 큐로 구현하며, 그래프의 최단 경로를 구하는 문제 등에 사용된다.<br>
13장에서는 40번 '네크워크 딜레이 타임' 문제를 통해 다익스트라 알고리즘으로 최단 경로를 찾는 문제에서 BFS로 구현하는 코드를 살펴보게 될 것이다.

그렇다면 여기서는 먼저 그림 12-7(323p)과 같은 순회 그래프를 한번 살펴보자.

> 그림 12-7

그래프를 표현하는 방법에는 크게 인접 행렬(Adjacency Matrix)과 인접 리스트(Adjacency List)의 2가지 방법이 있는데, 여기서는 이 그림 12-7(323p)의 그래프를 인접 리스트로 표현할 것이다.<br>
인접 리스트는 출발 노드를 키로, 도착 노드를 값으로 표현할 수 있다.<br>
도착 노드는 여러 개가 될 수 있으므로 리스트 형태가 된다.<br>
파이썬의 딕셔너리 자료형으로 다음과 같이 나타낼 수 있다.
```python
graph = {
    1: [2, 3, 4],
    2: [5],
    3: [5],
    4: [],
    5: [6, 7],
    6: [],
    7: [3],
}
```
이제 이 딕셔너리를 입력값으로 해서 각각 DFS, BFS를 구현해보고 어떤 결과가 나오는지 살펴보자.
<br><br>

## DFS(깊이 우선 탐색)
먼저, DFS부터 구현해보자.<br>
일반적으로 DFS는 스택으로 구현하며, 재귀를 이용하면 좀 더 간단하게 구현할 수 있다.<br>
코딩 테스트 시에도 재귀 구현이 더 선호되는 편이다.
<br><br>

### 재귀 구조로 구현
재귀를 이용한 DFS를 구현해보자.<br>
먼저, 위키피디아에 제시된 수도코드는 리스트 12-1(324p)과 같다.

> 리스트 12-1 재귀를 이용한 DFS 구현 수도코드

```
DFS(G, v)
    label v as discovered
    for all directed edges from v to w that are in G.adjacentEdges(v) do
        if vertex w is not labeled as discovered then
            recursively call DFS(G, w)
```
이 수도코드에는 정점 v의 모든 인접 유향(Directed) 간선들을 반복하라고 표기되어 있다.<br>
이 수도코드의 알고리즘을 동일하게 파이썬 코드로 구현해보면 다음과 같다.
```python
def recursive_dfs(v, discovered=[]):
    discovered.append(v)
    for w in graph[v]:
        if w not in discovered:
            discovered = recursive_dfs(w, discovered)
    return discovered
```
방문했던 정점, 즉 discovered를 계속 누적된 결과로 만들기 위해 리턴하는 형태만 받아오도록 처리했을 뿐 다른 부분들은, 예를 들어 변수명까지 동일하게 수도코드와 맞춰서 작성해봤다.<br>
이제 그림 12-7(323p) 그래프를 입력값으로 한 탐색 결과는 다음과 같다.
```python
>>> f'recursive dfs: {recursive_dfs(1)}'
'recursive dfs: [1, 2, 5, 6, 7, 3, 4]'
```
이 결과가 맞는지 확인하기 위해 그림 12-8과 같이 DFS를 직접 손으로 그려가며 결과를 확인해봤다.

> 그림 12-8

그림 12-8(325p)에서 막다른 곳에 도달할 때까지 연속으로 진행되는 탐색이 총 3번에 걸쳐 진행됐는데 1->2->5->6까지 진행하고 그다음 되돌아갔다가 다음번 탐색은 7->3, 다시 되돌아 나가 마지막으로 루트까지 거슬러 올라가서 4를 탐색하고 종료하게 된다.<br>
최종 결과는 1->2->5->6->7->3->4로 손으로 탐색한 결과가 코드의 실행 결과 [1, 2, 5, 6, 7, 3, 4]와 완전히 동일함을 확인할 수 있다.
<br><br>

### 스택을 이용한 반복 구조로 구현
이번에는 스택을 이용한 반복 결과로 DFS를 구현해보자.<br>
수도코드는 다음과 같다.

> 리스트 12-2 반복을 이용한 DFS 구현 수도코드

```
DFS-iterative(G, v)
    let S be a stack
    S.push(v)
    while S is not empty do
        v = S.pop()
        if v is not labeled as discovered then
            label v as discovered
            for all edges from v to w in G.adjacentEdges(v) do
                S.push(w)
```
리스트 12-2의 수도코드는 스택을 이용해 모든 인접 간선을 추출하고 다시 도착점인 정점을 스택에 삽입하는 구조로 구현되어 있다.<br>
파이썬 코드로 구현하면 다음과 같다.
```python
def iterative_dfs(start_v):
    discovered = []
    stack = [start_v]
    while stack:
        v = stack.pop()
        if v not in discovered:
            discovered.append(v)
            for w in graph[v]:
                stack.append(w)
    return discovered
```
이와 같은 반복 구현은, 앞서 코드가 길고 빈틈없어 보이는 재귀 구현에 비해 우아함은 떨어지지만, 좀 더 직관적이라 이해하기는 훨씬 더 쉽다.<br>
실행 속도 또한 더 빠른 편이다.<br>
대부분의 경우 재귀 구현은 반복으로, 반복 구현은 재귀로 서로 바꿔서 알고리즘을 구현할 수 있으므로 자유롭게 바꿔가며 익숙해질 때까지 꾸준히 연습해보자.<br>
이제 그림 12-7(323p) 그래프를 입력값으로 한 탐색 결과는 다음과 같다.
```python
>>> f'iterative dfs: {iterative_dfs(1)}'
'iterative dfs: [1, 4, 3, 5, 7, 6, 2]'
```
똑같은 DFS인데 순서가 다르다. 어떤 차이가 있을까?

앞서 재귀 DFS는 사전식 순서로 방문한 데 반해 반복 DFS는 역순으로 방문했다.<br>
스택으로 구현하다 보니 가장 마지막에 삽입된 노드부터 꺼내서 반복하게 되고 이 경우 인접 노드에서 가장 최근에 담긴 노드, 즉 가장 마지막부터 방문하기 때문이다.<br>
인접 노드를 한꺼번에 추가하는 형태이기 때문에, 자칫 BFS가 아닌가 하고 헷갈릴 수 있지만 깊이 우선으로 탐색한다는 점에서 DFS가 맞다.<br>
만약 BFS라면 [..., 4, 3, 5, ...] 순서가 아니라 []..., 4, 3, 2, ...] 순서가 되어야 할 것이다.<br>
마찬가지로 손으로 DFS를 그려가며 역순으로 방문해보면 이 순서가 맞음을 확인할 수 있을 것이다.<br>
앞서 재귀 DFS와는 약간의 순서 차이가 있지만 이번에도 반복을 이용해 DFS를 잘 구현해봤다.
<br><br>

## BFS(너비 우선 탐색)
다음은 BFS를 구현해보자. BFS는 DFS보다 쓰임새는 적지만, 최단 경로를 찾는 다익스트라 알고리즘 등에 매우 유용하게 쓰인다.<br>
먼저 BFS를 반복 구조로 구현해보자.
<br><br>

### 큐를 이용한 반복 구조로 구현
스택을 이용하는 DFS와 달리, BFS를 반복 구조로 구현할 때는 큐를 이용한다.<br>
수도코드는 다음과 같다.

> 리스트 12-3 큐를 이용한 BFS 수도코드

```
BFS(G, start_v)
    let Q be a queue
    label start_v as discovered
    Q.enqueue(start_v)
    while Q is not empty do
        v := Q.dequeue()
        if v is the goal then
            return v
        for all edges from v to w in G.adjacentEdges(v) do
            if w is not labeled as discovered then
                label w as discovered
                w.parent := v
                Q.enqueue(w)
```
리스트 12-3은 모든 인접 간선을 추출하고 도착점인 정점을 큐에 삽입하는 수도코드다.<br>
이를 파이썬 코드로 구현하면 다음과 같다.
```python
def iterative_bfs(start_v):
    discovered = [start_v]
    queue = [start_v]
    while queue:
        v = queue.pop(0)
        for w in graph[v]:
            if w not in discovered:
                discovered.append(w)
                queue.append(w)
    return discovered
```
리스트 자료형을 사용했지만 pop(0)과 같은 큐의 연산만을 사용했다.<br>
따라서 큐를 사용하는 것과 사실상 동일하다.<br>
좀 더 최적화를 위해서는 데크 같은 자료형을 사용해 보는 것을 고민해볼 수 있다.<br>
그렇다면 과연 너비 우선으로도 잘 실행될까?<br>
마찬가지로 그림 12-7(323p) 그래프를 입력값으로 한 탐색 결과는 다음과 같다.
```python
>>> f'iterative bfs: {iterative_bfs(1)}'
'iterative bfs: [1, 2, 3, 4, 5, 6, 7]'
```
BFS의 경우 그림 12-7(323p)의 단계별 차례인 숫자 순으로 실행됐으며, 1부터 순서대로 각각의 인접노드를 우선으로 방문하는 너비 우선 탐색이 잘 실행됐음을 확인할 수 있다.
<br><br>

### 재귀 구현 불가
이번에는 BFS를 재귀로 풀이해볼 수 있을까?

사실 많은 이가 혼동하는 부분인데 BFS는 재귀로 동작하지 않는다.<br>
큐를 이용하는 반복 구현만 가능하다.<br>
이 부분을 혼동하면 풀이에 어려움을 겪을 수 있다.<br>
특히 제한 시간이 있는 온라인 코딩 테스트에서 풀이에 잘못 접근해 BFS를 재귀로 풀이하는 데 시간을 들인다면 끝내 구현하지 못해 시간을 많이 허비할 수 있다.<br>
따라서 BFS는 재귀를 사용할 수 없고 큐를 이용해 구현한다는 사실을 반드시 명심해두기 바란다.
<br><br>

## 백트래킹
> 백트래킹(Backtracking)은 해결책에 대한 후보를 구축해 나아가다 가능성이 없다고 판단되는 즉시 후보를 포기(백트랙(Backtrack))해 정답을 찾아가는 범용적인 알고리즘으로 제약 충족 문제(Constraint Satisfaction Problem)에 특히 유용하다.

DFS(깊이 우선 탐색)를 이야기하다 보면 항상 백트래킹이라는 단어가 함께 나온다.<br>
백트래킹은 DFS보다 좀 더 광의의 의미를 지닌다. 백트래킹은 탐색을 하다가 더 갈 수 없으면 왔던 길을 되돌아가 다른 길을 찾는다는 데서 유래했다. 백트래킹은 DFS와 같은 방식으로 탐색하는 모든 방법을 뜻하며, DFS는 백트래킹의 골격을 이루는 알고리즘이다. 백트래킹은 주로 재귀로 구현하며, 알고리즘마다 DFS 변형이 조금씩 일어나지만 기본적으로 모두 DFS의 범주에 속한다. 백트래킹은 가보고 되돌아오고를 반복한다.<br>
운이 좋으면 시행착오를 덜 거치고 목적지에 도착할 수 있지만 최악의 경우에는 모든 경우를 다 거친 다음에 도착할 수 있다. 이 때문에 브루트 포스와 유사하다.<br>
하지만 한번 방문 후 가능성이 없는 경우에는 즉시 후보를 포기한다는 점에서 매번 같은 경로를 방문하는 브루트 포스보다는 훨씬 더 우아한 방식이라 할 수 있다.

> 그림 12-9 탐색해야 하는 가지가 많은 트리

그림 12-9(329p)와 같이 큰 트리가 있을 때, 브루트 포스로 전체 트리를 탐색한다면 매우 긴 시간이 소요된다.<br>
하지만 그림 12-10(329p)과 같이 DFS로 탐색을 시도하고, 가능성이 없는 후보는 즉시 포기하고 백트래킹한다면 트리의 불필요한 거의 대부분을 버릴 수 있다.

> 그림 12-10 트리를 탐색하여 가능성이 없는 후보를 포기하는 백트래킹의 예

이를 트리의 가지치기(Pruning)라고 하며, 이처럼 불필요한 부분을 일찍 포기한다면 탐색을 최적화할 수 있기 때문에, 가지치기는 트리의 탐색 최적화 문제와도 관련이 깊다.
<br><br>

## 제약 충족 문제
백트래킹은 제약 충족 문제(Constraint Satisfaction Problems)(이하 CSP)를 풀이하는 데 필수적인 알고리즘이다.<br>
앞서 살펴본 가지치기를 통해 제약 충족 문제를 최적화 하기 때문이다.

> 제약 충족 문제란 수많은 제약 조건(Constraints)을 충족하는 상태(States)를 찾아내는 수학 문제를 일컫는다.

특히 제약 충족 문제는 인공지능이나 경영 과학 분야에서 심도 있게 연구되고 있으며, 합리적인 시간 내에 문제를 풀기 위해 휴리스틱과 조합 탐색 같은 개념을 함께 결합해 문제를 풀이한다.<br>
제약 충족 문제는 대표적으로 스도쿠(Sudoku)처럼 1에서 9까지 숫자를 한 번만 넣는(제약 조건 충족) 정답(상태)을 찾아내는 모든 문제 유형을 말한다.<br>
스도쿠를 잘 풀이하려면 백트래킹을 하면서 가지치기를 통해 최적화하는 형태로 풀이할 수 있다.<br>
스도쿠 외에도 십자말 풀이, 8퀸 문제, 4색 문제 같은 퍼즐 문제와 배낭 문제, 문자열 파싱, 조합 최적화 문제 등이 모두 제약 충족 문제에 속한다.
<br><br>

---

지금까지 그래프와 탐색, 특히 DFS와 백트래킹, 제약 충족 문제 등을 살펴봤다.<br>
이제 이를 이용한 몇 가지 문제를 직접 풀어보자.
<br><br>











# 출처
* 오일러 경로와 오일러 회로 [[오일러 경로와 오일러 회로](#오일러-경로와-오일러-회로)]<br>
  https://rain-bow.tistory.com/entry/%EC%98%A4%EC%9D%BC%EB%9F%AC-%EA%B2%BD%EB%A1%9C%EC%99%80-%ED%9A%8C%EB%A1%9CEulerian-trail-circuit
<br><br>


