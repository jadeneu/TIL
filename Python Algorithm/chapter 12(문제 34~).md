# 목차

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
```python

```
























