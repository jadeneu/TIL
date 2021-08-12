# 목차
* [chapter 18. 이진 검색](#18장-이진-검색)
	+ [문제 66 회전 정렬된 배열 검색](#문제-66-회전-정렬된-배열-검색)
		- [문제 66 내가 짠 코드](#문제-66-내가-짠-코드)
	+ [문제 66 회전 정렬된 배열 검색 풀이](#문제-66-회전-정렬된-배열-검색-풀이)
		- [풀이1. 피벗을 기준으로 한 이진 검색](#풀이1-피벗을-기준으로-한-이진-검색)
			- [넘파이(NumPy) 모듈로 한 줄로 피벗을 찾을 수 있다?](#넘파이numpy-모듈로-한-줄로-피벗을-찾을-수-있다)
	+ [문제 67 두 배열의 교집합](#문제-67-두-배열의-교집합)
		- [문제 67 내가 짠 코드](#문제-67-내가-짠-코드)
	+ [문제 67 두 배열의 교집합 풀이](#문제-67-두-배열의-교집합-풀이)
		- [풀이1. 브루트 포스로 계산](#풀이1-브루트-포스로-계산)
		- [풀이2. 이진 검색으로 일치 여부 판별](#풀이2-이진-검색으로-일치-여부-판별)
		- [풀이3. 투 포인터로 일치 여부 판별](#풀이3-투-포인터로-일치-여부-판별)

<br><br><br>

# 18장 이진 검색
## 문제 66 회전 정렬된 배열 검색
> 525p

* 특정 피벗을 기준으로 회전하여 정렬된 배열에서 target 값의 인덱스를 출력하라.
* 입력
```
nums = [4,5,6,7,0,1,2], target = 1
```
* 출력
```
5
```
* 설명

정렬된 입력값은 [0,1,2,4,5,6,7]이며 여기서 이진 검색을 통해 1의 위치를 찾은 다음 (위치 1) 원래의 입력값에서 얼마만큼 돌아가 있는지를 확인하여 (4칸), '위치 1 + 4칸 = 인덱스 5'를 리턴한다.

<br><br>

### 문제 66 내가 짠 코드
* 잘못된 풀이
```python
def solution(nums, target):
  sorted_nums = sorted(nums)
  
  target_idx = binary_search(nums, target)
  sorted_target_idx = binary_search(sorted_nums, target)

  if target_idx == -1:
    return target_idx
  else:
    return target_idx + sorted_target_idx

  
def binary_search(lst, target):
  start = 0
  last = len(lst) - 1

  # 이진 검색
  while start < last:
    mid = start + (last-start)//2
    if lst[mid] > target:
      last = mid - 1
    elif lst[mid] < target:
      start = mid - 1
    else:
      return mid
  return -1


nums = [4,5,6,7,0,1,2]
target = 1
print(solution(nums, target))
```
정렬되어 있지 않은 `nums` 리스트를 이진 검색하려고 했다. 이런 실수를 조심해야겠다.

<br>

* 완성된 풀이
```python
def solution(nums, target):
  sorted_nums = sorted(nums)
  
  target_idx = nums.index(target)
  sorted_target_idx = binary_search(sorted_nums, target)

  if target_idx == -1:
    return target_idx
  else:
    return sorted_target_idx + abs(target_idx - sorted_target_idx)

  
def binary_search(lst, target):
  start = 0
  last = len(lst) - 1

  # 이진 검색
  while start < last:
    mid = start + (last-start)//2
    if lst[mid] > target:
      last = mid - 1
    elif lst[mid] < target:
      start = mid - 1
    else:
      return mid
  return -1


nums = [4,5,6,7,0,1,2]
target = 1
print(solution(nums, target))
```
return 값으로 (정렬 리스트에서 `target` 인덱스 값) + ((원래 `target` 인덱스 값) - (정렬 리스트에서 `target` 인덱스 값))을 줘서 정답을 도출해냈다.<br>
그러나 코드가 깔끔하진 않은 것 같다. 더 간단한 풀이로 만들 수 있을 것 같다.

<br><br>

## 문제 66 회전 정렬된 배열 검색 풀이
### 풀이1. 피벗을 기준으로 한 이진 검색
정렬이 되어 있긴 한데, 피벗을 기준으로 입력값이 돌아간 상황이다.<br>
피벗이 어떤 것인지 모르는 상태이므로, 기존 이진 검색을 그대로 활용할 수는 없고 좀 더 특별한 처리가 필요하다.<br>
이 문제에서 예제의 입력값은 [4,5,6,7,0,1,2]이며, 이 경우 **피벗은 인덱스 4**다.<br>
피벗을 찾는 방법은 여러 가지가 있겠지만, 여기서 가장 작은 값을 찾는다면 해당 위치의 인덱스가 피벗이 될 수 있을 것 같다. <br>
다음과 같이 최솟값을 찾아내보자.
```python
left, right = 0, len(nums) - 1
while left < right:
    mid = left + (right - left) // 2
    
    if nums[mid] > nums[right]:
        left = mid + 1
    else:
        right = mid
```
여기서는 이진 검색으로 최솟값을 찾았다.<br>
중앙의 위치를 구하는 코드는 앞서 523 페이지의 '이진 검색 알고리즘 버그'에서 살펴본 버그가 개선된 알고리즘을 적용했다.

<br>

#### 넘파이(NumPy) 모듈로 한 줄로 피벗을 찾을 수 있다?
만약 코딩 테스트가 아니라면 과학 계산 라이브러리인 넘파이(NumPy) 모듈을 활용해 numpy.argmin() 단 한 줄로 arg min을 금방 찾을 수 있다.

<img src="https://user-images.githubusercontent.com/55045377/128960118-1c1df47c-be82-461b-ba29-7dc42eaa9ff6.png" width=40% height=40%>

그러나 여기서는 코딩 테스트인 상황이므로 외부 모듈을 사용할 수 없다. 따라서 다음과 같은 코드로 넘파이의 argmin()을 흉내낼 수 있다.
```python
pivot = nums.index(min(nums))
```

<br><br>

이번에는 재귀가 아닌 반복으로 풀이해보자.<br>
65번 문제의 반복 풀이인 풀이2를 가져와서, 마찬가지로 4행에 중앙의 위치 계산 버그가 개선된 알고리즘을 적용해 다음과 같이 코드를 정리한다.
```python
pivot = left
left, right = 0, len(nums) - 1
while left <= right:
    mid = left + (right - left) // 2  # 자료형을 초과하지 않는 중앙 위치 계산
    mid_pivot = ...
    
    if nums[mid_pivot] < target:
        left = mid + 1
    elif nums[mid_pivot] > target:
        right = mid - 1
    else:
        return mid
    ...
```
여기서는 앞서 최솟값 left를 찾아내 pivot으로 구성하고, 이를 기준으로 피벗의 위치만큼 살짝 틀어준 mid_pivot을 구성한 다음, 다시 이진 검색을 통해 target 값을 찾았다.<br>
**그렇다면 mid_pivot을 실제로는 어떤 식으로 구현하면 될까?** <br>
다음 코드를 한번 살펴보자.
```python
mid_pivot = (mid + pivot) % len(nums)
```
mid_pivot은 중앙의 위치 mid에 피벗 pivot만큼 이동하고, 배열의 길이를 초과할 경우 모듈로 연산으로 회전될 수 있도록 처리했다.<br>
이제 타겟과 값을 비교하는 부분은 mid가 아닌 mid_pivot을 기준으로 하되, left와 right는 mid를 기준으로 이동한다.

예제의 입력값을 기준으로 나타낸 그림 18-4를 살펴보자.

<img src="https://user-images.githubusercontent.com/55045377/128969111-d0072330-2baf-4145-8163-75ef4c0660dc.png" width=50% height=50%>

이 그림에서 값에 대한 비교는 mid_pivot의 위치를 기준으로 하지만, mid의 이동은 기존 이진 검색과 동일하게 left, right를 기준으로 한다.<br>
즉 다른 포인터를 가리키는 셈이며, 실제로 값에 대한 비교는 pivot의 위치인, 4칸 우측으로 떨어진 mid_pivot을 기준으로 한다.<br>
당연히 최종 결과도 mid_pivot의 값을 리턴받아 결과로 삼는다.

이제 전체 코드는 다음과 같다.
```python
from typing import List


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # 예외 처리
        if not nums:
            return -1

        # 최소값 찾아 피벗 설정
        left, right = 0, len(nums) - 1
        while left < right:
            mid = left + (right - left) // 2

            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid

        pivot = left
        
        # 피벗 기준 이진 검색
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            mid_pivot = (mid + pivot) % len(nums)

            if nums[mid_pivot] < target:
                left = mid + 1
            elif nums[mid_pivot] > target:
                right = mid - 1
            else:
                return mid_pivot
        return -1
```

<br><br>

## 문제 67 두 배열의 교집합
> 529p

* 두 배열의 교집합을 구하라.
* 입력
```
nums1 = [1,2,2,1], nums2 = [2,2]
```
* 출력
```
[2]
```

<br>

* 입력
```
nums1 = [4,9,5], nums2 = [9,4,9,8,4]
```
* 출력
```
[9,4]
```

<br><br>

### 문제 67 내가 짠 코드
```python
def solution(nums1, nums2):
  nums1 = sorted(set(nums1))
  nums2 = sorted(set(nums2))
  answer = []

  for i in nums1:
    start, last = 0, len(nums2) - 1
    while start <= last:
      mid = start + (last-start) // 2
      if nums2[mid] > i:
        last = mid - 1
      elif nums2[mid] < i:
        start = mid + 1
      else:
        answer.append(nums2[mid])
        break

  return answer


nums1 = [1,2,2,1]
nums2 = [2,2] 
print(solution(nums1, nums2))
```

<br><br>

## 문제 67 두 배열의 교집합 풀이
### 풀이1. 브루트 포스로 계산
이 문제는 이진 검색과 투 포인터 등 다양한 풀이법을 시도할 수 있다. <br>
먼저 가장 간단하고 직관적인 **브루트 포스(Brute-Force)** 부터 풀이해보자.
```python
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        for n1 in nums1:
            for n2 in nums2:
                if n1 == n2:
                    result.add(n1)

        return result
```
**O(n^2)** 으로 반복하면서 일치하는 경우 무조건 추가한다.<br>
데이터 타입은 집합(set)이기 때문에 속도는 느리긴 해도 중복된 값은 알아서 잘 처리해줄 것이다.

<br><br>

### 풀이2. 이진 검색으로 일치 여부 판별
한쪽은 순서대로 탐색하고 다른 쪽은 정렬해서 이진 검색으로 값을 찾으면, 검색 효율을 획기적으로 높일 수 있다.<br>
이 경우 시간 복잡도는 **O(n log n)** 이 될 것이다.
```python
...
nums2.sort()
for n1 in nums1:
		i2 = bisect.bisect_left(nums2, n1)
		if n1 == nums2[i2]:
				result.add(n1)
```
nums2는 정렬한 상태에서, nums1을 O(n) 순차 반복하면서 nums2를 O(log n) 이진 검색한다. <br>
최초 정렬에 소요되는 O(n log n)을 감안해도 전체 O(n log n)에 가능하므로 앞서 O(n^2) 에 비해 훨씬 좋은 성능을 보인다.<br>
예외 처리를 포함한 전체 코드는 다음과 같다.
```python
import bisect
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        nums2.sort()
        for n1 in nums1:
            # 이진 검색으로 일치 여부 판별
            i2 = bisect.bisect_left(nums2, n1)
            if len(nums2) > 0 and len(nums2) > i2 and n1 == nums2[i2]:
                result.add(n1)

        return result
```

<br><br>

### 풀이3. 투 포인터로 일치 여부 판별
이 문제는 양쪽 다 정렬하여 투 포인터로 풀이할 수도 있다.<br>
마치 병합 정렬 시 마지막에 최종 결과를 비교하는 과정과 유사하다.<br>
다만 일치하는 값을 판별한다는 차이만 있을 뿐이다. <br>
이 경우 각각 정렬에 2 * O(n log n), 비교에 O(2n)이 소요되므로, 마찬가지로 전체 O(n log n)에 풀이가 가능하다. 

전체 코드는 다음과 같다.
```python
from typing import List, Set


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result: Set = set()
        # 양쪽 모두 정렬
        nums1.sort()
        nums2.sort()
        i = j = 0
        # 투 포인터 우측으로 이동하며 일치 여부 판별
        while i < len(nums1) and j < len(nums2):
            if nums1[i] > nums2[j]:
                j += 1
            elif nums1[i] < nums2[j]:
                i += 1
            else:
                result.add(nums1[i])
                i += 1
                j += 1

        return result
```
값이 작은 쪽 배열의 포인터가 한 칸씩 앞으로 이동하는 형태로 해서, 어느 한쪽의 포인터가 끝까지 도달하면 종료한다.<br>
이 경우 정렬을 제외하면, 비교에 따른 시간 복잡도는 싱수항을 제외해서 **O(n)** 에 불과하다.

<br><br><br>





















