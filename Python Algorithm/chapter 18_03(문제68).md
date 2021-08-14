# 목차

# 18장 이진 검색
## 문제 68 두 수의 합 II
> 532p

* 정렬된 배열을 받아 덧셈하여 타겟을 만들 수 있는 배열의 두 숫자 인덱스를 리턴하라.<br>
※ 주의: 이 문제에서 배열은 0이(Zero-Based) 아닌 1부터 시작하는 것으로 한다.

* 입력
```
numbers = [2,7,11,15], target = 9
```
* 출력
```
[1,2]
```

<br><br>

### 문제 68 내가 짠 코드
```python
import bisect

def solution(numbers, target):
  idx = bisect.bisect_left(numbers, target)  # idx = 2

  for i in range(idx):
    compare = target - numbers[i]
    start, last = i + 1, idx - 1
    while start <= last:
      mid = start + (last-start) // 2
      if numbers[mid] > compare:
        last = mid - 1
      elif numbers[mid] < compare:
        start = mid + 1
      else:
        return [i + 1, mid + 1]

numbers = [2,7,11,15]
target = 9
print(solution(numbers, target))
```
* 먼저 `bisect` 모듈을 사용하여 `target`이 `numbers`에 들어갈 수 있는 왼쪽 기준의 인덱스(`idx`)를 구한다.
* `numbers`의 0 인덱스부터 `idx - 1` 인덱스까지 for문을 돌며, 인덱스 순서대로 `target`에서 `numbers` 값을 빼서 `compare` 값을 만든 후 탐색을 한다. 
* 문제에서 리스트가 1부터 인덱스를 시작한다고 했으므로 `[i + 1, mid + 1]`을 return 해준다.

<br><br>

## 문제 68 두 수의 합 II 풀이
### 풀이1. 투 포인터
앞서 7장에서 7번 '두 수의 합' 문제는 투 포인터로 풀 수 없다(풀이5)고 했다.<br>
그러나 그 문제의 다른 버전 격인 이 문제는 입력 배열이 정렬되어 있다는 가정이 추가됐다.<br>
따라서 이 문제는 투 포인터로도 잘 풀릴 것이다.<br>
그렇다면 앞서 7번 문제에서는 풀이에 실패했던, 투 포인터 코드(풀이5)의 일부를 가져와보자.
```python
left, right = 0, len(nums) - 1
while not left == right:
    if nums[left] + nums[right] < target:
        left += 1
    elif nums[left] + nums[right] > target:
        right -= 1
    else:
        return left, right  # 1
```
이 문제의 경우 특별히 0이 아닌 1부터 시작한다고 했으니, 리턴하는 "# 1" 부분의 값에 각각 +1 을 하고, 문제에서 입력값에 해당하는 변수명이 기존 nums에서 numbers로 바뀌었으니 이 부분만 수정해주면 문제 없이 잘 풀릴 것 같다.

전체 코드는 다음과 같다.
```python
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        while not left == right:
            if numbers[left] + numbers[right] < target:
                left += 1
            elif numbers[left] + numbers[right] > target:
                right -= 1
            else:
                return left + 1, right + 1  # 리턴 값 +1
```
투 포인터 풀이의 경우 **O(n)** 에 풀이할 수 있다. 이 풀이는 68밀리초가 걸렸다.

<br><br>

### 풀이2. 이진 검색
이번에는 이 장의 주제인 이진 검색으로 풀이해보자.<br>
현재 값을 기준으로 나머지 값이 맞는지 확인하는 형태의 이진 검색 풀이를 시도해볼 수 있을 것 같다. 

전체 코드는 다음과 같다.
```python
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            left, right = k + 1, len(numbers) - 1
            expected = target - v
            # 이진 검색으로 나머지 값 판별
            while left <= right:
                mid = left + (right - left) // 2
                if numbers[mid] < expected:
                    left = mid + 1
                elif numbers[mid] > expected:
                    right = mid - 1
                else:
                    return k + 1, mid + 1
```
이 경우 이진 검색 log n을 n번 반복하므로 시간 복잡도는 **O(n log n)** 이 된다.<br>
이 2가지 방식만 놓고 봤을 때는, 투포인터가 O(n)으로 이진 검색 풀이 O(n log n)에 비해 더 빠르게 실행된다.<br>
이진 검색 풀이의 실행 속도는 112밀리초로 2배가량 늦다. <br>
그렇다면 이진 검색에 bisect 모듈을 사용해 좀 더 속도를 높일 수 있을까?

<br><br>

### 풀이3. bisect 모듈, 슬라이싱
bisect 모듈을 사용해 이진 검색을 구현해보자.<br>
이렇게 하면 이진 검색 알고리즘을 직접 구현할 필요가 없기 때문에, 코드도 한결 깔끔해질 것이다.

전체 코드는 다음과 같다.
```python
import bisect
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            expected = target - v
            i = bisect.bisect_left(numbers[k + 1:], expected)
            if i < len(numbers[k + 1:]) and numbers[i + k + 1] == expected:
                return k + 1, i + k + 2
```
left와 right 변수도 필요 없고, 예상대로 코드는 엄청나게 깔끔해졌다. <br>
그런데 문제가 있다. 이 풀이의 실행 속도는 무려 **2184밀리초**에 다다른다. 2초 이상 소요되는 셈이다.
앞서 이진 검색 풀이가 112밀리초 정도였으니, 20배 이상 느려졌다. 왜 그럴까?<br>
성능 개선을 시도해보자.

<br><br>

### 풀이4. bisect 모듈 + 슬라이싱 최소화
아무래도 파이썬 슬라이싱을 무리하게 적용한 게 원인인 듯 하여 nums 변수에 한 번만 사용해 담아두는 형태로 다음과 같이 개선을 시도해봤다.
```python
import bisect
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            expected = target - v
            nums = numbers[k + 1:]
            i = bisect.bisect_left(nums, expected)
            if i < len(nums) and numbers[i + k + 1] == expected:
                return k + 1, i + k + 2
```
이렇게 하니 **1136밀리초**에 풀려, 앞서 풀이3에 비해 2배가량 속도가 빨라졌다. <br>
그래도 풀이1 투 포인터 풀이에 비해서는 많이 느리다. 좀 더 개선할 방법이 필요하다.

<br><br>

### 풀이5. bisect 모듈 + 슬라이싱 제거
bisect 모듈의 기능을 좀 더 활용하기로 하고, 이진 검색 시 사용하는 bisect_left() 메소드의 공식 문서를 살펴보니 다음과 같이 기본 파라미터 외에도 여러 추가 파라미터가 있는 것을 발견했다.
```python
bisect.bisect_left(a, x, lo=0, hi=len(a))
```
이 문서를 참고해서 왼쪽 범위를 제한하는 파라미터인 lo를 찾아냈고, 이 값을 지정하는 것으로 풀이를 진행했다.<br>
* **NOTE: `lo`를 쓰면 왼쪽 범위만 제한하며, 리스트를 슬라이싱 하진 않음**

이제 더 이상 슬라이싱을 사용할 필요가 없어졌다.
```python
import bisect
from typing import List


class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for k, v in enumerate(numbers):
            expected = target - v
            i = bisect.bisect_left(numbers, expected, k + 1)
            if i < len(numbers) and numbers[i] == expected:
                return k + 1, i + 1
```
이 경우 **68밀리초**로 드디어 투 포인터 풀이와 속도가 같아졌다. <br>
슬라이싱을 사용하지 않고도 bisect 모듈만을 이용해 이진 검색의 성능을 개선하는 데 비로소 성공했다.

<br>

* **NOTE: 슬라이싱은 편리하고 빠른 모듈이지만, 이처럼 생각 없이 무분별하게 남용하다 보면 속도 저하의 주범이 될 수 있다. <br>
경우에 따라서는, 꼭 필요한 곳에만 적절히 사용해야 실행 속도를 좀 더 최적화할 수 있다.**

<br><br>

### 파이썬. 슬라이싱 성능
파이씬의 슬라이싱은 매우 효율적이고 빠르다고 했는데 왜 이런 일이 발생할까? <br>
그 이유는 **슬라이싱은 매번 새롭게 객체를 생성해서 할당하게 되고,** 엄청나게 큰 배열의 경우 슬라이싱으로 **새로운 객체를 생성하는 데 상당한 시간이 들기 때문이다.** <br>
마찬가지로 배열의 길이를 계산하는 데도 매번 길이를 계산하는 비용이 들기 때문에 여기에 상당한 시간이 소요된다. 

슬라이싱을 하지 않을 경우 배열의 길이는 별도 속성으로 미리 계산해서 갖고 있는 값이므로 매번 계산할 필요 없이 해당 속성을 리턴하기만 하면 되지만, 슬라이싱은 매번 새롭게 배열의 길이를 계산해야 한다. 새로운 객체이기 때문이다.

다음과 같이 1억 개 요소를 지닌 리스트를 생성했을 때 다른 리스트 변수로 할당하는 작업은 금방 실행이 된다.
```python
>>> a = [x for x in range(100000000)]
>>> id(a)
4376898632
>>> b = a
>>> id(b)
4376898632
```
여기서 b 변수는 a 변수 객체를 참조만 하면 되기 때문이다. 둘은 동일한 ID를 갖고 있다.

<br>

```python
>>> c = a[:]
>>> id(c)
4377566600
```
그러나 c 변수는 다르다. 슬라이싱으로 할당했고, 이 경우 객체 참조가 아닌 요소 전체, 즉 1억 개를 모두 복사하는 과정을 거친다. <br>
이로 인해 상당한 시간이 소요되며, 복사로 인해 ID 또한 달라지는 것을 확인할 수 있다.<br>
그렇다면 앞서 68번 문제 풀이5에서 길이를 비교하는 부분인 5행에 [:]를 이용해 일부러 슬라이싱을 시도해보자.
```python
def twoSum(self, numbers: List[int], target: int) -> List[int]:
    for k, v in enumerate(numbers):
        ...
        if i < len(numbers[:]) and numbers[i] == expected: # 슬라이싱 시도
            ...
```
이 경우 또한 **1136 밀리초**로, 1초 이상 소요된다.<br>
이처럼 매우 큰 배열의 경우 슬라이싱을 하게 되면 전체들 복사하는 과정이나 배열의 길이를 계산하는 부분에서 상당한 시간이 소요되므로 주의가 필요하다.

<br><br>

## 문제 69 2D 행렬 검색 II
> 537p

* mXn 행렬에서 값을 찾아내는 효율적인 알고리즘을 구현하라. 행렬은 왼쪽에서 오른쪽, 위에서 아래 오름차순으로 정렬되어 있다.
* 예제<br>
행렬은 다음과 같다.
```
[
  [1, 4, 7, 11, 15],
  [2, 5, 8, 12, 19],
  [3, 6, 9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
target=5일 경우, 값이 존재하므로 true를 리턴한다. target=20일 경우, 값이 존재하지 않으므로 false를 리턴한다.

<br><br>

### 문제 69 내가 짠 코드
```python
from bisect import bisect_left

def solution(map, target):
  m = len(map)
  n = len(map[0])

  for i in range(m):  # 1
    idx = bisect_left(map[i],target)  # 2
    if idx < n and  map[i][idx] == target:  # 3
      return True
  return False

map = [
       [1, 4, 7, 11, 15],
        [2, 5, 8, 12, 19],
        [3, 6, 9, 16, 22],
        [10, 13, 14, 17, 24],
        [18, 21, 23, 26, 30]
  ]
target = 5
print(solution(map, target))
```
* **# 1**: 행 `m`만큼 for문을 돌면서 `map` 안에 `target`값이 있는지 탐색한다.
* **# 2**: `bisect` 모듈을 사용하여 각 row에 `target`값이 들어갈 수 있는 인덱스 `idx`를 얻는다. 
* **# 3**: `target`이 `map[i]`의 마지막 원소보다 큰 값이면 인덱스를 벗어날 수 있으므로 `idx < n` 조건을 충족시켜야 한다.<br>
  또한 `map[i]`에 `target`값이 존재한다면 `idx` 인덱스에 위치해 있을 것이기 때문에 `map[i][idx] == target` 조건을 충족하는지 확인한다.
  
<br><br>

## 문제 69 2D 행렬 검색 II 풀이
### 풀이1. 첫 행의 맨 뒤에서 탐색
이 문제는 열(Column)을 기준으로 이진 검색을 수행한 다음, 찾아낸 값을 기준으로 해당 위치의 각 행(Row)을 기준으로 다시 이진 검색을 수행하면 해결할 수 있다.

그러나 첫 번째 이진 검색은 쉽게 할 수 있지만, 두 번째 이진 검색은 생각보다 효율적으로 풀이하기가 쉽지 않다.<br>
각 행에서 특정 인덱스를 기준으로 값을 추출해오는데 적지 않은 연산이 필요하기 때문이다.

































