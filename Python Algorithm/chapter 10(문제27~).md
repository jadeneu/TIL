# 목차


# 10장 데크, 우선순위 큐
## 우선순위 큐
> 우선순위 큐는 큐 또는 스택과 같은 추상 자료형과 유사하지만 추가로 각 요소의 '우선순위'와 연관되어 있다.

스택은 가장 나중에 삽입된 요소를 먼저 추출하고, 큐는 가장 먼저 삽입된 요소를 먼저 추출한다.<br>
그러나 이와 달리 **우선순위 큐**는 어떠한 특정 조건에 따라 우선순위가 가장 높은 요소가 추출되는 자료형이다.<br>
대표적으로 최댓값 추출을 들 수 있는데, 예를 들어 큐에 **[1, 4, 5, 3, 2]** 가 들어 있고 최댓값을 추출하는 우선순위 큐가 있다고 가정하자.<br>
이 경우 항상 남아 있는 요소들의 최댓값이 우선순위에 따라 추출되어 **5, 4, 3, 2, 1** 순으로 추출된다.

이는 정렬 알고리즘을 사용하면 우선순위 큐를 만들 수 있다는 의미이기도 하다.<br>
n개의 요소를 정렬하는 데 S(n)의 시간이 든다고 할 때, 새 요소를 삽입하거나 요소를 삭제하는데는 O(S(n))의 시간이 걸린다.<br>
반면 내림차순으로 정렬했을 때 최댓값을 가져오는 데는 맨 앞의 값을 가져오기만 하면 되므로 O(1)로 가능하다.<br>
대개 정렬에는 O(nlogn)이 필요하기 때문에 O(S(n))은 O(nlogn)정도가 든다. 그러나 실제로는 이처럼 단순 정렬보다는 힙 정렬 등의 좀 더 효율적인 방법을 활용한다.

이외에도 최단 경로를 탐색하는 다익스트라(Dijkstra) 알고리즘 등 우선순위 큐는 다양한 분야에 활용되며 힙(Heap) 자료구조와도 관련이 깊다.
<br><br>

## 리트코드
### 문제 27 k개 정렬 리스트 병합
> 274p

* **내가 짠 코드**<br>

<br><br>
### 문제 27 k개 정렬 리스트 병합 풀이
#### 풀이1. 우선순위 큐를 이용한 리스트 병합
앞서 우선순위 큐를 설명할 때 힙과 관련이 깊다고 했다. 특히 파이썬에서는 대부분의 우선순위 큐 풀이에 거의 항상 heapq 모듈을 사용하므로 잘 파악해두자. 왜 PriorityQueue 모듈을 사용하지 않고 heapq를 사용하는지는 뒤에 다시 설명한다.
```python
for lst in list:
    heapq.heappush(heap, (lst.val, lst))
```
lists는 3개의 연결 리스트인 [[1, 4, 5], [1, 3, 4], [2, 6]]로 구성된 입력값이며, 이 코드는 각 연결 리스트의 루트를 힙에 저장하게 된다.<br>
다시 설명하겠지만 파이썬의 heapq 모듈은 최소 힙(Min Heap)을 지원하며, 따라서 lst.val이 작은 순서대로 pop()할 수 있다.<br>
그런데 이렇게 저장하면 TypeError: '<' not supported between instances of 'ListNode' and 'ListNode'라는 에러가 발생한다. 이 에러 메시지는 '중복된 값을 넣을 수 없다'라는 뜻이다.

이 문제의 예제로 제시한 입력값은 3개의 연결 리스트 중 첫 번째와 두 번째의 루트가 각각 1로 동일하다.<br>
이렇게 동일한 값은 heappush() 함수에서 에러를 발생하기 때문에 중복된 값을 구분할 수 있는 추가 인자가 필요하다.<br>
따라서 사용할 일은 없지만, 오로지 에러를 방지하게 위한 용도로 연결 리스트의 순서를 적절히 삽입해보자.

코드는 다음과 같은 형태로 수정한다.
```python
for i in range(len(lists)):
    ...
    heapq.heappush(heap, (lists[i].val, i, lists[i]))
```
이제 heappop()으로 값을 추출하면 가장 작은 노드의 연결 리스트로부터 차례대로 나오게 되며, 이 값을 결과가 될 노드 result에 하나씩 추가한다.<br>
아울러 k개의 연결 리스트가 모두 힙에 계속 들어 있어야 그중에서 가장 작은 노드가 항상 차례대로 나올 수 있으므로 추출한 연결 리스트의 그다음 노드는 다음과 같이 다시 힙에 추가한다.
```python
while heap:
    node = heapq.heappop(heap)
    idx = node[1]
    result.next = node[2]
    
    result = result.next
    if result.next:
        heapq.heappush(heap, (result.next.val, idx, result.next))
```
이렇게 힙에 아무 값도 남지 않을 때까지 반복하면 result에는 작은 노드부터 차례대로 연결된다.

전체 코드를 정리하면 다음과 같다.
```python
def mergeKLists(self, lists: List[ListNode]) -> ListNode:
    root = result = ListNode(None)
    heap = []
    
    # 각 연결 리스트의 루트를 힙에 저장
    for i in range(len(lists)):
        if lists[i]:
            heapq.heappush(heap, (lists[i].val, i, lists[i]))
            
    # 힙 추출 이후 다음 노드는 다시 저장
    while heap:
        node = heapq.heappop(heap)
        idx = node[1]
        result.next = node[2]
        
        result = result.next
        if result.next:
            heapq.heappush(heap, (result.next.val, idx, result.next))
            
    return root.next
```
우선순위 큐 문제는 힙 문제와 사실상 중복되므로, 다른 우선순위 큐 문제들은 15장에서 다시 풀이해본다.
<br><br>

#### 파이썬. PriorityQueue vs heapq

























