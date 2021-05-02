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
그런데 이렇게 저장하면 **TypeError: '<' not supported between instances of 'ListNode' and 'ListNode'** 라는 에러가 발생한다. 이 에러 메시지는 '중복된 값을 넣을 수 없다'라는 뜻이다.

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

### heapq 모듈
heapq 모듈은 이진 트리(binary tree) 기반의 최소 힙(min heap) 자료구조를 제공한다.

min heap을 사용하면 원소들이 항상 정렬된 상태로 추가되고 삭제되며, min heap에서 가장 작은값은 언제나 인덱스 0, 즉, 이진 트리의 루트에 위치한다.<br> 
내부적으로 min heap 내의 모든 원소(k)는 항상 자식 원소들(2k+1, 2k+2) 보다 크기가 작거나 같도록 원소가 추가되고 삭제된다.

* **모듈 임포트**<br>
  우선 heapq 모듈은 내장 모듈이기 때문에 파이썬만 설치되어 있으면 다음과 같이 간단하게 임포트 후에 힙 관련 함수를 사용할 수 있다.
  ```python
  import heapq
  ```
  
* **최소 힙 생성**<br>
  heapq 모듈에은 파이썬의 보통 리스트를 마치 최소 힙처럼 다룰 수 있도록 도와준다.<br>
  그렇기 때문에, 그냥 빈 리스트를 생성해놓은 다음 heapq 모듈의 함수를 호출할 때 마다 이 리스트를 인자로 넘겨야 한다.<br>
  다시말해, 파이썬에서는 heapq 모듈을 통해서 원소를 추가하거나 삭제한 리스트가 그냥 최소 힙이다.
  ```python
  heap = []
  ```
  
* **힙에 원소 추가**<br>
  heapq 모듈의 **heappush() 함수**를 이용하여 힙에 원소를 추가할 수 있다. 첫번째 인자는 원소를 추가할 대상 리스트이며 두번째 인자는 추가할 원소를 넘긴다.
  ```python
  heapq.heappush(heap, 4)
  heapq.heappush(heap, 1)
  heapq.heappush(heap, 7)
  heapq.heappush(heap, 3)
  print(heap)
  ```
  ```python
  [1, 3, 7, 4]
  ```
  가장 작은 1이 인덱스 0에 위치하며, 인덱스 1(= k)에 위치한 3은 인덱스 3(= 2k + 1)에 위치한 4보다 크므로 힙의 공식을 만족한다.<br>
  내부적으로 이진 트리에 원소를 추가하는 heappush() 함수는 O(logn)의 시간 복잡도를 가진다.
  
* **힙에서 원소 삭제**<br>
  heapq 모듈의 heappop() 함수를 이용하여 힙에서 원소를 삭제할 수 있다.<br>
  원소를 삭제할 대상 리스트를 인자로 넘기면, 가장 작은 원소를 삭제 후에 그 값을 리턴한다.
  ```python
  print(heapq.heappop(heap))
  print(heap)
  ```
  ```python
  1
  [3, 4, 7]
  ```
  가장 작았던 1이 삭제되어 리턴되었고, 그 다음으로 작었던 3이 인덱스 0으로 올라왔다.
  ```python
  print(heapq.heappop(heap))
  print(heap)
  ```
  ```python
  3
  [4, 7]
  ```
  가장 작었던 3이 삭제되어 리턴되었고, 그 다음으로 작았던 4가 인덱스 0으로 올라왔다.<br>
  내부적으로 이진 트리로부터 원소를 삭제하는 heappop() 함수도 역시 O(logn)의 시간 복잡도를 갖는다.
  
* **최소값 삭제하지 않고 얻기**<br>
  힙에서 최소값을 삭제하지 않고 단순히 읽기만 하려면 일반적으로 리스트의 첫번째 원소에 접근하듯이 인덱스를 통해 접근하면 된다.
  ```python
  print(heap[0])
  ```
  ```python
  4
  ```
  여기서 주의사항은 인덱스 0에 가장 작은 원소가 있다고 해서, 인덱스 1에 두번째 작은 원소, 인덱스 2에 세번째 작은 원소가 있다는 보장은 없다는 것이다.<br>
  왜냐하면 힙은 heappop() 함수를 호출하여 원소를 삭제할 때마다 이진 트리의 재배치를 통해 매번 새로운 최소값을 인덱스 0에 위치시키기 때문이다.
  
  따라서 두번째로 작은 원소를 얻으려면 바로 heap[1]으로 접근하면 안되고, 반드시 heappop()을 통해 가장 작은 원소를 삭제 후에 heap[0]를 통해 새로운 최소값에 접근해야 한다.
  
* **기존 리스트를 힙으로 변환**<br>
  이미 원소가 들어있는 리스트 힙으로 만들려면 heapq 모듈의 heapify()라는 함수에 사용하면 됩니다.
  ```python
  heap = [4, 1, 7, 3, 8, 5]
  heapq.heapify(heap)
  print(heap)
  ```
  ```python
  [1, 3, 5, 4, 8, 7]
  ```
  heapify() 함수에 리스트를 인자로 넘기면 리스트 내부의 원소들의 위에서 다룬 힙 구조에 맞게 재배치되며 최소값이 0번째 인덱스에 위치된다.<br>
  즉, 비어있는 리스트를 생성한 후 heappush() 함수로 원소를 하나씩 추가한 효과가 난다.<br>
  heapify() 함수의 성능은 인자로 넘기는 리스트의 원소수에 비례한다. 즉 O(n)의 시간 복잡도를 갖는다.
  
* **[응용] 힙 정렬**<br>
  힙 정렬(heap sort)은 힙 자료구조의 성질을 이용한 대표적인 정렬 알고리즘이다.
  ```python
  import heapq

  def heap_sort(nums):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
  
    sorted_nums = []
    while heap:
        sorted_nums.append(heapq.heappop(heap))
    return sorted_nums

    print(heap_sort([4, 1, 7, 3, 8, 5]))
  ```
  ```python
  [1, 3, 4, 5, 7, 8]
  ```
<br><br>

### 파이썬. PriorityQueue vs heapq








# 출처
* [heapq 모듈](#heapq-모듈)<br>
  https://www.daleseo.com/python-heapq/
























