# 목차


# 11장 해시 테이블
## 리트코드
### 문제 28 해시맵 디자인
> 291p

* **내가 짠 코드**<br>
해시가 어떻게 일어나는지 파악할 목적으로 풀이를 보고 이해했다.
<br><br>

### 문제 28 해시맵 디자인 풀이
#### 풀이1. 개별 체이닝 방식을 이용한 해시 테이블 구현
앞서 파이썬의 딕셔너리는 오픈 어드레싱을 사용한다고 했지만 여기서는 개별 체이닝 방식으로 구현해본다.

우리가 구현할 MyHashMap 클래스의 전체 메소드는 다음과 같다.
```python
class MyHashMap:
    def __init__(self):
    
    def put(self, key: int, value: int) -> None:
    
    def get(self, key: int) -> int:
    def remove(self, key: int) -> None:
```

여기서는 초기화 __ init__(), 삽입 put(), 조회 get(), 삭제 remove()의 총 4가지 기능이 있으며, 편의상 키와 값은 모두 int로 한다.<br>
이외에도 키, 값을 보관하고 연결 리스트로 처리할 별도의 객체를 구현해야 하는데, 여기서는 다음과 같이 ListNode라는 이름의 클래스를 정의한다.
```python
class ListNode:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.next = None
```
<br>

이제, 가장 먼저 초기화부터 구현해보자.
```python
def __init__(self):
    self.size = 1000
    self.table = collections.defaultdict(ListNode)
```
기본 사이즈는 1,000개 정도로 적당히 설정하고, 각 ListNode를 담게 될 기본 자료형을 선언한다.<br>
편리하게 구현하기 위해 존재하지 않는 키를 조회할 경우 자동으로 디폴트를 생성해주는 collections.defaultdict를 사용했다.

이제 삽입 메소드 put()을 구현해보자.
```python
def put(self, key: int, value: int) -> None:
    index = key % self.size
    ...
```
이 문제는 편의상 모든 키를 정수형으로 지정했다.<br>
따라서 size의 개수만큼 모듈로(Modulo) 연산을 한 나머지를 해시값으로 정하는 매우 단순한 형태로 처리한다. 앞서 해시 함수를 살펴볼 때도 모듈로 연산을 이용한 나눗셈 방식을 살펴본 바 있으며, **이처럼 모듈로 연산을 통한 해싱은 해시 테이블의 가장 기본적인 해싱 방식이기도 하다.**<br>
이제 해싱한 결과인 index는 해시 테이블의 인덱스가 될 것이다.
<br><br>

```python
if self.table[index].value is None:
    self.table[index] = ListNode(key, value)
    return
```
이처럼 만약 해당 인덱스에 아무것도 없다면 키, 값을 삽입하고 바로 종료한다.<br>
여기서, self.table[index] is None으로 비교하지 않고 self.table[index].value is None으로 굳이 value의 존재 유무를 비교했는데 그렇게 하는 이유는 MyHashMap의 초기화 메소드 __ init__()에서 선언한 self.table이 collections.defaultdict(ListNode)이기 때문이다.<br>
defaultdict는 존재하지 않는 인덱스로 조회를 시도할 경우 에러를 발생하지 않고 그 자리에서 바로 디폴트 객체를 생성한다.<br>
여기서는 defaultdict(ListNode)로 선언했기 때문에 존재하지 않는 인덱스를 조회할 경우 바로 빈 ListNode를 생성할 것이다.

이제 ListNode() 클래스의 생성 함수 __ init__()을 살펴보면, key=None, value=None으로 초깃값을 정했다. 때문에 각각이 None인 ListNode가 조회 즉시 생성될 것이다.<br>
이 부분은 defaultdict의 특징이며 매우 편리하게 사용할 수 있는 반면, 이처럼 자동으로 처리해주는 부분들이 있으므로 자칫 잘못하면 버그를 유발할 수 있다.<br>
이 경우에도 만약 self.table[index] is None으로 처리했다면 절대로 True가 되지 않는 버그가 생길 것이다.<br>
따라서 이 같은 특징에 대해 충분히 숙지할 필요가 있다

이제 다음 코드를 살펴보자.
```python
p = self.table[index]
while p:
    if p.key == key:
        p.value = value
        return
    if p.next is None:
        break
    p = p.next
```
여기서부터는 해당 인덱스에 노드가 존재하는 경우다.<br>
즉 해시 충돌(Hash Collision)이 발생한 경우인데, 앞서 언급한 것처럼 개별 체이닝 방식으로 충돌을 해결할 것이다.<br>
즉 연결 리스트로 이어나갈 것이다. p는 인덱스의 첫 번째 값이며 여기서부터 p.next를 계속 탐색한다.<br>
종료 조건은 2가지 경우인데, 첫 번째는 이미 키가 존재할 경우 값을 업데이트하고 빠져나가는 경우고, 두 번째는 p.next is None이라면 아무것도 하지 않고 루프를 빠져나가는 경우다.<br>
만약 후자를 처리하지 않는다면 p = p.next로 인해 p는 None이 되기 때문에 그다음 구문인 다음 코드에서 에러가 발생할 것이다.
```python
p.next = ListNode(key, value)
```
에러 없이 정상적으로 진행되면 이 코드에서 p.next에 새 노드가 생성되면서 연결된다.<br>
기존에 존재하지 않았던 키라면 맨 마지막에 새로운 노드가 연결될 것이다. **여기까지가 개별 체이닝 방식의 해시 테이블 삽입 원리다.**<br>
군더더기를 제외하고 필수적인 기능만 구현했기 때문에 어렵지 않게 이해할 수 있을 것이다.<br>
아마 이 코드를 통해 이렇게 직접 구현해보면 해시 테이블의 삽입 원리를 이해하는 데도 많은 도움이 될 것이다.

이제 조회 메소드 get()을 구현해보자.
```python
def get(self, key: int) -> int:
    index = key % self.size
    if self.table[index].value is None:
        return -1
    ...
```
마찬가지로 모듈로 연산으로 인덱스를 결정하고, 해당 인덱스에 아무것도 없다면 -1을 리턴한다.<br>
이 경우는 해당하는 노드는 물론, 아직 어떠한 키도 이 값으로 해싱되지 않은 경우다.

다음은 노드가 존재할 때의 처리다.
```python
p = self.table[index]
while p:
    if p.key == key:
        return p.value
    p = p.next
return -1
```
여기서부터는 이 해싱 결과에 하나 이상의 노드가 존재한다는 얘기이므로, p.next로 탐색하면서 일치하는 키를 찾는다.<br>
찾게 되면 값을 리턴하고, 찾지 못한다면 루프를 빠져나오면서 마찬가지로 -1을 리턴한다.

이제 다음으로 삭제 메소드 remove()를 살펴보자.
```python
def remove(self, key: int) -> None:
    index = key % self.size
    if self.table[index].value is None:
        return
    ...
```
인덱스를 구한 다음 아무것도 없다면, 잘못된 키를 삭제 시도한 경우이므로 그냥 리턴한다.<br>
값이 있을 때는 2가지 케이스로 나눠서 처리한다.
```python
# 인덱스의 첫 번째 노드일 때 삭제 처리
p = self.table[index]
if p.key == key:
    self.table[index] = ListNode() if p.next is None else p.next
    return
```
먼저 이처럼 인덱스의 첫 번째 노드일 때, p.next is None이라면 유일한 노드를 삭제하는 경우이므로 원래는 모두 없애야 한다.<br>
그러나 여기서는 ListNode()로 빈 노드를 할당하게 했다. 애당초 self.table은 defaultdict(ListNode)이기 때문에 매번 빈 노드를 생성하기 때문이다.<br>
만약 여기서 self.table[index] = None을 할당한다면 앞서 추가, 조회 함수에서 self.table[index].value is None으로 비교를 시도할 때 에러가 발생할 것이다.
```python
# 연결 리스트 노드 삭제
prev = p
while p:
    if p.key == key:
        prev.next = p.next
        return
    prev, p = p, p.next
```
다음으로 연결 리스트인 노드를 삭제할 때는 prev는 이전 노드, p는 현재 노드로, 계속 p.next로 탐색하다가 일치하는 노드를 찾게 되면, 이전 노드의 다음을 현재 노드의 다음으로 연결한다. 즉 현재 노드를 아무런 연결 고리가 없도록 끊어버린다.<br>
가만히 살펴보면 약간의 버그 가능성이 엿보인다. 만약 루프에 들어서자마자 일치하는 키를 발견하게되면 prev.next = p.next가 동일한 부분이 될 것이다. 그러나 이런 경우는 발생하지 않는다. 왜냐면 처음부터 일치하는 경우는 이미 '# 인덱스의 첫 번째 노드일 때 삭제 처리'에서 처리가 되기 때문이다.<br>
따라서 여기서는 반드시 한 번 이상 반복한 후에야 일치하는 경우가 생길 것이다.<br>
여기서는 그대로 두지만 만약 이 부분이 못내 거슬린다면 여러분이 다른 형태로 직접 개선을 시도해봐도 좋다.

이제 전체 코드는 다음과 같다.
```python
class MyHashMap:
    # 초기화
    def __init__(self):
        self.size = 1000
        self.table = collections.defaultdict(ListNode)
        
    # 삽입
    def put(self, key: int, value: int) -> None:
        index = key % self.size
        # 인덱스에 노드가 없다면 삽입 후 종료
        if self.table[index].value is None:
            self.table[index] = ListNode(key, value)
            return
            
        
        # 인덱스에 노드가 존재하는 경우 연결 리스트 처리
        p = self.table[index]
        while p:
            if p.key == key:
                p.value = value
                return
            if p.next is None:
                break
            p = p.next
        p.next = ListNode(key, value)
        
    # 조회
    def get(self, key: int) -> int:
        index = key % self.size
        if self.table[index].value is None:
            return -1
            
        # 노드가 존재할 때 일치하는 키 탐색
        p = self.table[index]
        while p:
            if p.key == key:
                return p.value
            p = p.next
        return -1
        
    # 삭제
    def remove(self, key: int) -> None:
        index = key % self.size
        if self.table[index].value is None:
            return
            
        # 인덱스의 첫 번재 노드일 때 삭제 처리
        p = self.table[index]
        if p.key == key:
            self.table[index] = ListNode() if p.next is None else p.next
            return
            
        # 연결 리스트 노드 삭제
        prev = p
        while p:
            if p.key == key:
                prev.next = p.next
                return
            prev, p = p, p.next
```
전체 코드가 다소 길어 보이지만 군더더기 없이 개별 체이닝 방식으로 해시 테이블을 구현해봤다.
<br><br>






















