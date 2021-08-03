# 목차
* [chapter 17. 정렬](#17장-정렬)
  + [버블 정렬](#버블-정렬)
  + [병합 정렬](#병합-정렬)
  + [퀵 정렬](#퀵-정렬)
    - [퀵 정렬(quick sort) 알고리즘의 개념 요약](#퀵-정렬quick-sort-알고리즘의-개념-요약)
    - [퀵 정렬(quick sort) 알고리즘의 예제](#퀵-정렬quick-sort-알고리즘의-예제)
  + [안정 정렬 vs 불안정 정렬](#안정-정렬-vs-불안정-정렬)
* [리트코드](#리트코드)
  + [문제 58 리스트 정렬](#문제-58-리스트-정렬)
  + [문제 58 리스트 정렬 풀이](#문제-58-리스트-정렬-풀이)
    - [풀이1. 병합 정렬](#풀이1-병합-정렬)
    - [풀이2. 퀵 정렬](#풀이2-퀵-정렬)
    - [풀이3. 내장 함수를 이용하는 실용적인 방법](#풀이3-내장-함수를-이용하는-실용적인-방법)
---
* [References](#references)
<br><br><br>


# 17장 정렬
> 정렬 알고리즘은 목록의 요소를 특정 순서대로 넣는 알고리즘이다. 대개 숫자식 순서(Numerical Order)와 사전식 순서(Lexicographical Order)로 정렬한다.

정렬(Sorting) 알고리즘은 간단하고 익숙할 뿐만 아니라 여러모로 유용하다.<br>
그러나 효율적으로 해결하기가 쉽지 않기 때문에, 컴퓨터과학 초기에 정렬은 많은 연구가 필요한 매력적인 주제 중 하나였다.<br>
뿐만 아니라 빅오, 분할 정복, 힙, 이진 트리, 시간과 공간의 트레이드오프(Time-Space Tradeoffs) 등 여러 가지 컴퓨터과학의 핵심 주제를 아우르기 때문에, 정렬은 알고리즘에 본격적으로 입문하기에도 매우 좋은 주제이기도 하다.

정렬은 알고리즘의 꽃이기도 하다.<br>
그 바탕을 구성하는 논리는 다른 코딩을 하는데도 많은 도움이 된다.<br>
그런 면에서 정렬을 실제로 구현할 일은 없을지언정, 공부를 통해 얻는 이득은 매우 크다. <br>
우리는 당장 내일 만들 무언가를 위해 기술을 익히는 용도로 공부를 하기도 하지만, 장기적인 관점에서 미래를 위한 꾸준히 기본기를 다져나가는 학습도 필요하기 때문이다.
<br><br>

## 버블 정렬
사실 따지고 보면, 버블 정렬은 전혀 중요한 알고리즘이 아니다.<br>
정렬 자체가 실무와는 다소 거리가 있기도 하지만 그중에서도 버블 정렬은 더욱 거리가 먼 알고리즘이다.<br>
전혀 중요하지 않다. 왜 중요하지 않다고 이야기했는지 아래 설명을 보자.

* **리스트 17-1** 버블 정렬 수도 코드<br>

  ```
  Bubblesort(A)
      for i from 1 to A.length
          for j from 0 to A.length - 1
              if A[j] > A[j + 1]
                  swap a[j] with a[j + 1]
  ```
리스트 17-1 의 수도코드를 보면 금방 알 수 있다.<br>
이 알고리즘은 n번의 라운드로 이뤄져 있으며, 각 라운드마다 배열의 아이템을 한 번씩 쭉 모두 살펴본다.<br>
연달아 있는 아이템 2개의 순서가 잘못되어 있는 것을 발견하면, 두 아이템을 맞바꾼다.<br>
배열 전체를 쭉 살펴보는 것을 n번 하기 때문에 **시간 복잡도는 항상 O(n^2)** 이다. <br>
이보다 더 비효율적일 수는 없으며 구현 가능한 가장 느린 정렬 알고리즘이다.

물론 스왑이 이뤄지지 않았는지 여부를 확인해 n^2보다 좀 더 최적화할 수 있지만, 그럼에도 다른 정렬 알고리즘과는 여전히 성능 차이가 있다.<br>
그래서 앞서, 중요하지 않은 알고리즘이라 거듭 강조한 것이다.
수도코드를 살펴 봤으니 이제 파이썬 구현은 가볍게 해볼 수 있을 것 같다.

```python
def bubblesort(A):
    for i in range(1, len(A)):
        for j in range(0, len(A) - 1):
            if A[j] > A[j + 1]:
                A[j], A[j + 1] = A[j + 1], A[j]
```
수도코드를 파이썬 코드로 직접 구현해봤다.<br>
i와 j를 비교하는 순서는 앞에서부터 할 수 있고 뒤에서부터 할 수도 있다.<br>
"Introduction to Algorithms" 책에서는 뒤에서부터 비교하도록 수도코드를 제시 하고 있으나, 여기서는 앞에서부터 비교를 진행했다. 큰 차이는 없다.
<br><br>

## 병합 정렬
병합 정렬(Merge Sort)은 컴퓨터과학 역사상 최고의 천재라 일컬어지는 존 폰 노이만(John von Neumann)이 1945년에 고안한 알고리즘으로, 분할 정복(Divide and Conquer) (22장 참조)의 진수를 보여주는 알고리즘이다.<br>
**최선과 최악 모두 O(n log n)인 사실상 완전한 θ(n log n)으로 일정한 알고리즘**이며, 대부분의 경우 퀵 정렬보다는 느리지만 일정한 실행 속도뿐만 아니라 무엇보다도 안정 정렬(Stable Sort)이라는 점에서 여전히 상용 라이브러리에 많이 쓰이고 있다.<br>
곧 풀어볼 58번 문제에서도, 병합 정렬의 고른 성능 덕분에 퀵 정렬로도 풀리지 않던 문제가 병합 정렬로는 잘 풀리는 경우를 확인하게 될 것이다.<br>
간단히 그림 17-3에서 병합 정렬의 도식화만 살펴보자.

<img src="https://user-images.githubusercontent.com/55045377/125877575-b4b00f9c-d7a8-4b8a-b9f6-533dded212a3.png" width=65% height=65%>

이 그림에서 우리는 분할 정복으로 일정하게 정렬이 이뤄지는 병합 정렬의 특징을 잘 파악할 수 있다.<br>
[38, 27, 43, 3, 9, 82, 10]인 입력값은 [38, 27, 43, 3]과 [9, 82, 10]으로 두 부분으로 분할, 다시 [38, 27], [43, 3], [9, 82 ], [10]으로 네 부분으로 분할 등의 방식으로 각각 더 이상 쪼갤 수 없을 때까지 계속해서 분할한 후, 분할이 끝나면 정렬하면서 정복해 나간다.<br>
분할 정복에 대해서는 22장에서 좀 더 자세히 살펴본다.

## 퀵 정렬
퀵 정렬(Quick Sort)은 영국의 컴퓨터과학자 토니 호어(Tony Hoare)가 1959년에 고안한 알고리즘으로, 피벗을 기준으로 좌우를 나누는 특징 때문에 파티션 교환 정렬(Partition-Exchange Sort)이라고도 불리운다. <br>
병합 정렬과 마찬가지로 분할 정복 알고리즘이며 여기에 피벗(Pivot)이라는 개념을 통해 피벗보다 작으면 왼쪽, 크면 오른쪽과 같은 방식으로 파티셔닝하면서 쪼개 나간다.<br>
여러 가지 변형과 개선 버전이 있는데 여기서는 N.로무토(Lomuto)가 구현한 파티션 계획(Partition Scheme)을 살펴본다.

로무토 파티션이란 항상 맨 오른쪽의 피벗을 택하는 단순한 방식으로, 토니 호어가 고안한 최초의 퀵 정렬 알고리즘보다도 훨씬 더 간결하고 이해하기 쉽기 때문에 퀵 정렬을 소개할 때는 항상 맨 처음에 언급되며, "Introduction to Algorithms"에서도 '퀵 정렬 맨 처음에 등장하는 가장 기본적인 방식이기도 하다.<br>
그렇다면 리스트 17-2의 수도코드를 기준으로 살펴보자.

* **리스트 17-2** 퀵 정렬 수도코드
  ```
  Quicksort(A, lo, hi)
      if lo < hi then
          pivot := partition(A, lo, hi)
          Quicksort(A, lo, pivot - 1)
          Quicksort(A, pivot + 1, hi)
  ```
이 수도코드는 퀵 정렬의 메인 함수에서부터 시작한다.<br>
파티션을 나누고 각각 재귀 호출하는 전형적인 분할 정복 구조를 띤다. <br>
이 수도코드를 실제 동작 가능한 파이썬 코드로 구현해보면 다음과 같다.
```python
def quicksort(A, lo, hi):
...
    if lo < hi:
        pivot = partition(lo, hi)
        quicksort(A, lo, pivot - 1)
        quicksort(A, pivot + 1, hi)
```
<br>

이제 리스트 17-3에서 파티션을 나누는 함수의 수도코드를 살펴보자.
* **리스트 17-3** 퀵 정렬 로무토 파티션 함수 수도코드

  ```
  partition(A, lo, hi)
      pivot := A[hi]
      i := lo
      for j := lo to hi do
          if A[j] < pivot then
              swap A[i] wlth A[j]
              i := i + 1
      swap A[i] with A[hi]
      return i
  ```
이 코드는 앞서 언급한 퀵 소트의 가장 간단한 분할 알고리즘인 로무토 파티션 수도코드로, 로무토 파티션은 맨 오른쪽을 피벗으로 정하는 가장 단순한 방식이다.<br>
이를 파이썬 코드로 구현하면 다음과 같다.<br>
수도코드와 기본적인 알고리즘은 동일하며, 변수명만 i, j에서 left, right로, 좀 더 직관적으로 살짝 수정해봤다.
```python
def partition(lo, hi):
    pivot = A[hi]
    left = lo
    for right in range(lo, hi):
        if A[right] < pivot:
            A[left], A[right] = A[right], A[left]
            left += 1
    A[left], A[hi] = A[hi], A[left]
    return left
```
여기서 피벗은 맨 오른쪽 값을 기준으로 하며, 이를 기준으로 2개의 포인터가 이동해서 오른쪽 포인터의 값이 피벗보다 작다면 서로 스왑하는 형태로 진행된다.<br>
이를 도식화해보면 다음 그림 17-4와 같다.

<img src="https://user-images.githubusercontent.com/55045377/126113632-8b83244b-d86f-44db-9363-cdacebd25265.png" width=50% height=50%>

이 그림에서 보듯이 오른쪽 right 포인터가 이동하면서 피벗의 값이 오른쪽 값보다 더 클 때, 왼쪽과 오른쪽의 스왑이 진행된다.<br>
스왑 이후에는 왼쪽 left 포인터가 함께 이동한다.<br>
여기서 피벗의 값은 4 이므로, 오른쪽 포인터가 끝에 도달하게 되면 4 미만인 값은 왼쪽으로, 4 이상인 값은 오른쪽에 위치하게 된다. <br>
그리고 왼쪽 포인터의 위치로 피벗 아이템이 이동한다.<br>
즉 그림 17-4에서 최종 결과인 8)을 보면, 4를 기준으로 작은 값은 왼쪽에, 큰 값은 오른쪽으로 분할되어 있고, 피벗이 그 중앙으로 이동하는 모습을 확인할 수 있다.<br>
이렇게 계속 분할하면서 정복을 진행하여 코드 기준으로 lo < hi를 만족하지 않을 때까지, 즉 서로 위치가 역전할 때까지 계속 재귀로 반복되면서 정렬이 완료된다.<br>
중첩 함수를 이용해 파이썬답게 구현해 좀 더 깔끔하게 정리한 전체 코드는 다음과 같다.
```python
def quicksort(A, lo, hi):
    def partition(lo, hi):
        pivot = A[hi]
        left = lo
        for right in range(lo, hi):
            if A[right] < pivot:
                A[left], A[right] = A[right], A[left]
                left += 1
        A[left], A[hi] = A[hi], A[left]
        return left
        
    if lo < hi:
        pivot = partition(lo, hi)
        quicksort(A, lo, pivot - 1)
        quicksort(A, pivot + 1, hi)
```
퀵 정렬은 그 이름처럼 매우 빠르며 굉장히 효율적인 알고리즘이다. 그러나 **최악의 경우에는 O(n^2)** 이 된다.<br>
만약 이미 정렬된 배열이 입력값으로 들어왔다고 가정해보자.<br>
이 경우 피벗은 계속 오른쪽에 위치하게 되므로 파티셔닝이 전혀 이뤄지지 않는다.<br>
이때 n번의 라운드에 걸쳐 결국 전체를 비교하기 때문에, 버블 정렬과 다를 바 없는 최악의 성능을 보이게 된다.<br>
항상 일정한 성능을 보이는 병합 정렬과 달리, 퀵 정렬은 이처럼 입력값에 따라 성능 편차가 심한 편이다.<br>
하지만 피벗을 선택하는 알고리즘을 개선해 퀵 정렬을 좀 더 최적화하는 등 이미 다양한 연구 결과가 많이 나와 있기도 하다.
<br><br>

### 퀵 정렬(quick sort) 알고리즘의 개념 요약
* **과정 설명**
  1. 리스트 안에 있는 한 요소를 선택한다. 이렇게 고른 원소를 피벗(pivot) 이라고 한다.
  2. 피벗을 기준으로 피벗보다 작은 요소들은 모두 피벗의 왼쪽으로 옮겨지고 피벗보다 큰 요소들은 모두 피벗의 오른쪽으로 옮겨진다. (피벗을 중심으로 왼쪽: 피벗보다 작은 요소들, 오른쪽: 피벗보다 큰 요소들)
  3. 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬한다.
      * 분할된 부분 리스트에 대하여 순환 호출 을 이용하여 정렬을 반복한다.
      * 부분 리스트에서도 다시 피벗을 정하고 피벗을 기준으로 2개의 부분 리스트로 나누는 과정을 반복한다.
  4. 부분 리스트들이 더 이상 분할이 불가능할 때까지 반복한다.
      * 부분 리스트들이 더 이상 분할이 불가능할 때까지 반복한다.<br><br>
        <img src="https://user-images.githubusercontent.com/55045377/126121380-afed4e11-df2f-4612-bc5e-88d9c795057f.png" width=60% height=60%>
        
<br><br>

### 퀵 정렬(quick sort) 알고리즘의 예제
배열에 5, 3, 8, 4, 9, 1, 6, 2, 7이 저장되어 있다고 가정하고 자료를 오름차순으로 정렬해 보자.<br><br>

<img src="https://user-images.githubusercontent.com/55045377/126122115-5db293b7-a0a3-4dc5-a76d-0ccf25a9a0af.png" width=80% height=80%>

<br><br>

## 안정 정렬 vs 불안정 정렬
> 안정 정렬(Stable Sort) 알고리즘은 중복된 값을 입력 순서와 동일하게 정렬한다.

퀵 정렬의 또 다른 문제점은 안정 정렬이 아니라는 점이다.<br>
예를 들어 다음 그림 17-5와 같이, 지역별 발송 시각을 시간 순으로 정렬한 택배 발송 로그가 있다고 가정해보자.

**안정 정렬의 경우에는** 기존의 시간 순으로 정렬했던 순서는 지역명으로 재정렬하더라도 기존 순서가 그대로 유지된 상태에서 정렬이 이뤄진다. 그러나 **불안정 정렬의 경우에는** 시간 순으로 정렬한 값을 지역명으로 재정렬하면 기존의 정렬 순서는 무시된 채 모두 뒤죽박죽 뒤섞이고 만다.

<img src="https://user-images.githubusercontent.com/55045377/126247727-b2e97d71-fb63-45e8-ab85-81f37ad78375.png" width=80% height=80%>

이처럼 입력값이 유지되는 안정 정렬 알고리즘이 유지되지 않는 불안정 정렬 알고리즘보다 훨씬 더 유용하리라는 점은 쉽게 예상할 수 있을 것이다.

대표적으로 병합 정렬은 안정 정렬이며, 심지어 버블 정렬 또한 안정 정렬이다.<br>
반면 퀵 정렬은 불안정 정렬이다. 게다가 입력값에 따라 버블 정렬 만큼이나 느려질 수 있다.<br>
그야말로 최고의 알고리즘으로 칭송받던 퀵 정렬이 경우에 따라서는 최악의 알고리즘이 될 수도 있다.<br>
이처럼 고르지 않은 성능 탓에 실무에서는 병합 정렬이 여전히 활발히 쓰이고 있으며, 파이썬의 기본 정렬 알고리즘으로는 6장 156페이지에서 살펴본 것처럼 병합 정렬과 삽입 정렬을 휴리스틱하게 조합한 팀소트(Timsort)를 사용한다.
<br><br>

# 리트코드
## 문제 58 리스트 정렬
> 489p

* 연결 리스트를 O(n log n)에 정렬하라.
* 입력
  ```
  4->2->1->3
  ```
* 출력
  ```
  1->2->3->4
  ```
<br><br>

* **내가 짠 코드**<br>
  풀이를 보고 코드를 짜봤다.
  ```python
  class Node(object):
  def __init__(self, val=0, next=None):
    self.val = val
    self.next = next

  class Solution(object):
    def sort_list(self, lst):
      if not(lst and lst.next):
        return lst

      half, slow, fast = None, lst, lst
      while fast and fast.next:
        half, slow, fast = slow, slow.next, fast.next.next
      half.next = None

      l1 = self.sort_list(lst)
      l2 = self.sort_list(slow)
      return self.merge(l1, l2)

    def merge(self, l1, l2):
      if l1 and l2:
        if l1.val > l2.val:
          l1, l2 = l2, l1
        l1.next = self.merge(l1.next, l2)
      return l1 or l2

  solution = Solution()
  n1 = Node(3)
  n2 = Node(1, n1)
  n3 = Node(2, n2)
  n4 = Node(4, n3)
  print(solution.sort_list(n4))
  ```
<br><br>

## 문제 58 리스트 정렬 풀이
### 풀이1. 병합 정렬
이 문제는 연결 리스트를 입력값으로 주기 때문에 직접 정렬을 구현해야 하는 좋은 문제지만, 시간 복잡도 O(n log n)으로 풀어야 하는 제약사항이 있다.<br>
당연히 버블 정렬 같은 알고리즘은 사용할 수 없으며 연결 리스트 입력에 대해서는 파이썬에서 정렬할 수 있는 별도의 함수를 제공하지 않기 때문에 직접 정렬 알고리즘을 구현해야 한다.

그렇다면 병합 정렬이나 퀵 정렬 정도를 생각할 수 있는데 연결 리스트는 특성상 피벗을 고정된 위치로 지정할 수밖에 없고 입력값에 따라 성능의 편차가 심하므로, 여기서는 우선 병합 정렬로 구현을 시도해보자. <br>
입력값에 따른 성능 편차는 이후 퀵 정렬 풀이에서 다시 자세하게 살펴볼 것이다.

입력값 -1->5->3->4->0 연결 리스트의 병합 정렬 과정을 도식화해보면 그림 17-6과 같다.

<img src="https://user-images.githubusercontent.com/55045377/126266980-c978cc30-47e4-4ca3-86de-9abda0cc7bb0.png" width=50% height=50%>

먼저 병합 정렬의 분할 정복(Divide and Conquer)을 위해서는 중앙을 분할(Divide)해야 한다.<br>
이 그림에서도 5를 기준으로 분할하는 과정이 잘 나타나 있다.<br>
그러나 연결 리스트는 전체 길이를 알 수 없기 때문에 여기서는 다음과 같이 런너(Runner) 기법을 활용해보자.

```python
half, slow, fast = None, head, head
while fast and fast.next:
    half, slow, fast = slow, slow.next, fast.next.next
half.next = None
```
half, slow, fast 3개 변수를 만들어 두고 slow는 한 칸씩, fast는 두 칸씩 앞으로 이동한다.<br>
**이렇게 하면 fast가 맨 끝에 도달했을 때 slow는 중앙에 도착해 있을 것이다.**<br>
half는 slow의 바로 이전 값으로 한다.<br>
그리고 마지막에는 half.next = None으로 half 위치를 기준으로 연결 리스트 관계를 끊어버린다. <br>
그림 17-6에서는 5가 half가 된다. 

이제 다음과 같이 분할해서 재귀 호출을 진행해보자.
```python
def sortList(self, head: ListNode) -> ListNode:
    ...
    l1 = self.sortList(head)
    l2 = self.sortList(slow)
```
head는 시작 노드이고, slow는 우리가 탐색을 통해 발견한 중앙 지점이다.<br>
그림 17-6에서는 3이 slow가 된다.<br>
이 값을 기준으로 계속 재귀 호출을 해나가면 결국 연결 리스트는 -1, 5, 3, 4, 0 단일 아이템으로 모두 쪼개진다.
```python
def sortList(self, head: ListNode) -> ListNode:
    ...
    return self.mergeTwoLists(l1, l2)
```
이처럼 마지막에는 최종적으로 쪼갰던 아이템을 다시 합쳐서 리턴한다. <br>
그냥 합치는 것이 아니라 다음처럼 크기 비교를 통해 정렬하면서 이어 붙인다.
```python
def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
    if l1 and l2:
        if l1.va1 > l2.va1:
            l1, l2 = l2, l1
        l1.next = self.mergeTwoLists(l1.next, l2)
        
    return l1 or l2
```
mergeTwoLists() 메소드는 연결 리스트를 입력값으로 받아, 길이가 얼마가 되든 재귀 호출을 통해 l1의 포인터를 이동하면서 정렬해 리턴한다.<br>
여기서 return l1 or l2는 l1에 값이 있다면 항상 l1을 리턴하고, l1이 None인 경우 l2를 리턴한다.<br>
즉 l1이 우선이며, 다음과 같은 간단한 실험을 통해 확인할 수 있다.
```python
>>> 1 or None
1
>>> 1 or 2
1
>>> None or 2
2
```

이 부분은 앞서 8장 14번 '두 정렬 리스트 병합' 문제와 동일한 방식으로 풀이가 가능하다.<br>
그때는 다음과 같은 코드로 풀이했다.
```python
def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
    if (not l1) or (l2 and l1.va1 > l2.va1):
        l1, l2 = l2, l1
    if l1:
        l1.next = se1f.mergeTwoLists(l1.next, l2)
    return l1
```
굳이 l1 or l2를 비교하지 않고 l1이 None이라면 미리 스왑해버리는 방식인데, 어떤 식으로 풀이하든 결과는 동일하다.<br>
둘 중 아무거나 마음에 드는 구현을 택하면 된다.

그렇다면 과연 이 함수가 잘 동작하는지, 최종적으로 비교가 진행될 -1->5와 0->3->4의 마지막 정렬 부분을 그림으로 나타내 자세히 살펴보자.

<img src="https://user-images.githubusercontent.com/55045377/126434287-0a59ee03-fef6-44bf-bd3e-f01ae1496fe6.png" width=50% height=50%>

코드에서 처음에는 l1에 -1, l2에는 0이 입력값으로 들어오고, l1의 next인 5는 0보다 크기 때문에 스왑한다.<br>
이후에는 스왑 없이 계속 next 순서로 3, 4로 연결된다.<br>
마지막에는 4의 next가 None이고, 이 경우 return l1 or l2 부분에서 l2 값이 리턴되며, 즉 5가 리턴된다.<br>
그림 17-7과 같은 순서로 진행이 되며, 재귀 호출된 부분들을 거슬러서 풀어보면 -1->0->3->4->5. <br>
최종 정렬이 잘 진행됐음을 확인할 수 있다.

이제 전체 코드는 다음과 같이 깔끔하게 정리할수 있다.
```python
# 두 정렬 리스트 병합
def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
    if l1 and l2:
        if l1.val > l2.val:
            l1, l2 = l2, l1
        l1.next = self.mergeTwoLists(l1.next, l2)
        
    return l1 or l2
    
def sortList(self, head: ListNode) -> ListNode:
    if not (head and head.next):
        return head
        
    # 런너 기법 활용
    half, slow, fast = None, head, head
    while fast and fast.next:
        half, slow, fast = slow, slow.next, fast.next.next
    half.next = None
    
    # 분할 재귀 호출
    l1 = self.sortList(head)
    l2 = self.sortList(slow)
    
    return self.mergeTwoLists(l1, l2)
```
이 풀이는 **320밀리초**가 걸려, 나쁘지 않은 실행 시간 속도를 보인다.

<br>

* 실제로 돌려볼 수 있는 코드
```python
class ListNode(object):
  def __init__(self, val=0, next=None):
    self.val = val
    self.next = next

class Solution(object):
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 and l2:
            if l1.val > l2.val:
                l1, l2 = l2, l1
            l1.next = self.mergeTwoLists(l1.next, l2)
            
        return l1 or l2
        
    def sortList(self, head: ListNode) -> ListNode:
        if not (head and head.next):
            return head
            
        # 런너 기법 활용
        half, slow, fast = None, head, head
        while fast and fast.next:
            half, slow, fast = slow, slow.next, fast.next.next
        half.next = None
        
        # 분할 재귀 호출
        l1 = self.sortList(head)
        l2 = self.sortList(slow)
        
        return self.mergeTwoLists(l1, l2)
    
solution = Solution()
n1 = ListNode(3)
n2 = ListNode(1, n1)
n3 = ListNode(2, n2)
n4 = ListNode(4, n3)
print(solution.sortList(n4))
```
<br><br><br>

### 풀이2. 퀵 정렬
그렇다면 이 문제를 퀵 정렬로 풀 수 있을까? 그리고 더 빨리 풀 수 있을까? <br>
당연히 퀵 정렬로 풀 수는 있다. 그러나 이 문제를 로무토 파티션 등의 일반적인 퀵 정렬 알고리즘으로 구현하면, 타임아웃이 발생해 풀이가 불가능하다.<br>
퀵 정렬을 변형해 좀 더 최적화를 진행해야 풀이가 가능하다.<br>
하지만, 해당 풀이는 이 책의 범위를 벗어나므로 여기서는 설명하지 않는다.

직접 퀵 정렬을 그대로 구현해 테스트해본 결과, 파이썬에서는 타임아웃이 나며 풀이가 불가능했다.<br>
같은 알고리즘으로 자바와 C++는 풀이가 가능했으나, 병합 정렬보다 한참 더 오래 걸렸다. 겨우 풀이가 가능한 정도였다.<br>
문제가 발생하는 입력값은 다음과 같은 경우였다.
```
# 1,000개 입력값
[1,3,3,1,3,1,3,3,2,3,2,2,1,1,1,3,2,2,1,1,2,2,2,3,3,1,1,2,
1,2,1,3,3,2,2,1,3,1,3,1,3, ... ,3,1,2,2,2,1,3,3,3,2,3,1,
1,2,3,3,3,1,3,3,1,2,3,1,3,1,3,2,1,1,3,3,3,2,2,3,3]
```
이렇게 1, 2, 3, 단 3개의 아이템으로 구성된 1,000개 입력값으로 이뤄진 테스트 케이스가 있는데, 이 부분에서 파이썬의 퀵 정렬은 계속 타임아웃을 발생했다. <br>
퀵 정렬은 대표적인 불안정 정렬로, 같은 값이 반복될 경우에도 계속해서 스왑을 시도한다. <br>
연결 리스트는 특성상 피벗을 임의로 지정하기가 어렵기 때문에 여기서는 피벗을 항상 첫 번째 값으로 설정했는데,<br>
이 경우 이미 정렬된 리스트가 입력값으로 들어오게 되면 계속해서 불균형 리스트로 나뉘기 때문에 그림 17-8의 2)와 같은 최악의 결과를 보일 수 있다.

<img src="https://user-images.githubusercontent.com/55045377/126577494-e05370d1-707a-453d-a6cd-b212ed1444a5.png" width=70% height=70%>

이러한 문제는 이미 퀵 정렬을 소개할 때 충분히 언급한 바 있다.<br>
이처럼 퀵 정렬은 입력 값에 따라 성능의 편차가 크고 다루기가 까다롭기 때문에, 성능이 우수함에도 실무에서는 좀처럼 퀵 정렬을 사용하지 않는 이유이기도 하다.<br>
이 문제 또한 퀵 정렬로 풀이하기가 어렵다. 특히 동일한 방식으로 자바나 C++로 풀었을 때는 통과했지만, 파이썬은 계속해서 통과하지 못했다.<br>
리트코드를 비롯한 여타 코딩 테스트 플랫폼들의 타임아웃 설정의 조금 아쉬운 부분이기도 하다(동일 알고리즘으로 풀면 C++로는 풀이가 되는데, 파이썬에서는 타임이웃이 발생한다).

그렇다면 이 문제를 시간 내에 통과할 수 있는 또 다른 방법은 없을까?
<br><br><br>

### 풀이3. 내장 함수를 이용하는 실용적인 방법
사실 이 문제는 정렬 알고리즘을 직접 구현할 필요가 없다.<br>
요즘 대부분의 프로그래밍 언어들은 표준 라이브러리에서 이미 성능 좋은 정렬 알고리즘을 제공하고 있기 때문이다.<br>
앞서 봤듯이 파이썬에서도 .sort()와 sorted()를 통해 효율적인 정렬 알고리즘을 제공하고 있다. <br>
팀소트(Timsort)를 사용한다는 점 또한 이미 살펴본 바 있다.<br>
그렇다면 기본 라이브러리에서 제공하는 함수를 이용해 풀이를 진행할 수 있을까?<br>
만약 기본 라이브러리의 정렬 함수를 사용한다면 속도가 가장 빠를 것이다. C로 신중하게 구현한 팀소트를 사용하기 때문이다.<br>
실행 시간을 조금이라도 더 줄여야 하는 코딩 테스트에서는 가장 현명한 접근 방식이기도 하며, 실무에서도 당연히 이 같은 방식으로 풀이하는 편이 훨씬 더 실용적이다.

그렇다면 내장 함수를 이용하는 실용적인 풀이를 한번 진행해보자.<br>
먼저, 다음처럼 연결 리스트를 파이썬의 리스트로 만든다.
```python
lst: List = []
while p:
    lst.append(p.val)
    p = p.next
```

그다음, 파이썬의 내장 정렬 함수인 sort()를 다음과 같이 수행한다.
```python
lst.sort()
```

정렬한 리스트는 다음과 같이 다시 연결 리스트로 만든다.
```python
p = head
for i in range(len(lst)):
    p.val = lst[i]
    p = p.next
return head
```
여기서 p는 포인터다.<br>
연결 리스트를 순회하기 위한 포인터 변수를 p로 설정했고, 순회하며 값을 채워넣는 역할을 한다.<br>
그런 다음 루트 노드 head를 리턴한다.<br>
이제 전체 코드는 다음과 같다.
```python
def sortList(self, head: ListNode) -> ListNode:
    # 연결 리스트 -> 파이썬 리스트
    p = head
    lst: List = []
    while p:
        lst.append(p.val)
        p = p.next
        
    # 정렬
    lst.sort()
    
    # 파이썬 리스트 -> 연결 리스트
    p = head
    for i in range(len(lst)):
        p.val = lst[i]
        p = p.next
    return head
```
이 풀이는 **84밀리초**에 풀린다.<br>
무엇보다 단순하면서도, 직관적이고, 쉽다. <br>
놀라운 점은 연결 리스트를 리스트로 변경하는 과정이 추가됐음에도 이 방식이 가장 빠르다는 점이다.<br>
이처럼 실무에서나 코딩 테스트 시에 내장함수를 잘 활용하면 매우 효율적이기 때문에, 별도의 제약사항이 없다면 현명하게 활용할 필요가 있다.

<br><br><br>
























---
# References
* https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html
  
<br><br><br>
