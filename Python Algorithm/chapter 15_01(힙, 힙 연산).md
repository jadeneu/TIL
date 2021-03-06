# 목차
* [chapter 15. 힙](#15장-힙)
  + [힙 연산](#힙-연산)
    - [삽입](#삽입)
    - [추출](#추출)
  + [참고. 이진 힙 vs 이진 탐색 트리(BST)](#참고.-이진-힙-vs-이진-탐색-트리(bst))
<br><br><br>

# 15장 힙
> 힙은 힙의 특성(최소 힙(Min Heap)에서는 부모가 향상 자식보다 작거나 같다)을 만족하는 거의 완전한 트리(Almost Complete Tree)인 특수한 트리 기반의 자료구조다.

**힙(Heap)** 은 그래프나 트리와는 전혀 관계 없어 보이는 독특한 이름과 달리, 트리 기반의 자료구조다.<br>
앞서 우선순위 큐를 사용할 때 매번 활용했던 heapq 모듈이 바로 힙으로 구현되어 있으며, 그중에서도 **파이썬에는 최소 힙만 구현되어 있다.**<br>
최소 힙은 부모가 항상 자식보다 작기 때문에 루트가 결국 가장 작은 값을 갖게 되며, 우선순위 큐에서 가장 작은 값을 추출하는 것은 매번 힙의 루트를 가져오는 형태로 구현된다.<br>
기반 구현을 살펴보면, **우선순위 큐 ADT(추상 자료형)는 주로 힙으로 구현하고, 힙은 주로 배열로 구현한다.**<br>
따라서 우선순위 큐는 결국은 배열로 구현하는 셈이 된다.

여기서 오해하기 쉬운 특징 중 하나는 힙은 정렬된 구조가 아니라는 점이다.<br>
**최소 힙의 경우 부모 노드가 항상 작다는 조건만 만족할 뿐, 서로 정렬되어 있지 않다.**<br>
예를 들어 오른쪽의 자식 노드가 레벨 차이에도 불구하고, 왼쪽 노드보다 더 작은 경우도 얼마든지 있을 수 있다.<br>
부모, 자식 간의 관계만 정의할 뿐, 좌우에 대한 관계는 정의하지 않기 때문이다.

<img src="https://user-images.githubusercontent.com/55045377/123357813-37f42780-d5a5-11eb-9e92-76fd8caf9660.png" width=50% height=50%>

그림 15-1에서도 부모는 항상 자식보다 작을 뿐, 좌우의 정렬 관계는 제각각이다.<br>
여기서는 대부분 왼쪽이 오른쪽보다 작지만, 14의 자식 노드인 33과 17은 왼쪽이 오른쪽보다 더 크다.<br>
자식이 둘인 힙은 특별히 이진 힙(Binary Heap)이라 하며, 대부분은 이진 힙이 널리 사용된다.<br>
트리 중에서 이진 트리가 널리 사용되는 이유와 비슷하다.

힙이라는 자료구조 자체는 J.W.J. 윌리엄스(Williams)라는 영국의 컴퓨터과학자가 1964년 힙 정렬 알고리즘을 고안하면서 설계했다.<br>
사실상 힙이라는 자료구조는 힙 정렬의 부산물인 셈이다.<br>
**힙은 완전 이진 트리이기 때문에 배열에 순서대로 표현하기에 적합하다.**<br>
앞서 14장의 47번 ‘이진 트리 직렬화 & 역직렬화’ 문제에서 살펴보기도 했던 그림 15-2는 이진 힙의 배열 표현이며, 이처럼 루트부터 차례대로 나열하면 1, 2, 4, ... 순서대로 각 레벨의 노드가 2배씩 증가하는 형태로 배열에 나열할 수 있다

<img src="https://user-images.githubusercontent.com/55045377/123358290-2f502100-d5a6-11eb-84aa-aa2717f42ee1.png" width=70% height=70%>

그림 15-2의 2)처럼 완전 이진 트리 형태인 이진 힙은 배열에 빈틈없이 배치가 가능하며, 대개 트리의 배열 표현의 경우 계산을 편하게 하기 위해 인덱스는 1부터 사용한다.

힙은 항상 균형을 유지하는 특징 때문에 다양한 분야에 널리 활용된다. 대표적으로 우선순위 큐뿐만 아니라 이를 이용한 다익스트라 알고리즘에도 활용된다.<br>
힙 덕분에 다익스트라 알고리즘의 시간 복잡도는 O(V^2) 에서 O(E log V)로 줄어들 수 있었다.<br>
이외에도 원래의 용도인 힙 정렬과 최소 신장 트리(Minimum Spanning Tree)를 구현하는 프림 알고리즘(Prim's Algorithm) 등에도 활용되며, 중앙값의 근사값(Approximation)을 빠르게 구하는 데도 활용할 수 있다.<br>
부모, 자식 관계가 정의되어 있는 완전 이진 트리이기 때문에 적절히 중간 레벨의 노드를 추출하면 중앙값에 가까운 값을 정확하지는 않지만 근사치로 빠르게 추출할수있기 때문이다.
<br><br>

## 힙 연산
그렇다면 실제로 이진 힙을 한번 구현해보자.<br>
파이썬의 heapq 모듈에서 지원하는 최소 힙 연산을 여기서는 파이썬의 리스트만으로 동일하게 구현해보자.<br>
먼저 이진 힙을 구현하기 위한 클래스 정의부터 진행해보자.
```python
class BinaryHeap(object):
    def __init__(self):
        self.items = [None]
        
    def __len__(self):
        return len(self.items) - 1
```
클래스의 뼈대를 만들고, __ len __ () 메소드를 정의했다.<br>
__ len __ ()처럼 밑줄( _ ) 2개로 둘러싸인 함수는 파이썬의 매직 메소드로 여러 가지 내부(Built-In) 기능이 동작되는 데 사용된다.<br>
예를 들어 len(a)를 하게 되면, 내부적으로 a. __ len __ ()을 호출하여 이 결과를 리턴하게 된다.<br>
여기서도 len()으로 호출하면 마지막 요소의 인덱스를 가져오기 위해 실제 길이보다 하나 더 작은 값을 가져오도록, __ len __ () 함수에서 실제 len()의 결과와는 조금 다른 형태로 구현해뒀다. 

아울러 0번 인덱스는 사용하지 않기 위해 None을 미리 설정해뒀다.<br>
0번 인덱스를 사용하지 않는 모습은 그림 15-2에서도 확인할 수 있는데, 대개 트리의 배열 표현의 경우 인덱스 계산을 편하게 하기 위해 인덱스는 1부터 사용한다.<br>
특히 이진 힙에서는 항상 1번 인덱스부터 사용한다.
<br><br>

### 삽입
힙에 요소를 삽입하기 위해서는 업힙(Up-Heap) 연산을 수행해야 한다.<br>
일반적으로 업힙 연산은 percolate_up()이라는 함수로 정의한다.<br>
힙에 요소를 삽입하는 과정을 한번 살펴보자.

1. 요소를 가장 하위 레벨의 최대한 왼쪽으로 삽입한다(배열로 표현할 경우 가장 마지막에 삽입한다).
2. 부모 값과 비교해 값이 더 작은 경우 위치를 변경한다.
3. 계속해서 부모 값과 비교해 위치를 변경한다(가장 작은 값일 경우 루트까지 올라감).

이제 이 과정을 나타낸 그림 15-3을 살펴보자.

<img src="https://user-images.githubusercontent.com/55045377/123366369-0e41fd00-d5b3-11eb-9bec-3347a4cb84cb.png" width=80% height=80%>

이 그림에는 신규 아이템 7이 마지막에 삽입되어, 부모 노드의 값과 비교하면서 점점 스왑되는 과정이 잘 나타나 있다.<br>
두 번째 스왑 이후에야 부모 노드인 5보다 더 크기 때문에, 더 이상 스왑되지 않고 멈추는 걸 확인할 수 있다.<br>
이 과정을 코드로 구현해보면 다음과 같다.
```python
def _percolate_up(self):
    i = len(self)
    parent = i // 2
    while parent >= 0:
        if self.items[i] < self.items[parent]:
            self.items[parent], self.items[i] = \
                self.items[i], self.items[parent]
        i = parent
        parent = i // 2
        
def insert(self, k):
    self.items.append(k)
    self._percolate_up()
```
삽입 자체는 insert() 함수를 호출해 실행된다. <br>
코드에서 insert() 함수의 self.items.append()는 1번 과정이고, percolate_up() 함수는 2번, 3번 과정이다.<br>
이 과정을 통해 그림 15-3에서 7은 5보다는 크기 때문에 루트까지는 될 수 없고, 계속 스왑되다가 두 번째 레벨에 안착하게 된다.<br>
시간 복잡도는 O(log n) 이다.<br>
_ percolate_up() 함수를 보면 parent를 i // 2로 약 절반씩 줄여나가는 형태이므로 로그만큼 연산을 수행하는 것을 알 수 있다.<br>
percolate_up() 함수명 앞에는 내부 함수라는 의미로 PEP 8 기준과 관례에 따라 밑줄( _ )을 부여해서 _ percolate_up( )으로 정했다.
<br><br>

### 추출
추출 자체는 매우 간단하다.<br>
루트를 추출하면 된다. 그렇다면 시간 복잡도는 O(1)이라고 생각할 수 있겠지만, 추출 이후에 다시 협의 특성을 유지하는 작업이 필요하기 때문에 시간 복잡도는 O(log n)이다.

<img src="https://user-images.githubusercontent.com/55045377/123505608-18d7c180-d69b-11eb-9928-362a7925b19d.png" width=75% height=75%>

그림 15-4에는 이진 힙에서 요소의 추출 과정을 설명한다.<br>
추출 이후에 비어 있는 루트에는 가장 마지막 요소가 올라가게 되고, 이번에는 반대로 자식 노드와 값을 비교해서 자식보다 크다면 내려가는 다운힙(Down-Heap) 연산이 수행된다.<br>
일반적으로 힙 추출에 많이쓰이는 percolate_down()이라는 이름의 함수로 구현해보자.

이 과정은 리스트 15-1과 같은 위키피디아의 수도코드를 참고하여 알고리즘을 동일하게 파이썬 코드로 구현해보자.

* **리스트 15-1** 이진 힙의 요소 추출 과정 수도코드
```
Max-Heapify (A, i):
    left ← 2xi
    right ← 2xi + 1
    largest ← i
    
    if left <= heap_length[A] and A[left] > A[largest] then:
        largest ← left
        
    if right <= heap_length[A] and A[right] > A[largest] then:
        largest ← right
        
    if largest != i then:
        swap A[i] and A[largest]
        Max-Heapify(A, largest)
```
수도코드가 깔끔하게 잘 정리되어 있다.<br>
여기서는 수도코드 또한 인덱스가 1 이상일 때만 동작하도록 구현되어 있다.<br>
앞서 그림 15-2에서도 1번 인덱스부터 시작한다고 설명한 바 있고, 이후 코드에서도 0번 인덱스에는 None을 할당하고 1번부터 사용하게 했다.<br>
로버트 세지윅의 "알고리즘 개정 4판" 2장, ‘정렬’에 나오는 이진 힙의 정의에서도, 첫 번째 항목은 사용하지 않는다고 설명한다.

첫 번째 항목(0번 인덱스)은 항상 비워두고 1번 인덱스부터 사용하는 이유는 인덱스 계산을 편하게 하기 위함이다. <br>
특히 1번 인덱스부터 사용하게 되면 인덱스 계산이 깔끔하게 떨어진다.<br>
이런 경우는 머신러닝 분야에서도 종종 볼 수 있다. 경사하강법(Gradient Descent)에서 기울기를 계산할 때 미분이 깔끔하게 떨어지도록 일부러 상수항을 부여하기도 한다.<br>
대부분은 컴퓨터가 직접 계산해주기 때문에 개발자가 내부 계산을 직접 들여다 볼 필요는 없지만, 이처럼 내부를 들여다 보게 되면 깔끔한 계산을 위해 별도 처리를 해주는 경우를 종종 볼 수 있다.<br>
대개 논문에서는 이처럼 별도 처리를 통해 계산이 깔끔하게 되도록 수식을 기입해두곤 한다.

그렇다면 이진 힙에서 인덱스를 1부터 시작하는 경우에는 어떻게 계산이 될까?<br>
다음 리스트 15-2와 같이 부모, 자식 노드의 인덱스를 계산할 수 있다.

* **리스트 15-2** 이진 힙의 인덱스 위치 계산 수도코드
```
Parent(i)
    return ceil((i - 1) / 2)
Left(i)
    return 2i
Right(i)
    return 2i + 1
```
이 수도코드에서 부모 노드를 구하는 코드를 보면 2를 나눈 올림값으로 정리되며, 자식 노드도 왼쪽, 오른쪽 각각이 i * 2, i * 2 + 1로 깔끔하게 계산이 처리된다.<br>
파이썬 코드에서도 마찬가지로 깔끔하게 인덱스를 계산할 수 있다. <br>
이처럼 깔끔한 계산을 위해 1번 인덱스부터 사용하며, 0번 인덱스는 비워두는 것이다.<br>
참고로 여기서 수도코드는 최대 힙이다.<br>
우리가 구현하려는 것은 최소 힙이므로 이 부분은 수도코드의 알고리즘을 적절히 수정해 다음과 같이 구현할 수 있다.

```python
def _percolate_down(self, idx):
    left = idx * 2
    right = idx * 2 + 1
    smallest = idx
    
    if left <= len(self) and self.items[left] < self.items[smallest]:
        smallest = left
        
    if right <= len(self) and self.items[right] < self.items[smallest]:
        smallest = right
        
    if smallest != idx:
        self.items[idx], self.items[smallest] = \
            self.items[smallest], self.items[idx]
        self._percolate_down(smallest)
        
    def extract(self):
        extracted = self.items[1]
        self.items[1] = self.items[len(self)]
        self.items.pop()
        self._percolate_down(1)
        return extracted
```
마찬가지로 추출 자체는 extract() 함수를 호출해 실행된다.<br>
이후 루트 값이 추출되고, percolate_down()이 실행되는데 여기서는 최소 힙이므로 변수명도 smallest로 변경해봤다.

각각 왼쪽과 오른쪽 자식을 비교하고 더 작다면 해당 인덱스로 교체한다.<br>
인덱스가 교체되었다면 서로 값을 스왑하고 다시 재귀 호출한다. <br>
값이 스왑되지 않을 때까지 계속 자식 노드로 내려가면서 스왑될 것이다.<br>
즉 힙 특성이 유지될 때까지 계속 반복해서 재귀 호출된다.

이렇게 위키피디아의 이진 힙 추출 알고리즘까지 동일하게 구현해봤다.<br>
이제 전체 코드는 다음과 같다.
```python
class BinaryHeap(object):
    def __init__(self):
        self.items = [None]
        
    def __len__(self):
        return len(self.items) - 1
        
    # 삽입 시 실행, 반복 구조 구현
    def _percolate_up(self):
        i = len(self)
        parent = i // 2
        while parent > 0:
            if self.items[i] < self.items[parent]:
                self.items[parent], self.items[i] = \
                    self.items[i], self.items[parent]
            i = parent
            parent = i // 2
            
    def insert(self, k):
        self.items.append(k)
        self._percolate_up()
    
    # 추출시 실행, 재귀 구조 구현
    def _percolate_down(self, idx):
        left = idx * 2
        right = idx * 2 + 1
        smallest = idx

        if left <= len(self) and self.items[left] < self.items[smallest]:
            smallest = left

        if right <= len(self) and self.items[right] < self.items[smallest]:
            smallest = right

        if smallest != idx:
            self.items[idx], self.items[smallest] = \
                self.items[smallest], self.items[idx]
            self._percolate_down(smallest)

        def extract(self):
            extracted = self.items[1]
            self.items[1] = self.items[len(self)]
            self.items.pop()
            self._percolate_down(1)
            return extracted
```
기존 파이썬 heap 모듈의 heapq.heappush()는 insert()에, heapq.heappop()은 extract()에 대응된다.<br>
이처럼 이진 힙의 연산에 각각 대응되는 함수를 모두 구현해봤다.
<br><br>

## 참고. 이진 힙 vs 이진 탐색 트리(BST)
그렇다면 지금까지 살펴본 이진 힙과 이진 탐색 트리(BST)의 차이점은 무엇일까?<br>
얼핏 보면 비슷해 보이기도 하는데, 어떤 차이점이 있으며 각각 어떤 경우에 활용되는지 혼동스러울 수 있을 것 같다.

가장 직관적인 차이점을 들자면, **힙은 상/하 관계를 보장**하며, 특히 최소 힙에서는 부모가 항상 자식보다 작다.<br>
반면 **BST는 좌/우 관계를 보장**한다. BST에서 부모는 왼쪽 자식보다는 크며, 오른쪽 자식보다는 작거나 같다.<br>
이 같은 특징으로 인해 BST는 탐색과 삽입 모두 O(log n) 에 가능하며, 모든 값이 정렬되어야 할 때 사용한다.<br>
반면 가장 큰 값을 추출하거나(최대 힙) 가장 작은 값을 추출하려면(최소 힙) **이진 힙**을 사용해야 한다.<br>
이진 힙은 이 작업이 O(1)에 가능하다. <br>
우선순위와 연관되어 있으며 따라서 이진 힙은 우선순위 큐에 활용된다.
<br><br>























