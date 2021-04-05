# 7장 배열
> 배열은 값 또는 변수 엘리먼트의 집합으로 구성된 구조로, 하나 이상의 인덱스 또는 키로 식별된다.

<br>자료구조는 크게 메모리 공간 기반의 **연속 방식**과 **포인터 기반**의 연결 방식으로 나뉜다.<br>
배열은 이 중에서 연속 방식의 가장 기본이 되는 자료형이다.<br>
(연결 방식의 가장 기본이 되는 자료형은 연결 리스트)

### 동적 배열의 등장
배열은 고정된 크기만큼의 연속된 메모리 할당이다.<br>
그러나 실제 데이터에서는 전체 크기를 가늠하기 힘들 때가 많다.<br>
그렇다면 미리 크기를 지정하지 않고 자동으로 조정할 수 있다면 좋지 않을까?<br>
이러한 고민을 해결하고자 크기를 지정하지 않고 자동으로 리사이징하는 배열인 **동적 배열**이 등장했다.

파이썬에서는 리스트가 동적 배열 자료형이다.<br>

### 동적 배열의 원리
동적 배열의 원리는 간단하다.<br>
미리 초깃값을 작게 잡아 배열을 생성하고, 데이터가 추가되면서 꽉 채워지면, 늘려주고 모두 복사하는 식이다.<br>
대개는 **더블링(Doubling)** 이라 하여 2배씩 늘려주게 되는데, 당연히 모든 언어가 항상 그런 것은 아니다.

그렇다면 파이썬의 더블링 구조를 잠깐 살펴보자.<br>
CPython의 내부 구현을 살펴보면 동적 배열인 리스트의 구현은 CPython의 listobject.c에 정의되어 있다.<br>
여기에는 다음 코드에서 주석에 기술된 수치만큼인 0, 4, 8, 16, ... 순으로 재할당하도록 정의되어 있다.
```python
// cpython/Objects/listobject.c
// The growth pattern is: 0, 4, 8, 16, 25, 35, 46, 58, 72, 88, ...
new_allocated = (size_t)newsize + (newsize >> 3) + (newsize < 9 ? 3 : 6);
```
이 재할당 비율을 그로스 팩터(Growth Factor), 즉 '성장 인자'라고 한다.<br>
파이썬의 그로스 팩터는 초반에는 2배씩 늘려 가지만, 전체적으로는 약 1.125배로, 다른 언어에 비해서는 다소 조금만 늘려가는 형태로 구현되어 있다.


### 정리
동적 배열은 정적 배열과 달리 크기를 지정할 필요가 없어 매우 편리하게 활용할 수 있으며, 조회 또한 기존의 배열과 동일하게 O(1)에 가능하다.<br>
그러나 더블링이 필요할 만큼 공간이 차게 되면, 새로운 메모리 공간에 더 큰 크기의 배열을 할당하고 기존 데이터를 복사하는 작업이 필요하므로 O(n) 비용이 발생한다.<br>
즉 최악의 경우 삽입 시 O(n)이 되지만 자주 일어나는 일은 아니므로, 분할 상환 분석에 따른 입력 시간은 여전히 O(1)이다.<br>
이처럼 동적 배열은 분할 상환 분석에 따른 시간 복잡도를 설명하는 대표적인 자료형이기도 하다.<br>
<br>
## 리트코드 문제
### 07. 두 수의 합
> 173p

* **내가 짠 코드**
```python
def two_sum(nums:list,target:int) -> list:
  new_list = []
  sum = 0

  for i in range(len(nums)):
    for j in range(i+1,len(nums)):
      sum = nums[i] + nums[j]
      if sum == target:
        new_list.append(i)
        new_list.append(j)
        return new_list



nums = list(map(int, input().split()))
target = int(input())

print(two_sum(nums,target))
```
<br>

* **꿀팁**<br>
한 번에 리스트로 입력 받고 싶다면 다음과 같은 형식을 사용하면 된다.
```python
l = list(map(int, input().split()))
```
<br>

* **풀이1. 브루트 포스로 계산**<br>
이 문제는 매우 쉽다. 그러나 최적화할 수 있는 여러 가지 방법이 숨어 있어서, 코딩 인터뷰 시 높은 빈도로 출제되는 문제다.

  + 브루트 포스(Brute-Force)<br>
  배열을 2번 반복하면서 모든 조합을 더해서 일일이 확인해보는 무차별 대입 방식(173p)
  
```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  for i in range(len(nums)):
    for j in range(i + 1, len(nums)):
      if nums[i] + nums[j] == target:
        return [i, j]
```

이 경우 풀이 시간 복잡도는 O(n^2)이다.<br>
정확히는 (1/2)n^2 정도가 되겠지만 시간 복잡도를 표기할 때 상수항은 기입하지 않기 때문에 n^2이다.<br>
문제가 풀리기는 하지만 브루트 포스 방식은 지나치게 느리다. 이 풀이는 **5284밀리초**나 소요됐다.<br>
앞으로 문제를 풀 때 비효율적인 풀이법에서 꼭 한 번씩 첫 번째 풀이로 등장할 것이다.
<br>

* **풀이2. in을 이용한 탐색**<br>
모든 조합을 비교하지 않고 정답, 즉 타겟에서 첫 번째 값을 뺀 값 target - n이 존재하는지 탐색하는 문제로 변경해보자.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  for i, n in enumerate(nums):
    complement = target - n
    
    if complement in nums[i + 1]:
      return [nums.index(n), nums[i + 1:].index(complement) + (i + 1)]
```

여기서 in의 시간 복잡도는 O(n)이고, 따라서 전체 시간 복잡도는 이전과 동일한 O(n^2)이다.<br>
그런데 여기서는 같은 시간 복잡도라도 in 연산 쪽이 훨씬 더 가볍고 빠르다. **864밀리초** 만에 실행이 가능하다<br>

* **풀이3. 첫 번째 수를 뺀 결과 키 조회**<br>
이번에는 시간 복잡도를 개선해 속도를 높여보자.<br>
비교나 탐색 대신 한 번에 정답을 찾을 수 있는 방법은 없을까?

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  nums_map + {}
  # 키와 값을 바꿔서 딕셔너리로 저장
  for i, num in enumerate(nums):
    nums_map[num] = i
    
  # 타겟에서 첫 번째 수를 뺀 결과를 키로 조회
  for i, num in enumerate(nums):
    if target - num in nums_map and i != nums_map[target - num]:
      return [i, nums_map[target - num]]
```

이 경우 타겟에서 첫 번째 수를 빼면 두 번째 수를 바로 알아낼 수 있다.<br>
그렇다면 두 번째 수를 키로 하고 기존의 인덱스는 값으로 바꿔서 딕셔너리로 저장해두면, 나중에 두 번째 수를 키로 조회해서 정답을 즉시 찾아낼 수 있을 것이다.<br>
이제 타겟에서 첫 번째 수를 뺀 결과를 키로 조회해보면 두 번째 수의 인덱스를 즉시 조회할 수 있다.<br>
딕셔너리는 해시 테이블로 구현되어 있고, 이 경우 조회는 평균적으로 O(1)에 가능하다.<br>
실제로 불과 **48밀리초** 만에 실행된다.

    + 참고 | 딕셔너리의 주요 연산 시간 복잡도 129p

* **풀이4. 조회 구조 개선**<br>
딕셔너리 저장과 조회를 2개의 for 문으로 각각 처리했던 방식을 좀 더 개선해서 이번에는 하나의 for로 합쳐서 처리해보자.<br>
이 경우 전체를 모두 저장할 필요 없이 정답을 찾게 되면 함수를 바로 빠져나올 수 있다.<br>
그러나 두 번째 값을 찾기 위해 어차피 매번 비교해야 하기 때문에 앞서 풀이에 비해 성능상의 큰 이점은 없을 것 같다.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  nums_map ={}
  # 하나의 for 문으로 통합
  for i, num in enumerate(nums):
    nums_map[num] = i
    if target - num in nums_map:
      return [nums_map[target - nums], i]
```

실행 속도를 확인해보니 예상했던 것처럼 **44밀리초**로 앞서 풀이와 큰 차이는 없다.<br>
그러나 코드는 한결 더 간결해졌다.

* **풀이5. 투 포인터 이용**<br>
  + 투 포인터란
    왼쪽 포인터와 오른쪽 포인터의 합이 타겟보다 크다면 오른쪽 포인터를 왼쪽으로, 작다면 왼쪽 포인터를 오른쪽으로 옮기면서 값을 조정하는 방식이다.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  left, right = 0, len(nums) - 1
  while not left == right:
    # 합이 타겟보다 작으면 왼쪽 포인터를 오른쪼긍로
    if nums[left] + nums[right] < target:
      left += 1
    # 합이 타겟보다 크면 오른쪽 포인터를 왼쪽으로
    elif nums[left] + nums[right] > target:
      right -= 1
    else:
      return [left, right]
```

이처럼 투 포인터로 구현해보니 코드도 간결하고 이해하기 쉽다.<br>
투 포인터의 시간 복잡도도 O(n)이고, 불필요한 추가 계산도 필요 없어, 매우 빠른 속도가 기대된다.<br>
그러나 이 문제는 투 포인터로 풀 수 없다.<br>
왜냐면 입력값은 nums는 정렬된 상태가 아니기 때문이다.<br>
제대로 풀이하려면 정렬이 필요하다.<br>
따라서 다음과 같이 정렬 로직을 추가해보자.

```python
def towSum(self, nums: List[int], target: int) -> List[int]:
  nums.sort()
  ...
```

그런데 이렇게 하면 인덱스는 모두 엉망이 되기 때문에 매우 심각한 문제가 발생한다.<br>
만약 이 문제가 인덱스가 아니라 값을 찾아내는 문제라면, 얼마든지 정렬하고 투 포인터로 풀이할 수 있다.<br>
하지만 이처럼 인덱스를 찾아내는 문제에서는 이렇게 정렬을 통해 인덱스를 섞어 버리면 곤란하다.<br>
원래 인덱스를 찾을 수가 없기 때문이다.

* **풀이6. 고(GO) 구현**<br>
178p 참고<br>
(**8밀리초** 만에 실행된다.)
<br>

### 08. 빗물 트래핑
> 180p

* **내가 짠 코드**















