# 목차
* [chapter 12. 그래프](#12장-그래프)
* [리트코드](#리트코드)
  + [문제 34 순열](#문제-34-순열)
  + [문제 34 순열 풀이](#문제-34-순열-풀이)
    - [풀이1. DFS를 활용한 순열 생성](#풀이1-dfs를-활용한-순열-생성)
    - [풀이2. itertools 모듈 사용](#풀이2-itertools-모듈-사용)
    - [문법. 객체 복사](#문법-객체-복사)
  + [문제 35 조합](#문제-35-조합)
  + [문제 35 조합 풀이](#문제-35-조합-풀이)
    - [풀이1. DFS로 k개 조합 생성](#풀이1-dfs로-k개-조합-생성)
    - [풀이2. itertools 모듈 사용](#풀이2-itertools-모듈-사용)
  + [문제 36 조합의 합](#문제-36-조합의-합)
  + [문제 36 조합의 합 풀이](#문제-36-조합의-합-풀이)
    - [풀이1. DFS로 중복 조합 그래프 탐색](#풀이1-dfs로-중복-조합-그래프-탐색)
<br><br><br>

# 12장 그래프
## 리트코드
### 문제 34 순열
> 341p

* **내가 짠 코드**<br>
  순열 백트래킹 구글링해서 품
  ```python
  def permutation(digit):
    def dfs(chosen,visited):

      if len(chosen) == len(digit):
        print(chosen)
        return

      for i in range(len(digit)):
        if not visited[i]:
          chosen.append(digit[i])
          visited[i] = 1  # 방문 표시
          dfs(chosen,visited)
          chosen.pop()
          visited[i] = 0


    visited = [0 for _ in range(len(digit))]
    dfs([],visited)

  digit = [1,2,3]
  permutation(digit)
  ```
  (이중 리스트는 구현 못한 상태)
<br><br>

  풀이에서 [:] 부분 보고 해결함 -> 이중 리스트 구현 완성
  ```python
  def permutation(digit):
      visited = [0 for _ in range(len(digit))]
      result = []

      def dfs(chosen,visited):

          if len(chosen) == len(digit):
            result.append(chosen[:])
            return

          for i in range(len(digit)):
            if not visited[i]:
              chosen.append(digit[i])
              visited[i] = 1  # 방문 표시
              dfs(chosen,visited)
              chosen.pop()
              visited[i] = 0

      dfs([],visited)
      return result

  digit = [1,2,3]
  print(permutation(digit))
  ```
<br><br>

### 문제 34 순열 풀이
#### 풀이1. DFS를 활용한 순열 생성
예제값 [1,2,3]의 순열의 수 수식은 3!/(3-3)!이 되고, 분모는 (3-3)!=1 이므로 분자의 팩토리얼만 계산하면 순열의 수는 3! = 3 x 2 x 1 = 6 이 된다.<br>
이처럼 개수를 구하는 건 어렵지 않다.<br>
그러나 이 문제는 모든 결과를 생성해내야 하는데, 경우의 수만 계산하는 것에 비해서는 다소 까다로운 편이다.<br>
순열이란 결국 모든 가능한 경우를 그래프 형태로 나열한 결과라고 할 수 있기 때문에 그래프로 표현해보면 그림 12-12(342p)와 같다.

> 그림 12-12

이 그림에서 리프(Leaf) 노드, 즉 A33의 모든 노드가 순열의 최종 결과다.<br>
가만히 살펴보면 레벨이 증가할수록 자식 노드의 개수는 점점 작아진다.<br>
처음에는 A03의 자식 노드는 3개였다가 A13의 자식 노드는 2개, A23의 자식 노드는 1개 순으로, 이는 순열의 수식이기도 한 3 x 2 x 1 형태이기도 하다.<br>
만약 입력값이 5개라면 수식은 5 x 4 x 3 x 2 x 1 이고 이때 자식 노드의 개수도 마찬가지로 5개부터 4개, 3개, ... , 1개 순이 될 것이다.

이제 dfs() 탐색 함수를 만들어보자.
```python
def dfs(elements):
    # 리프 노드일 때 결과 추가
    if len(elements) == 0:
        results.append(prev_elements[:])
        
    # 순열 생성 재귀 호출
    for e in elements:
        next_elements = elements[:]
        next_elements.remove(e)
        
        prev_elements.append(e)
        dfs(next_elements)
        prev_elements.pop()
```
이전 값을 하나씩 덧붙여 계속 재귀 호출을 진행하다 리프 노드에 도달한 경우, 즉 len(elements) == 0일 때 결과를 하나씩 담는다.<br>
이때 중요한 부분은 결과를 추가할 때 prev_elements[:]로 처리해야 한다는 점이다.<br>
파이썬은 모든 객체를 참조하는 형태로 처리되므로 만약 results.append(prev_elements)를 하게 되면 결과 값이 추가되는 게 아닌 prev_elements에 대한 참조가 추가되며, 이 경우 참조된 값이 변경될 경우 같이 바뀌게 된다.<br>
**따라서 반드시 값을 복사하는 형태로 참조 관계를 갖지 않도록 처리해야 한다.**<br>
[:]로 처리하는 방법 외에도 copy()를 하거나, 복잡한 리스트는 deepcopy()로 처리하는 방법도 있다.<br>
이 부분만 주의해 순열 생성을 계속 재귀 호출하면 풀이를 완성할 수 있다.

전체 코드는 다음과 같다.
```python
def permute(self, nums: List[int]) -> List[List[int]]:
    results = []
    prev_elements = []
    
    def dfs(elements):
        # 리프 노드일 때 결과 추가
        if len(elements) == 0:
            results.append(prev_elements[:])

        # 순열 생성 재귀 호출
        for e in elements:
            next_elements = elements[:]
            next_elements.remove(e)

            prev_elements.append(e)
            dfs(next_elements)
            prev_elements.pop()
            
    dfs(nums)
    return results
```
<br><br>

* **참고 | 원소 삭제 후(remove) 남은 원소들 append하는 다른 방식**
  ```python
  m = lst[i]
  remain = lst[:i] + lst[i+1:]
  ```

<br><br>

#### 풀이2. itertools 모듈 사용
파이썬에는 itertools라는 모듈이 있다.<br>
itertools 모듈은 반복자 생성에 최적화된 효율적인 기능들을 제공하므로, 실무에서는 알고리즘으로 직접 구현하기보다는 가능하다면 itertools 모듈을 사용하는 편이 낫다.<br>
이미 잘 구현된 라이브러리라 직접 구현함에 따른 버그 발생 가능성이 낮고, 무엇보다 효율적으로 설계된 C 라이브러리라 속도에도 이점이 있다.

항상 그렇듯 잘 만들어진 라이브러리를 사용하면 풀이는 매우 간단해진다.<br>
이제 바로 전체 코드를 살펴보면 다음과 같다.
```python
def permute(self, nums: List[int]) -> List[List[int]]:
    return list(itertools.permutations(nums))
```
리트코드에서 36밀리초에 실행되는데, 이 정도면 상위 5% 이내에 드는 매우 빠른 속도다.<br>
파이썬 코드로 아무리 효율적인 알고리즘을 작성한다 해도 이보다 빨리 실행되기는 힘들 것이다.<br>
딱 한 가지 걸리는 부분은. 이 함수의 결과가 리스트 내 튜플이라는 점이다.<br>
permutations() 함수가 튜플 모음을 반환하기 때문이다.<br>
리트코드 문제에서는 리스트를 반환하도록 요구하기 때문에 실제로는 전체 코드에서 다음과 같이 변환해서 처리해야 한다.
```python
def permute(self, nums: List[int]) -> List[List[int]]:
    return list(map(list, itertools.permutations(nums)))
```
이렇게 하면 문제에서 요구하는 리스트 내 리스트로 정확히 리턴할 수 있다.<br>
그러나 리트코드에서는 튜플 또한 정상적으로 정답으로 처리되므로 굳이 이렇게 처리하지 않아도 특별히 문제는 없다.
<br><br>

#### 문법. 객체 복사
파이썬의 중요한 특징 중 하나는 모든 것이 객체라는 점이다.<br>
심지어 숫자, 문자까지도 모두 객체다.<br>
숫자, 문자가 리스트, 딕셔너리 같은 객체와의 차이점이라면 불변 객체라는 차이뿐이다.<br>
그러다 보니 **별도로 값을 복사하지 않는 한, 변수에 값을 할당하는 모든 행위는 값 객체에 대한 참조가 된다.**<br>
이 말은 참조가 가리키는 원래의 값을 변경하면 모든 참조, 즉 모든 변수의 값 또한 함께 변경된다는 말이기도 하다.<br>
이 같은 특징은 C에서 포인터를 자주 사용해온 오래된 개발자거나 C++에서 참조를 경험해본 개발자에게는 익숙하지만 대부분의 사람들에게는 생소할 것이고, 이 같은 파이썬의 특징은 충분히 숙지할 필요가 있다.<br>
(같은 얘기를 8장, '연결 리스트' 211페이지에서 '다중 할당'을 성명하며 똑같이 언급한 바 있다)

그렇다면 참조가 되지 않도록 값 자체를 복사하려면 어떻게 해야 할까?<br>
앞서 문제 풀이1에서도 살펴봤지만 가장 간단한 방법은 [:]로 처리하는 방법이다.
```python
>>> a = [1,2,3]
>>> b = a
>>> c = a[:]
>>> id(a), id(b), id(c)
(4362781552, 4362781552, 4361580048)
```
[:]로 처리한 변수 c는 다른 ID를 갖는 것을 확인할 수 있다.<br>
참조로 처리된 변수 b는 a와 동일한 ID를 갖지만 변수 c는 값 자체가 복사되어 새로운 객체가 되었다.<br>
이외에도 좀 더 직관적으로 처리하는 방법은 다음과 같이 명시적으로 copy() 메소드를 사용하는 방법이다.
```python
>>> d = a.copy()
>>> id(a), id(b), id(c), id(d)
(4362781552, 4362781552, 4361580048, 4363974768)
```
이 경우 변수 d 또한 다른 ID를 갖는다.<br>
값이 복사되어 새로운 객체가 되었음을 확인할 수 있다.<br>
이처럼 단순한 리스트는 [:] 또는 copy()로도 충분하지만, **복잡한 리스트의 경우 다음과 같이 copy.deepcopy()로 처리해야 한다.**<br>
그렇게 하면 복잡하게 중첩된 리스트도 문제 없이 복사된다.
```python
>>> import copy
>>> a = [1, 2, [3, 5], 4]
>>> b = copy.deepcopy(a)
>>> id(a), id(b), b
(4480589512, 4481610824, [1, 2, [3, 5], 4])
```
이처럼 리스트 내에 리스트를 갖는 중첩된 리스트도 deepcopy()를 사용하면 문제 없이 값을 복사할 수 있으며, 다른 ID를 갖는다.
<br><br>

### 문제 35 조합
> 346p

* **내가 짠 코드**<br>
예전에 짠 코드 보고 품
```python
def combination(n, k):
  result = []

  def dfs(chosen,cnt):
    if len(chosen) == k:
      result.append(chosen[:])
      return

    for i in range(cnt,n+1):
      chosen.append(i)
      dfs(chosen, i+1)
      chosen.pop()

  dfs([],1)
  return result

n = 4
k = 2
print(combination(n,k))
```
<br>

* **NOTE: 조합의 경우, dfs() 함수 호출시 인덱스가 들어간다(중복조합도 마찬가지)**
<br><br>

### 문제 35 조합 풀이
#### 풀이1. DFS로 k개 조합 생성
조합의 수를 구하는 수식은 n!/k!(n-k)! 이며, 이 문제 예제 입력값 n=4, k=2의 경우 4!/2!(4-2)! = 6 으로, 이 문제의 출력 또한 6개의 결과가 있으므로 일치한다.<br>
그러나 순열 문제와 마찬가지로 개수뿐만이 아닌 모든 결과를 출력하는 일은 다소 까다로운 편이다.

n=5, k=3일 때 5!/3!(5-3)! = 10 이며 10가지 경우를 모두 나열해보면 그림 12-13(347p)과 같다.

> 그림 12-13 조합을 나열한 결과

<br>

순열의 경우 자기 자신을 제외하고 모든 요소를 next_elements로 처리했으나, 이와 달리 조합의 경우 자기 자신뿐만 아니라 앞의 모든 요소를 배제하고 next_elements를 구성한다.<br>
따라서 여기서는 그냥 elements라는 이름으로 다음과 같이 처리한다.
```python
def dfs(elements, start: int, k: int):
    if k == 0:
        results.append(elements[:])
        return
        
    for i in range(start, n + 1):
        elements.append(i)
        dfs(elements, i + 1, k - 1)
        elements.pop()
```
여기서는 1부터 순서대로 for 문으로 반복하되, 재귀 호출할 때 넘겨주는 값은 자기 자신 이전의 모든 값을 고정하여 넘긴다.<br>
따라서 남아 있는 값끼리 나머지 조합을 수행하게 되며, k=0이 되면 결과에 삽입한다.<br>
마찬가지로 참조로 처리되지 않도록, 결과는 [:] 연산자로 값 자체를 복사해 추가한다.

전체 코드는 다음과 같다.
```python
def combine(self, n: int, k: int) -> List[List[int]]:
    results = []
    
    def dfs(elements, start: int, k: int):
        if k == 0:
            results.append(elements[:])
            return
        
        # 자신 이전의 모든 값을 고정하여 재귀 호출
        for i in range(start, n + 1):
            elements.append(i)
            dfs(elements, i + 1, k - 1)
            elements.pop()
            
    dfs([], 1, k)
    return results
```
34번 순열 문제와 이 문제는 서로 비슷한 면이 있지만, 순열과 조합이 다르듯 구현 방식에도 차이가 있다.<br>
무엇보다 이 문제는 모든 순열을 생성하는 34번 문제와 달리 k개의 조합만을 생성해야 한다는 제약 조건이 추가된 문제다.<br>
따라서 dfs() 함수에서 k 값을 별도로 전달받아 1씩 줄여나가며 재귀 호출하는 구조로, k가 0이 되면 바로 빠져나가는 로직이 추가되어 있다.<br>
그러므로 조금 다른 유형의 문제라는 점을 염두에 두고 유심히 코드를 살펴본다면 어럽지 않게 특징과 차이점을 파악할 수 있을 것이다.
<br><br>

#### 풀이2. itertools 모듈 사용
이 문제도 itertools를 이용해 한 줄 풀이가 가능하다.<br>
34번 순열 문제에서 itertools.permutations()가 있었던 것처럼 itertools.combination() 도 당연히 지원한다.
<br><br>

```python
def combine(self, n: int, k: int) -> List[List[int]]:
    return list(itertools.combinations(range(1, n + 1), k))
```
<br>

풀이1은 536밀리초, 풀이2는 76밀리초가 걸린다.
<br><br>

### 문제 36 조합의 합
> 351p

* **내가 짠 코드**<br>
```python
# 중복조합 만들어야 함

def combination(candi, tar):
  result = []

  def dfs(chosen,cnt):
    if sum(chosen) == 7:
      result.append(chosen[:])

    for i in range(cnt, len(candi)+1):
      chosen.append(candi[i-1])
      dfs(chosen,i+1)
      chosen.pop()

  dfs([],1)
  return result

candidates = [2,3,6,7]
target = 7
print(combination(candidates, target))
```
중복조합을 만들려다가 조합까지만 만들었다. -> 구현 못함
<br><br>

### 문제 36 조합의 합 풀이
#### 풀이1. DFS로 중복 조합 그래프 탐색
이제 조합을 응용한 문제를 풀어볼 차례다.<br>
합 target을 만들 수 있는 모든 번호 조합을 찾는 문제인데, 앞서 순열 문제와 유사하게 DFS(깊이 우선 탐색)와 백트래킹으로 풀이할 수 있다.<br>
간단히 구조를 그려보면 다음 그림 12-15(352p)과 같은 입력값의 중복 조합 그래프를 풀이하는 문제로 도식화 할 수 있다.

> 그림 12-15

모든 중복 조합에서 찾아야 하기 때문에, 이 그림과 같이 항상 부모의 값부터 시작하는 그래프로 구성할 수 있다.<br>
만약 조합이 아니라 순열을 찾는 문제하면 자식 노드는 항상 처음부터 시작해야 해서 훨씬 더 많은 계산이 필요할 것이다.<br>
그러나 **조합은 각각의 노드가 자기 자신부터 하위 원소까지의 나열로만 정리할 수 있다.**<br>
이제 이 중복 조합 그래프를 DFS로 다음과 같이 탐색할 수 있다.
```python
def dfs(csum, index, path):
    ...
    for i in range(index, len(candidates)):
        dfs(csum - candidates[i], i, path + [candidates[i]])
```
DFS로 재귀 호출하되, dfs() 함수의 첫 번째 파라미터는 합을 갱신해나갈 csum(candidates_sum이라는 의미로 정한 변수명이다), 두 번째 파라미터는 순서(자기 자신을 포함하는), 세 번째 파라미터는 지금까지의 탐색 경로로 정한다.<br>
그런데 이 탐색 코드는 종료 조선이 없으며, 자기 자신을 포함하기 때문에 무한히 탐색하게 될 것이다.<br>
따라서 종료 조건이 필요하다. 종료 조건은 다음 2가지 경우로 정한다.
```
1. csum < 0(마이너스인 경우): 목표값을 초과한 경우로 탐색을 종료한다.
2. csum = 0(0인 경우): csum의 초기값은 target이며, 따라서 csum의 0은 target과 일치하는 정답이므로 결과 리스트에 추가하고 탐색을 종료한다.
```
이런 경우에 다음과 같이 종료하게 되면 가지치기가 되어 더 이상 불필요한 탐색은 하지 않게 될 것이다.
```python
def dfs(csum, index, path):
    if csum < 0:
        return
    if csum == 0:
        result.append(path)
        return
```
<br>

나머지 경우는 계속해서 탐색을 시도한다.<br>
그림 12-15(352p)에서 ...으로 표현한 것처럼 종료 조건을 만족하지 못할 경우에는 계속해서 DFS(깊이 우선 탐색)를 시도한다.<br>
이제 두 파트를 합쳐서 전체 코드를 정리해보면 다음과 같다.
```python
def combinationSum(self, candidates: List[int], target: int) \ 
        -> List[List[int]]:
    result = []
    
    def dfs(csum, index, path):
        # 종료 조건
        if csum < 0:
            return
        if csum == 0:
            result.append(path)
            return
            
        # 자신 부터 하위 원소 까지의 나열 재귀 호출
        for i in range(index, len(candidates)):
            dfs(csum - candidates[i], i, path + [candidates[i]])  # 1
            
    dfs(target, 0, [])
    return result
```
그런데 만약 입력값에 0이 포함되어 있다면 어떻게 될까?<br>
종료 조건을 만족할 수 없기 때문에 무한히 깊이 탐색을 시도하게 된다.<br>
따라서 실제로는 입력값 0에 대한 예외 처리도 필요하다.<br>
만약 이 문제를 조합이 아닌 순열 문제로 풀이해본다면 어떻게 될까?

앞서 문제 설명을 시작할 때 잠깐 언급했지만 완성된 전체 코드에서 dfs()를 호출하는 '# 1'에서 다음과 같이 i가 아닌 0을 기입하면 된다.<br>
그렇게 하면 항상 첫 번째 값부터 탐색을 시도하기 때문에 순열로 풀이할 수 있을 것이다.
```python
dfs(csum - candidates[i], 0, path + [candidates[i]])
```
<br><br>

* **번외(또다른 코드)**<br>
```python

def whichone(candidates, target):
    result = []
    
    # target이 그대로 candidates에 있는 경우
    if target in candidates:
        result.append([target])
        candidates.remove(target)
    
    def dfs(tmp):
        
        # target이 되기 위해 더해야 할 값(정답코드에서는 csum)이 
        # candidate의 min값 보다 작을 경우 return
        if target - sum(tmp) < min(candidates):
            tmp = [c]
            return 
        
        # csum이 candidate에 있으면 바로 append
        if target - sum(tmp) in candidates:
            tmp.append(target - sum(tmp))
            if set(tmp) not in list(map(set, result)):
                result.append(tmp[:])
       
        # 아닌 경우, 또 candidates들을 더하기!
        else:
            for other in candidates:
                tmp.append(other)
                dfs(tmp)
                tmp.pop()

                
    for c in candidates:
        dfs([c])
            
    return result
```



















