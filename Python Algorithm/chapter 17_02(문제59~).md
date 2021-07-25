# 목차

# 17장. 정렬
## 문제 59 구간 병합
* 겹치는 구간을 병합하라.
* 입력
```
[[1,3],[2,6],[8,10],[15,18]]
```
* 출력
```
[[1,6],[8,10],[15,18]]
```
* 설명<br>
  [1,3]과 [2,6]이 겹치므로 [1,6]이 된다.
  
<br><br>

* **내가 짠 코드**
```python

```

<br><br>

## 문제 59 구간 병합 풀이
### 풀이1. 정렬하여 병합
이 문제를 풀기 위해서는 먼저 정렬을 수행한다. 정렬 순서는 첫 번째 값을 기준으로 한다.
```python
sorted(intervals, key=lambda x: x[0])
```
람다를 이용하면 첫 번째 값을 키로 이용하라는 지시를 할 수 있다. <br>
두 번째 값을 사용할 경우에는 당연히 x[1]로 지정하면 된다.<br>
그렇게 했을 때 현재 아이템의 시작이 이전 아이템의 끝과 겹치게 되면 그림 17-9와 같이 최댓값을 기준으로 병합하는 형태로 계속 반복해 나간다.

<img src="https://user-images.githubusercontent.com/55045377/126862200-ec2a3695-9b37-4f6a-83ce-0d5d4f00a9ae.png" width=70% height=70%>

이 그림에서 (1,3)은 (2, 6)과 병합되어 (1, 6)이 되었다.<br>
만약 다음 아이템의 시작 값이 이전 아이템의 끝과 더 이상 겹치지 않게 된다면, 병합을 멈추고 다음과 같이 merged += i, 를 이용해 새로운 아이템으로 추가한다.<br>
전체 코드는 다음과 같다.
```python
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
    merged = []
    for i in sorted(intervals, key=lambda x: x[0]):
        if merged and i[0] <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], i[1])
        else:
            merged += i,
    return merged
```
이 방식의 시간 복잡도는 정렬에 소요되는 O(n log n) 정도다.<br>
나머지 리스트를 순회하는 작업은 선형 시간에 수행되므로, 상한은 마찬가지로 O(n log n)이다.
<br><br>

### 문법. 콤마(,) 연산자
이 풀이에는 중간에 특이한 문법이 등장한다.
```python
merged += i.,
```
이 같은 문법인데 연산자와 변수 뒤에 콤마(,)가 붙어 있다. 이렇게 하면 어떻게 동작할까? 먼저 기본적인 추가 연산을 해보자.
```python
>>> a = [1]
>>> b = [2,3]
>>> a += b
>>> a
[1, 2, 3]
```
단순히 +=를 했을 때는 요소를 이어붙인다. 행렬의 연결(Concatenate) 연산과 동일하다. <br>
이번에는 콤마를 넣어보자.
```python
>>> a = [1]
>>> b = [2,3]
>>> a += b,
>>> a
[1, [2, 3]]
```
이렇게 중접 리스트가 된다. 콤마는 중칩 리스트로 만들어주는 역할을 하며, 대괄호 []를 부여한 것과 동일한 역할을 한다.
```python
>>> a += [b]
>>> a
[1, [2, 3]]
```
콤마를 사용하든 대괄호를 사용하든 모두 동일한 역할을 하므로, 본인이 쓰기 펀한 방식으로 택해서 사용하면 된다.

<br><br>

## 문제 60 삽입 정렬 리스트
* 연결 리스트를 삽입 정렬로 정렬하라.

<br><br>

* **내가 짠 코드**<br>
```python

```

<br><br>

## 문제 60 삽입 정렬 리스트 풀이
### 풀이1. 삽입 정렬
입력값 4->2->1->3 연결 리스트이고 head는 루트 노드인 4를 가리킬 때의 삽입 정렬 과정을 그림 17-10으로 나타냈다.

<img src="https://user-images.githubusercontent.com/55045377/126888116-ae852ca9-eda0-4660-939c-d3a13431e1b1.png" width=70% height=70%>

먼저, 삽입 정렬은 정렬을 해야 할 대상과 정렬을 끝낸 대상, 두 그룹으로 나눠 진행한다.<br>
head는 정렬을 해야 할 대상이며, cur는 정렬을 끝낸 대상으로 정한다.

다음과 같이 정렬을 해야 할 대상 head를 반복한다.
```python
cur = parent = ListNode(None)
while head:
    while cur.next and cur.next.val < head.val:
        cur = cur.next
```
먼저, cur와 parent는 빈 노드로 정한다.<br>
cur에는 정렬을 끝낸 연결 리스트를 추가해줄 것이고, parent는 계속 그 위치에 두어 사실상 루트를 가리키게 한다.<br>
정렬을 끝낸 cur는 이미 정렬된 상태이므로, 정렬을 해야 할 대상 head와 비교하면서 더 작다면 계속 cur.next를 이용해 다음으로 이동한다.

이제 정렬이 필요한 위치, 즉 cur에 삽입될 위치를 찾았다면 다음과 같이 cur 연결 리스트에 추가한다.
```python
cur.next, head.next, head = head, cur.next, head.next

cur = parent
```
찾은 cur 위치 다음에 head가 들어가고 head.next에는 cur.next를 연결해 계속 이어지게 한다.<br>
그리고 다음번 head는 head.next로 차례를 이어받는다.<br>
이후에는 cur = parent를 통해 다시 처음으로 되돌아가며, 차례대로 다시 비교하게 된다. <br>
그림 17-10에서 cur의 마지막 노드는 2->3->4 형태가 되는데, 이는 입력값에서 유일하게 그 직전 값인 1보다 head의 값인 3이 더 크기 때문이다.<br>
따라서 cur = cur.next로 작거나 같아질 때까지 계속 전진한다.<br>
이후에 head는 None 이 되면서 비교가 끝나게 된다.
```python
def insertionSortList(self, head: ListNode) -> ListNode:
    cur = parent = ListNode(None)
    while head:
        while cur.next and cur.next.val < head.val:
            cur = cur.next
            
        cur.next, head.next, head = head, cur.next, head.next

        cur = parent
    return cur.next
```
매우 깔끔한 삽입 정렬 풀이법이다.그러나 수행시간은 **1936 밀리초**, 거의 2초가 걸렸다.<br>
이 정도면 타임아웃으로 처리되어도 할 말이 없다. <br>
분명 문제에서 제시한 대로 삽입 정렬을 정석대로 풀이했는데 왜 이런 결과가 나왔을까? 좀 더 최적화할 방
법은 없을까?

<br><br>

### 풀이2. 삽입 정렬의 비교 조건 개선





































