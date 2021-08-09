# 목차
* [chapter 18. 이진 검색](#18장-이진-검색)
  + [문제 65 이진 검색](#문제-65-이진-검색)
    - [문제 65 내가 짠 코드](#문제-65-내가-짠-코드)
  + [문제 65 이진 검색 풀이](#문제-65-이진-검색-풀이)
    - [풀이1. 재귀 풀이](#풀이1-재귀-풀이)
      - [파이썬. 재귀 제한](#파이썬-재귀-제한)
    - [풀이2. 반복 풀이](#풀이2-반복-풀이)
    - [풀이3. 이진 검색 모듈](#풀이3-이진-검색-모듈)
      - [bisect 모듈](#bisect-모듈)
    - [풀이4. 이진 검색을 사용하지 않는 index 풀이](#풀이4-이진-검색을-사용하지-않는-index-풀이)
    - [참고. 이진 검색 알고리즘 버그](#참고-이진-검색-알고리즘-버그)
---
* [References](#references)

<br><br><br>


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
`mid` 값을 (`last`+`start`)//2 로 두니 의도한 대로 mid 값이 부여되었고, 정답도 잘 출력되었다.<br>
그러나 아래의 풀이를 보니 `target` 값이 `nums`에 없는 경우를 고려하여 `return -1`을 했으면 더 좋았을 것 같다.

<br><br>

## 문제 65 이진 검색 풀이
### 풀이1. 재귀 풀이
먼저, 간단히 재귀로 이진 검색을 구현할 수 있다.<br>
절반씩 범위를 줄여나가며 맞출 때까지 계속 재귀 호출하면 된다.<br>
앞서 카드 마술 설명과 동일한 정석대로의 풀이로, 전체 코드는 다음과 같다.
```python
def search(self, nums: List[int], target: int) -> int:
    def binary_search(left, rlght):
        if left <= rlght:
            mid = (left + right) // 2
            
            if nums[mid] < target:
                return binary_search(mid + 1, right)
            elif nums[mid] > target:
                return binary_search(left, mid - 1)
            else:
                return mid
        else:
            return -1
            
    return binary_search(0, len(nums) - 1)
```

<br><br>

### 파이썬. 재귀 제한
파이썬에는 재귀 호출에 대한 호출 횟수 제한이 있으며 기본 값은 1,000으로 설정되어 있다.
```python
>>> sys.getrecursionlimit()
l000
```
당연히 이 값은 변경할 수 있으나 일반적으로 코딩 테스트 플랫폼에서는 sys 모듈을 이용한 설정 변경을 허용하지 않기 때문에 가급적 모든 재귀 풀이는 1,000회 반복 이내에 풀이가 가능하도록 구현해야 한다.<br>
앞서 풀이의 경우 별도로 횟수 제한을 하지 않았으나 2^1000은 무려 1071508607186267320948425049060001810561404811705533607 44375038837035105112493612249319837881569585812759467291755314682518714528569231404359845775746985748039345674824230985421074605062371141895418215304647498358194126739876755916554394600629145711964686542167660429831652624386837205668069376이기 때문에 이보다 더 큰 테스트 케이스로 인해 에러를 발생하는 일은 아마 없을 것이다. <br>
하지만 파이썬은 재귀 횟수의 제한이 있기 때문에 이 부분은 항상 유의해야 한다.

<br><br>

### 풀이2. 반복 풀이
이제 이 문제를 반복 풀이로 바꿔보자.<br>
대부분의 재귀 풀이는 반복 풀이로 변경할 수 있다.<br>
대개는 재귀 풀이가 더 우아한 편이지만, 반복 풀이는 좀 더 직관적이라 이해가 쉽다는 장점이 있다. <br>
전체 코드는 다음과 같다.
```python
def search(self, nums: List[int], target: int) -> int:
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] < target:
            left = mid + 1
        elif nums[mid] > target:
            right = mid - 1
        else:
            return mid
    return -1
```

<br><br>

### 풀이3. 이진 검색 모듈
앞서 풀이1과 풀이2 에서 이진 검색을 직접 구현했지만 사실 파이썬에서는 이진 검색을 직접 구현할 필요가 없다.<br>
**이진 검색 알고리즘을 지원하는 bisect 모듈**을 기본으로 제공하기 때문이다. <br>
여러 가지 예외 처리를 포함한 이진 검색 알고리즘이 깔끔하게 모듈 형태로 구현되어 있으므로, 이 모률을 이용하면 이진 검색을 파이썬다운 방식으로 다음과 같이 좀 더 간단히 풀이할 수 있다.
```python
def search(self, nums: List[int], target: int) -> int:
    index = bisect.bisect_left(nums, target)
    
    if index < len(nums) and nums[index] == target:
        return index
    else:
        return -1
```

<br><br>

### bisect 모듈
* `bisect_left(a, x)`<br><br>
정렬된 순서를 유지하면서 리스트 a에 데이터 x 삽입할 가장 왼쪽 인덱스 찾기

* `bisect_right(a, x)`<br><br>
정렬된 순서를 유지하면서 리스트 a에 데이터 x 삽입할 가장 오른쪽 인덱스 찾기

→ 값이 특정 범위에 속하는 원소의 개수

<br>

```python
from bisect import bisect_right, bisect_left

a = [1,2,3,4,4,8]
x = 4

print(bisect_left(a,x))
print(bisect_right(a,x))
```
```
3
5
```

<br><br>

### 풀이4. 이진 검색을 사용하지 않는 index 풀이
앞서 1번부터 3번 풀이까지는 이진 검색을 사용하여 풀었지만, 이번에는 이진 검색을 사용하지 않고 한번 시도해보자.<br>
파이썬에서 제공하는, 해당 값의 인덱스를 찾아내는 index() 메소드를 활용하는 방법이다.<br>
이 경우 존재하지 않는 값이라면 에러가 발생하므로, 에러인 ValueError를 예외 처리하여 -1을 리턴하도록 처리하면 풀이가 가능하다.
```python
def search(self, nums: List[int], target: int) -> int:
    try:
        return nums.index(target)
    except ValueError:
        return -1
```

<br><br>

### 참고. 이진 검색 알고리즘 버그
사실 이진 검색 알고리즘에는 오랫동안 아무도 발견하지 못한 버그가 있었다.<br>
이 오류는 무엇이었을까? 앞서 65번 문제의 풀이에서, 중앙의 위치를 구하는 풀이1 코드의 4행과 풀이2 코드의 4행을 자세히 살펴보자.<br>
코드는 다음과 같다.
```python
mid = (left + right) // 2
```
left와 right를 더하고 결과를 반으로 나눠 가운데를 계산한다.<br>
수학적으로 잘못된 점은 전혀 없다.<br>
그러나 컴퓨터과학에서는 문제를 일으킬 만한 소지가 있는 코드다. <br>
두 수를 더하면 항상 각각의 수보다 큰 수가 된다.<br>
그러나 자료형에는 최댓값이 있다.

<img src="https://user-images.githubusercontent.com/55045377/128662175-8ac8a766-8e0e-405c-9520-5ac11719ed99.png" width=60% height=60%>

만약 그림 18-2에서와 같이 left + right가 자료형의 최댓값을 넘어선다면, 좀 더 구체적으로 int 형일 때 2^31 - 1을 넘어선다면, C에서는 예상치 못한 결과가 나오게 되고, 자바에서는 오류가 발생한다. <br>
결과가 int 자료형이 허용하는 최댓값을 초과하므로 오버플로(Overflow)가 발생하기 때문이다.

그렇다면 이진 검색은 어떻게 구현하면 될까?

<img src="https://user-images.githubusercontent.com/55045377/128662578-fafa54a0-ed2e-4d06-b0f4-af948ede5a4a.png" width=60% height=60%>

그림 18-3에서 left + (right - left) / 2를 계산하면 오버플로를 피하면서 정확한 mid 값을 구할 수 있다.<br>
두 수를 더하고 그 합을 반으로 나누는 대신, 두 수의 뺄셈을 해서 그 차를 반으로 나눈 후 낮은 수에 더한다. <br>
(right - left)는 항상 right보다 작은 값이 나오므로 오버플로의 위험이 없다. <br>
(right - left) / 2에 left를 더한 값은 right를 넘지 않으므로 이 역시 오버플로의 위험이 없다.

여기서는 나눗셈 결과가 실수형일 경우, 정수형으로 내림하는 부분을 포함해 계산하는 코드를 다음과 같이 정리할 수 있다.
```python
mid = left + (right - left) // 2
```

65번 문제 풀이1의 4행을 수정한 전체 코드는 다음과 같다.
```python
def search(self, nums: List[int], target: int) -> int:
    def binary_search(left, rlght):
        if left <= rlght:
            # 자료형을 초과하지 않는 중앙 위치 계산
            mid = left + (right - left) // 2
            
            if nums[mid] < target:
                return binary_search(mid + 1, right)
            elif nums[mid] > target:
                return binary_search(left, mid - 1)
            else:
                return mid
        else:
            return -1
            
    return binary_search(0, len(nums) - 1)
```

<br><br>



















---
# References
* https://velog.io/@woo0_hooo/python-Bisect-%ED%95%A8%EC%88%98%EB%9E%80

<br><br><br>

