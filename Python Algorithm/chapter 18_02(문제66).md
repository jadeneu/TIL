# 목차

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
    mid = (start + (last-start))//2
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
    mid = (start + (last-start))//2
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






























