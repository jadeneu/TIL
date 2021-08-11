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




