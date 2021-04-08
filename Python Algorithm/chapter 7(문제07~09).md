# 목차
* [chapter 7. 배열](#7장-배열)<br>
  + [동적 배열의 등장](#동적-배열의-등장)
  + [동적 배열의 원리](#동적-배열의-원리)
  + [정리](#정리)
* [리트코드 문제](#리트코드-문제)
  + [문제 07 두 수의 합](#문제-07-두-수의-합)
  + [문제 07 두 수의 합 풀이](#문제-07-두-수의-합-풀이)
    - [풀이1. 브루트 포스로 계산](#풀이1-브루트-포스로-계산)
    - [풀이2. in을 이용한 탐색](#풀이2-in을-이용한-탐색)
    - [풀이3. 첫 번째 수를 뺀 결과 키 조회](#풀이3-첫-번째-수를-뺀-결과-키-조회)
    - [풀이4. 조회 구조 개선](#풀이4-조회-구조-개선)
    - [풀이5. 투 포인터 이용](#풀이5-투-포인터-이용)
    - [풀이6. 고(GO) 구현](#풀이6-고go-구현)
  + [문제 08 빗물 트래핑](#문제-08-빗물-트래핑)
  + [문제 08 빗물 트래핑 풀이](#문제-08-빗물-트래핑-풀이)
    - [풀이1. 투 포인터를 최대로 이동](#풀이1-투-포인터를-최대로-이동)
    - [풀이2. 스택 쌓기](#풀이2-스택-쌓기)
  + [문제 09 세 수의 합](#문제-09-세-수의-합)
  + [문제 09 세 수의 합 풀이](#문제-09-세-수의-합-풀이)
    - [풀이1. 브루트 포스로 계산](#풀이1-브루트-포스로-계산)
    - [풀이2. 투 포인터로 합 계산](#풀이2-투-포인터로-합-계산)





# 7장 배열
> 배열은 값 또는 변수 엘리먼트의 집합으로 구성된 구조로, 하나 이상의 인덱스 또는 키로 식별된다.

<br>자료구조는 크게 메모리 공간 기반의 **연속 방식**과 포인터 기반의 **연결 방식**으로 나뉜다.<br>
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
```c
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
### 문제 07 두 수의 합
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

### 문제 07 두 수의 합 풀이

#### 풀이1. 브루트 포스로 계산 <br>
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
<br><br>

#### 풀이2. in을 이용한 탐색<br>
모든 조합을 비교하지 않고 정답, 즉 타겟에서 첫 번째 값을 뺀 값 target - n이 존재하는지 탐색하는 문제로 변경해보자.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  for i, n in enumerate(nums):
    complement = target - n
    
    if complement in nums[i + 1]:
      return [nums.index(n), nums[i + 1:].index(complement) + (i + 1)]
```

여기서 in의 시간 복잡도는 O(n)이고, 따라서 전체 시간 복잡도는 이전과 동일한 O(n^2)이다.<br>
그런데 여기서는 같은 시간 복잡도라도 in 연산 쪽이 훨씬 더 가볍고 빠르다. **864밀리초** 만에 실행이 가능하다.
<br><br>

#### 풀이3. 첫 번째 수를 뺀 결과 키 조회<br>
이번에는 시간 복잡도를 개선해 속도를 높여보자.<br>
비교나 탐색 대신 한 번에 정답을 찾을 수 있는 방법은 없을까?

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  nums_map = {}
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
<br><br> 

#### 풀이4. 조회 구조 개선<br>
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
<br><br>

#### 풀이5. 투 포인터 이용<br>
  + 투 포인터란<br>
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
그러나 이 문제는 투 포인터로 풀 수 없다. 왜냐면 입력값은 nums는 정렬된 상태가 아니기 때문이다.<br>
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
<br><br>

#### 풀이6. 고(GO) 구현<br>
178p 참고<br>
(**8밀리초** 만에 실행된다.)
<br>

-----

### 문제 08 빗물 트래핑
> 180p


### 문제 08 빗물 트래핑 풀이
#### 풀이1. 투 포인터를 최대로 이동
그림 7-5(180p)에서 가장 높이가 높은 막대를 한번 살펴보자.<br>
막대는 높고 낮음에 무관하게, 전체 부피에 영향을 끼치지 않으면서 그저 왼쪽과 오른쪽을 가르는 장벽 역할을 한다.
<br><br>

```python
volume += left_max - height[left]
...
volume += right_max = height[right]
```
이처럼 최대 높이의 막대까지 각각 좌우 기둥 최대 높이 left_max, right_max가 현재 높이와의 차이만틈 물 높이 volume을 더해 나간다.
<br><br>

```python
if left_max <= right_max:
  volume += left_max = height[left]
  left += 1
else:
  volume += right_max - height[right]
  right -= 1
```
이 경우 적어도 낮은 쪽은 그만큼 항상 채워질 것이기 때문에, 좌우 어느 쪽이든 낮은 쪽은 높은 쪽을 향해서 포인터가 가운데로 점점 이동한다.<br>
이렇게 하면 그럼 7-5(180p)에서 가장 높이가 높은 막대, 즉 '최대' 지점에서 좌우 포인터가 서로 만나게 되며 O(n)에 풀이가 가능하다.<br>

전체 코드는 다음과 같다.

```python
def trap(self, height: List[int]) -> int:
  if not height:
    return 0

  volume = 0
  left, right = 0, len(height) - 1
  left_max, right_max = height[left], height[right]

  while left < right:
    left_max, right_max = max(height[left], left_max),
                          max(height[right], right_max)
    # 더 높은 쪽을 향해 투 포인터 이동
    if left_max <= right_max:
      volume += left_max - height[left]
      left += 1
    else:
      volume += right_max - height[right]
      right -= 1
  return volume
```

#### 풀이2. 스택 쌓기
그림 7-6(182p)과 같이 스택에 쌓아 나가면서 현재 높이가 이전 높이보다 높을 때, 즉 꺾이는 부분 변곡점을 기준으로 격차만큼 물 높이 volume을 채운다.<br>
이전 높이는 고정된 형태가 아니라 들쑥날쑥하기 때문에, 계속 스택으로 채워 나가다가 변곡점을 만날 때마다 스택에서 하나씩 꺼내면서 이전과의 차이만큼 물 높이를 채워 나간다.<br>
스택으로 이전 항목들을 되돌아보며 체크하기는 하지만, 기본적으로 한 번만 살펴보고 때문에 마찬가지로 O(n)에 풀이가 가능하다.

전체 코드는 다음과 같다.
```python
def trap(self, height: List[int]) -> int:
  stack = []
  volume = 0
  
  for i in range(len(height)):
    # 변곡점을 만나는 경우
    while stack and height[i] > height[stack[-1]]:
      # 스택에서 꺼낸다
      top = stack.pop()
      
      if not len(stack):
        break
        
      # 이전과의 차이만큼 물 높이 처리
      distance = i - stack[-1] - 1
      waters = min(height[i], height[stack[-1]]) - height[top]
      
      volume += distance * waters
      
    stack.append(i)
  return volume
```

이 문제는 상당히 어려운 문제다. 특히 스택 쌀기 풀이는 직관적으로 떠올리기가 쉽지 않을 뿐더러 풀이 방법 또한 많이 고민해봐야 하는 문제이다.<br>
하지만 면접관이 이 문제를 화이트보드에 풀어보라고 요구하는 경우에는,<br>
그림 7-5(180p) 또는 그림 7-6(182p)을 화이트보드에 그리는 정도로 개념을 잘 잡아서 설명하면 그리 어렵지 않을 것 같다.

-----

### 문제 09 세 수의 합
> 184p

* **내가 짠 코드**
```python
def sum(nums: list) -> list:
  nums.sort()
  dict_nums = {}
  new_list = []

  for i,num in enumerate(nums):
    dict_nums[num] = i # 딕셔너리에 '값':'인덱스' 형태로 넣는다
    for j in range(i+1,len(nums)):
      two_sum = nums[i] + nums[j]
      last_num = 0 - two_sum
      
      # last_num이 dict_num(키)에 있다면
      if last_num in dict_nums:
        # dict_num[last_num]의 값(=인덱스)이 i 또는 j가 아니라면
        if dict_nums[last_num] != i or j:
          # 세 수를 정렬한 리스트가 new_list에 없다면
          if sorted([nums[i],nums[j],last_num]) not in new_list:
            new_list.append(sorted([nums[i],nums[j],last_num]))

  return(new_list)


nums = [-1,0,1,2,-1,-4]
print(sum(nums))
```
<br>

### 문제 09 세 수의 합 풀이
#### 풀이1. 브루트 포스로 계산
가장 먼저 브루트 포스로 풀이해보자.<br>
언뜻 보면 O(n^3) 정도에 풀이가 가능해 보인다. 그러나 이 경우 타임아웃이 발생해 풀리지 않을 것도 같다.<br>
어쨌든 일단 쉽게 접근할 수 있는 브루트 포스 풀이법을 한번 시도해보자.

앞뒤로 같은 값이 있을 경우, 이를 쉽게 처리하기 위해 먼저 다음과 같이 sort() 함수를 사용해 정렬부터 한다.
```python
nums.sort()
```
정렬의 시간 복잡도는 O(nlogn)이며, 정렬한 결과 [-4, -1, -1, 0, 1, 2] 가 나온다.

이 브루트 포스 풀이에는 중복된 값이 있을 수 있으므로 이 경우 다음과 같이 continue로 건너뛰도록 처리한다.
```python
if i > 0 and nums[i] == nums[i - 1]:
  continue
```

이제 전체 코드는 다음과 같다.
```python
def threeSum(self, nums: List[int]) -> List[List[int]]:
  results = []
  nums.sort()
  
  # 브루트 포스 n^3 반복
  for i in range(len(nums) - 2):
    # 중복된 값 건너뛰기
    if i > 0 and nums[i] == nums[i - 1]:
      continue
    for j in range(i + 1, len(nums) - 1):
      if j > i + 1 and nums[j] == nums[j - 1]:
        continue
      for k in range(j + 1, len(nums)):
        if k > j + 1 and nums[k] == nums[k - 1]:
          continue
        if nums[i] + nums[j] + nums[k] == 0:
          results.append([nums[i], nums[j], nums[k]])
          
  return results
```
틀린 부분은 없지만 예상대로 이 방식으로는 문제가 풀리지 않는다. 타임아웃으로 풀이에 실패한다.
<br><br>

#### 풀이2. 투 포인터로 합 계산
i를 축으로 하고, 중복된 값을 건너뛰게 한 부분은 다음과 같이 앞서 풀이와 동일하다.
```python
for i in range(len(nums) - 2):
  if i > 0 and nums[i] == nums[i - 1]:
    continue
```
<br>

이제 중복이 아닌 경우 투 포인터로 풀이할 수 있다. 이를 도식화해보면 그림 7-8(186p)과 같다.

i의 다음 지점과 마지막 지점을 left, right로 설정하고 간격을 좁혀가며 sum을 계산한다.<br> 
이 부분을 코드로 구현해보면 다음과 같다.
```python
left, right = i + 1, len(nums) - 1
while left < right:
  sum = nums[i] + nums[left] + nums[right]
```
투 포인터가 간격을 좁혀나가며 합 sum을 계산한다.<br>

이제 다음 코드를 살펴보자.
```python
if sum < 0:
  left += 1
elif sum > 0:
  right -= 1
```
sum이 0보다 작다면 값을 더 키워야 하므로 left를 우측으로 이동하고, 0보다 크다면 값을 더 작게 하기 위해 right를 좌측으로 이동한다.
<br><br>

```python
if sum < 0:
  ...
elif sum > 0:
  ...
else:
  results.append((nums[i], nums[left], nums[right]))
  
  while left < right and nums[left] == nums[left + 1]:
    left += 1
  while left < right and nums[right] == nums[right - 1]:
    right -= 1
```
sum = 0이면 정답이므로, 이 경우 결과를 리스트 변수 results에 추가한다. 추가한 다음에는 left, right 양 옆으로 동일한 값이 있을 수 있으므로 이를 left += 1, right -= 1을 반복해서 스킵하도록 처리한다.
<br><br>

```python
left += 1
right -= 1
```
마지막으로 left를 한 칸 우측으로, right를 한 칸 왼쪽으로 더 이동하고 다음으로 넘긴다.

얼핏 생각해보면 left 또는 right 둘 중 하나만 이동해야 하는 게 아닌가 싶지만, 어차피 sum=0인 상황이므로 어느 한쪽만 이동하는 경우는 불가능하다.<br>
나머지 값을 찾으려면 결국 둘 다 모두 움직여야 한다.

앞서 설명한 내용들을 모두 정리하면, 전체 코드는 다음과 같다.
```python
def threeSum(self, nums: List[int]) -> List[List[int]]:
  results = []
  nums.sort()
  
  for i in range(len(nums) - 2):
    # 중복된 값 건너뛰기
    if i > 0 and nums[i] == nums[i - 1]:
      continue
      
    # 간격을 좁혀가며 합 sum 계산
    left, right = i + 1, len(nums) - 1
    while left < right:
      sum = nums[i] + nums[left] + nums[right]
      if sum < 0:
        left += 1
      elif sum > 0:
        right -= 1
      else:
        # sum = 0인 경우이므로 정답 및 스킵 처리
        results.append((nums[i], nums[left], nums[right]))
  
        while left < right and nums[left] == nums[left + 1]:
          left += 1
        while left < right and nums[right] == nums[right - 1]:
          right -= 1
        left += 1
        right -= 1
        
  return results        
```

* **참고 | 투 포인터**<br>
  투 포인터는 여러 가지 방식이 있지만, 대개는 시작점과 끝점 또는 왼쪽 포인터와 오른쪽 포인터 두 지점을 기준으로 하는 문제 풀이 전략을 뜻한다.<br>
  범위를 좁혀나가기 위해서는, 일반적으로 배열이 정렬되어 있을 때 좀 더 유용하다.
  
  투 포인터는 주로 정렬된 배열을 대상으로 하며, 2개의 포인터가 좌우로 자유롭게 움직이며 문제를 풀이한다. 이 때문에 투 포인터라는 이름이 붙었다.<br>
  (슬라이딩 윈도우와 여러모로 비슷한 점이 많음)
  
  


















