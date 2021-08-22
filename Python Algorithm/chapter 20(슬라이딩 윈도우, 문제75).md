# 목차

# 20장 슬라이딩 윈도우
> **슬라이딩 윈도우(Sliding Window)** 란 고정 사이즈의 윈도우가 이동하면서 윈도우 내에 있는 데이터를 이용해 문제를 풀이하는 알고리즘을 말한다.

**슬라이딩 윈도우**는 투 포인터와 함께 알고리즘 문제 풀이에 매우 유용하게 사용되는 중요한 기법이다. <br>
투 포인터와 비슷하지만 이와 구분하기 위해 일반적으로 **고정 사이즈 윈도우를 사용하는 경우를 슬라이딩 윈도우**로 따로 구분하기도 한다. <br>
또한 주로 정렬된 배열을 대상으로 하는 투 포인터와 달리 **슬라이딩 윈도우는 정렬 여부에 관계 없이 활용된다**는 차이가 있다.

<img src="https://user-images.githubusercontent.com/55045377/130307020-99a622e8-a7e7-4b10-bf83-6494a43afaa8.png" width=50% height=50%>

그림 20-1에서, 좌측 그림의 투 포인터는 주로 정렬된 배열을 대상으로 한다.<br>
[1, 2, 3, 4, 5]가 모두 순서대로 정렬되어 있으며, 처음에는 2개의 포인터가 1과 5를 가리키고 있으나 그다음에는 왼쪽 포인터만 2를 가리키도록 이동했고, 또 그다음에는 2개의 포인터가 모두 움직여 3, 4를 가리킨다.

이처럼 윈도우의 사이즈가 가변적이며 좌우 포인터가 자유롭게 이동할 수 있는 방식이 투 포인터인 반면, 슬라이딩 윈도우는 [1, 3, 4, 2, 5] 와 같이 정렬되어 있지 않은 배열에도 적용할 수 있다.<br>
윈도우 사이즈는 고정이며 좌 또는 우 한쪽 방향으로만 이동한다.<br>
그러나 교과서에 정의된 내용이 아니기 때문에 실무에서 주로 이런 형태로 쓰일 뿐, 서로 혼용되어 쓰이기도 한다.

투 포인터와 슬라이딩 윈도우의 특징을 표 20-1에 정리했다.

<img src="https://user-images.githubusercontent.com/55045377/130307164-c9bf6e46-fdcb-4c07-a22c-15f67f12beb5.png" width=60% height=60%>

<br>

* **슬라이딩 윈도우 명칭의 유래**<br>
슬라이딩 윈도우는 2개의 네트워크 호스트 간의 패킷 흐름을 제어하기 위한 방법을 지칭하는 네트워크 용어이기도 하다. <br>
네트워크에서 패킷을 전송할 때 고정 사이즈의 윈도우가 옆으로 이동하면서 그다음 패킷들을 전송하는 방식을 말하는데, 지금까지 설명한 슬라이딩 윈도우 알고리즘의 행동 패턴과 동일하기 때문에 슬라이딩 윈도우의 명칭은 여기서 유래한 것으로 추측할 수 있다.

<br>

그렇다면 이제 슬라이딩 윈도우를 이용해 알고리즘 문제를 풀이해보자. <br>
교과서에 명확하게 정의되어 있는 내용이 아닌 만큼 때로는 투 포인터와 서로 혼용되어 풀이하는 경우가 있지만, 둘 사이를 명확하게 구분하는 게 큰 의미는 없기 때문에 특별히 문제는 없다.<br>
여기서도 자유롭게 혼용해 풀이해보겠다.

<br><br>

## 문제 75 최대 슬라이딩 윈도우
> 571p

* 배열 nums가주어졌을 때 k 크기의 슬라이딩 윈도우를 오른쪽 끝까지 이동하면서 최대 슬라이딩 윈도우를 구하라.
* 입력
  ```
  nums = [1,3,-1,-3,5,3,6,7], k = 3
  ```
* 출력
  ```
  [3,3,5,5,6,7]
  ```
* 설명<br>
  ```
   윈도우 포지션                 최대
  ---------------               ------
  [1  3  -1] -3  5  3  6  7       3
   1 [3  -1  -3] 5  3  6  7       3
   1  3 [-1  -3  5] 3  6  7       5
   1  3  -1 [-3  5  3] 6  7       5
   1  3  -1  -3 [5  3  6] 7       6
   1  3  -1  -3  5 [3  6  7]      7
   ```

<br><br>

### 문제 75 내가 구현한 코드
```python
def solution(nums, k):
  # 예외 처리
  if not nums:                    # 1
    return []

  left, right = 0, k-1            # 2
  answer = []

  while right < len(nums):
    tmp = nums[left:right+1]      # 3
    answer.append(max(tmp))       # 4
    left += 1                     # 5
    right += 1

  return answer


nums = [1,3,-1,-3,5,3,6,7]
k = 3
print(solution(nums, k))
```
* **# 1**: `nums`가 빈 리스트인 경우 `return []`을 한다.
* **# 2**: `left`와 `right`를 인덱스 `0`과 `k-1`로 설정한다.
* **# 3**: `tmp`는 `nums`를 `k` 크기만큼 슬라이싱한 리스트이다.
* **# 4**: `nums`를 슬라이싱한 `tmp` 요소 중 가장 큰 요소를 `answer`에 append 한다.
* **# 5**: `left`와 `right`를 한 칸 이동시킨다.

<br><br>

## 문제 75 최대 슬라이딩 윈도우 풀이
### 풀이1. 브루트 포스로 계산
제목부터 '슬라이딩 윈도우'라는 단어가 포함된 전형적인 슬라이딩 윈도우 문제로, 문제를 잘 읽어보면 파이썬에서는 슬라이싱과 내장 함수를 사용해 매우 쉬운 방식으로 풀이할 수 있을 것 같다.
```python
r = []
for i in range(len(nums) - k + 1):
    r.append(max(nums[i:i + k]))
```
이 코드는 정확히 문제에서 요구하는 대로, 슬라이딩 윈도우를 우측으로 움직여 가며 max()로 최댓값을 추출한다.<br>
매번 윈도우의 최댓값을 계산하기 때문에 이 경우 시간 복잡도는 O(n)이다. 이 풀이는 **704밀리초**가 소요된다.
```python
from typing import List


class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums:
            return nums

        r = []
        for i in range(len(nums) - k + 1):
            r.append(max(nums[i:i + k]))

        return r
```

<br><br>

### 풀이2. 큐를 이용한 최적화
좀 더 최적화할 수 있는 방법이 있을 것 같다. 어차피 슬라이딩 윈도우를 한 칸씩 움직여야 하는 부분은 개선이 어렵다.<br>
그렇다면 **max()를 계산하는 부분에서 최적화**를 할 수 있지 않을까?<br>
정렬되지 않은 슬라이딩 윈도우에서 최댓값을 추출하려면 어떠한 알고리즘이든 결국 한 번 이상은 봐야 하기 때문에, 최댓값 계산을 O(n) 이내로 줄일 수 있는 방법이 없다.

따라서 가급적 최댓값 계산을 최소화하기 위해 이전의 최댓값을 저장해뒀다가 한 칸씩 이동할 때 새 값에 대해서만 더 큰 값인지 확인하고, 최댓값이 윈도우에서 빠지게 되는 경우에만 다시 전체를 계산하는 형태로 개선한다면, 계산량을 획기적으로 줄일 수 있을 것 같다.

선입선출(FIFO, First-In First-Out) 형태로 풀이할 수 있기 때문에, 이에 해당하는 대표적인 자료형인 큐(Queue)(9장 참고)를 사용하고 처음에는 다음과 같이 구현한다.

```python
current_max = float('-inf')

for i, v in enumerate(nums):
    window.append(v)
    if i < k - 1:
        continue
    ...
```
이 코드에서는 k 만큼, 이후 비즈니스 로직은 상관하지 않고 일단 값을 계속 채워 넣는다.

참고로 파이썬에서는 큐 사용이 필요할 경우, 실제로는 기능이 많고 좀 더 성능이 좋은 데크를 주로 사용한다.<br>
다음과 같이 collections 모듈을 이용해 선언할 수 있다.
```python
window = collections.deque()
```

<br>

아직 최댓값이 반영된 상태가 아니라면, 현재 윈도우 전체의 최댓값을 계산해야 한다.<br>
이미 최댓값이 존재한다면 새로 추가된 값이 기존 최댓값보다 더 큰 경우에만 최댓값을 교체한다.<br>
바로 이 부분이 성능 개선을 위한 핵심이다. 매번 최댓값을 계산할 필요가 없기 때문이다.

이 로직을 코드로 구현해보면 다음과 같다.
```python
if current_max == float('-inf'):
    current_max = max(window)
elif v > current_max:
    current_max = v
```
이처럼 새로 추가된 값이 기존 최댓값보다 더 큰 경우에만 최댓값을 교체한다.

이제 다음과 같이 최댓값을 결과에 추가한다.
```python
results.append(current_max)
```

<br>

슬라이딩 윈도우는 오른쪽으로 점차 이동한다.,<br>
이동하면서 시작하자마자 다시 신규 요소가 추가될 것이므로 가장 오래된 값은 마지막에 제거한다.<br>
이때 만약 그 값이 현재 윈도우의 최댓값이라면, 기존의 최댓값은 더 이상 윈도우에 포함되지 않으므로 다음과 같이 처리한다.
```python
if current_max == window.popleft():
    current_max = float('-inf')
```

* **참고**<br>
최댓값에 시스템이 지정할 수 있는 가장 낮은 값을 지정하여 초기화한다. 이렇게 하면 이후에 다시 최댓값을 계산하게 할 수 있다.

<br>

전체 코드는 다음과 같다.
```python
import collections
from typing import List


class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        results = []
        window = collections.deque()
        current_max = float('-inf')
        for i, v in enumerate(nums):
            window.append(v)
            if i < k - 1:
                continue

            # 새로 추가된 값이 기존 최대값보다 큰 경우 교체
            if current_max == float('-inf'):
                current_max = max(window)
            elif v > current_max:
                current_max = v

            results.append(current_max)

            # 최대값이 윈도우에서 빠지면 초기화
            if current_max == window.popleft():
                current_max = float('-inf')
        return results
```
필요할 때만 전체의 최댓값을 계산하고 이외에는 새로 추가되는 값이 최대인지만을 확인하는 형태로 계산량을 획기적으로 줄였다.<br>
실행 속도는 **156밀리초**로 브루트 포스로 모두 계산하는 것에 비해 약 5배가량 더 빠른 속도에 실행되도록 개선할 수 있었다.

<br><br><br>


























