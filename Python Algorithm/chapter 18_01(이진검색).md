# 목차

# 18장 이진 검색
> 이진 검색(Binary Search)이란 정렬된 배열어|서 타켓을 찾는 검색 알고리즘이다.

**이진 검색은** 값을 찾아내는 **시간 복잡도가 O(log n)** 이라는 점에서 대표적인 로그 시간 알고리즘이며, 이진 탐색 트리(Binary Search Tree)(이하 BST)와도 유사한 점이 많다.<br>
그러나 이진 탐색 트리가 정렬된 구조를 저장하고 탐색하는 **'자료구조'** 라면, 이진 검색은 정렬된 배열에서 값을 찾아내는 **'알고리즘'** 자체를 지칭한다.

14장에서 BST(이진 탐색 트리)를 소개할 때 카드 마술 얘기를 언급했다.<br>
1부터 100까지 관객이 마음속에 생각해둔 숫자를 맞춰 보겠다고 할 때, 단 7번의 카드만 제시하면 7번 이내에 무조건 맞출 수 있기 때문이다.<br>
정말로 그런지 간단한 실험을 진행해보자.

관객이 만약 77을 택했다면, 이진 검색으로 숫자를 맞춰보자.<br>
숫자를 맞추는 과정을 그림 18-1에 나타냈다.

<img src="https://user-images.githubusercontent.com/55045377/128652708-805b1a1c-1994-492c-b855-0e81d5edb8c7.png" width=50% height=50%>

먼저, 50부터 탐색을 시작한다.<br>
77은 50보다 크므로 왼쪽 포인터를 오른쪽으로 이동한다.<br>
그다음은 75다. 77은 75보다 크므로 마찬가지로 왼쪽 포인터를 오른쪽으로 이동한다.<br>
다음은 87이다. 77은 87보다는 작으므로 이번에는 오른쪽 포인터를 왼쪽으로 이동한다.

이런 식으로 점점 범위를 좁혀 나가면 7번 이내에 무조건 숫자를 맞출 수 있게 된다.<br>
마찬가지로 이 그림에서도 7번의 비교로 정답을 찾아냈다.

이 마술의 핵심은 이진 검색의 마법이고, 궁극적으로는 로그의 힘이다.
```python
>>> math.log2(100)
6.643856189774724
```
100의 이진 로그 log(2)100의 결과를 이와 같이 코드로 계산해보면 6.6 정도로 7번 만에 모두 찾아낼 수 있다.

이제 관련 문제를 풀어보면서 이진 검색을 코드로 직접 구현해보자.

<br><br>

## 문제 65 이진 검색
> 518p

* 정렬된 nums를 입력받아 이진 검색으로 target에 해당하는 인덱스를 찾아라.
* 입력
```
nums = [-1,0,3,5,9,12], target = 9
```
* 출력
```
4
```

<br><br>

### 문제 65 내가 짠 코드
* 잘못된 풀이1
```python
def solution(nums, target):
  while len(nums) > 0:
    mid = (len(nums)//2) - 1
    if nums[mid] > target:
      nums = nums[:mid+1]
    elif nums[mid] < target:
      nums = nums[mid+1:]
    else:
      return mid

nums = [-1,0,3,5,9,12]
target = 9
print(solution(nums, target))
```
`nums`를 `mid`값을 기준으로 슬라이싱하는 방법으로 풀었는데, 이렇게 하니 정확한 인덱스 값이 나오지 않았다.

<br>

* 잘못된 풀이2
```python
def solution(nums, target):
  start = 0
  last = len(nums) - 1
  while start < last:
    mid = (last - start)//2
    if nums[mid] > target:
      last = mid
    elif nums[mid] < target:
      start = mid + 1
    else:
      return mid

nums = [-1,0,3,5,9,12]
target = 9
print(solution(nums, target))
```
`mid`값을 (`last`-`start`)//2 로 두니 `mid`값은 계속 작을 수 밖에 없었고, 무한 루프가 발생했다.

<br>

* 완성 코드
```python
def solution(nums, target):
  start = 0
  last = len(nums) - 1
  while start < last:
    mid = (last + start)//2
    if nums[mid] > target:
      last = mid
    elif nums[mid] < target:
      start = mid + 1
    else:
      return mid

nums = [-1,0,3,5,9,12]
target = 9
print(solution(nums, target))
```
`mid` 값을 (`last`+`start`)//2 로 두니 의도한 대로 mid 값이 부여되었고, 정답도 잘 출력되었다.

<br><br>

## 문제 65 이진 검색 풀이
### 풀이1. 재귀 풀이
























