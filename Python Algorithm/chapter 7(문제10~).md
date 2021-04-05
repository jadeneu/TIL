# 7장
## 리트코드 문제
### 문제 10 배열 파티션 I
> 190p


* **내가 짠 코드**<br>
```python
def array_partition(nums:list) -> int:
  nums.sort()
  cnt = 0 # min(a,b)의 합

  for i in range(1,len(nums)+1,2):
    if i+1 <= len(nums):
      cnt += min(nums[-i],nums[-(i+1)])

  return cnt


nums = list(map(int,input().split()))
print(array_partition(nums))
```
<br><br>

### 문제 10 배열 파티션 I 풀이
#### 풀이1. 오름차순 풀이
문제가 상당히 특이하고 바로 이해하기가 쉽지 않다.<br>
그러나 찬찬히 살펴보면 페어의 min()을 합산했을 때 최대를 만드는 것은 결국 min()이 되도록 커야 한다는 뜻이고, 뒤에서부터 내림차순으로 집어넣으면 항상 최대 min() 페어를 유지할 수 있다.<br>
이 문제에서 배열 입력값은 항상 2n개일 것이므로 앞에서부터 오름차순으로 집어넣어도 결과는 같을 것이다.

뒤에서부터 내림차순으로 만들어도 결과는 동일하며, 여기서는 오름차순으로 구현해본다.<br>
전체 코드는 다음과 같다.
```python
def arrayPairSum(self, nums: List[int]) -> int:
  sum = 0
  pair = []
  nums.sort()
  
  for n in nums:
  # 앞에서부터 오름차순으로 페어를 만들어서 합 계산
  pair.append(n)
  if len(pair) == 2:
    sum += min(pair)
    pair = []
    
  return sum
```
<br><br>

#### 풀이2. 짝수 번째 값 계산
이렇게 페어에 대해 일일이 min() 값을 구하지 않아도 짝수 번째 값(0부터 시작하므로)을 더하면 될 것 같다. 정렬된 상태에서는 짝수 번째에 항상 작은 값이 위치하기 때문이다.<br>
불필요한 리스트 변수를 생략할 수 있기 때문에, 전체 코드 또한 많이 줄어들어 매우 간결하게 구현할 수 있다.
```python
def arrayPairSum(self, nums: List[int]) -> int:
  sum = 0
  nums.sort()
  
  for i, n in enumerate(nums):
    # 짝수 번째 값의 합 계산
    if i % 2 == 0:
      sum += n
      
  return sum
```
<br><br>

#### 풀이3. 파이썬다운 방식
이 코드를 좀 더 정리할 수 있을까? 앞서 살펴본 슬라이싱을 활용하면 놀랍게도 단 한 줄로도 풀이가 가능하다.<br>
그야말로 파이썬다운 방식이다.
```python
def arrayPairSum(self, nums: List[int]) -> int:
  return sum(sorted(nums)[::2])
```
슬라이싱 구문 [::2]는 2칸씩 건너뛰므로 짝수 번째를 계산하는 것과 동일하다.

참고 | 문자열 슬라이싱 144p
<br><br>

#### 정리
풀이3 파이썬다운 방식이 가장 코드가 짧으면서도 슬라이싱을 사용한 덕분에 성능 또한 가장 좋다.<br>
풀이2 짝수 번째 계산도 매번 min()을 계산하지 않고 단순히 해당 인덱스를 찾기만 하면 되므로 풀이1 오름파순 풀이에 비해 좀 더 성능이 좋은 편이다.
<br><br>

---

### 문제 11 자신을 제외한 배열의 곱
>193p

* **내가 짠 코드**<br>
생각 안 나서 풀이 봄
<br><br>

### 문제 11 자신을 제외한 배열의 곱 풀이
#### 풀이1. 왼쪽 곱셈 결과에 오른쪽 값을 차례대로 곱셈
이 문제에는 중요한 제약사항이 있다. '나눗셈을 하지 않고 O(n)에 풀이하라'는 점인데 이 말은 당장 머릿속에 떠오르는 풀이법인, 미리 전체 곱셈 값을 구해놓고 각 항목별로 자기 자신을 나눠서 풀이하는 방법은 안 된다는 뜻이기도 하다.<br>
그렇다면 풀이 방법은 한 가지 뿐이다. 자기 자신을 제외하고 왼쪽의 곱셈 결과와 오른쪽의 곱셈 결과를 곱해야 한다.

먼저 왼쪽부터 곱해서 result에 추가한다. 곱셈 결과는 그대로 out 리스트 변수에 담기게 되며, 마지막 값 곱셈을 제외하여 결과는 [1, 1, 2, 6]이 된다.<br>
코드로 구현하면 다음과 같다.
```python
p = 1
for i in range(0, len(nums)):
  out.append(p)
  p = p * nums[i]
```
이처럼 변수 i가 오른쪽으로 이동하면서 해당 인덱스의 값을 곱해 나간 다음, 오른쪽에서 곱해서 넣는다.<br>
여기서 만약 별도의 리스트 변수를 만들고 그 변수에 우측 곱셈 결과를 넣으면, 공간 복잡도는 O(n)이 된다.<br>
그러나 기존 out 변수를 재활용한다면 공간 복잡도 O(1)에 풀이가 가능하다.<br>
참고로 출력에 필요한 공간은 공간 복잡도에 포함하지 않는다.
<br><br>

```python
p = 1
for i in range(len(nums) - 1, 0 - 1, -1):
  out[i] = out[i] * p
  p = p * nums[i]
```
이번에는 왼쪽의 곱셈 결과에 오른쪽 마지막 값부터 차례대로 곱해 나간다.<br>
for 문의 range(x, y, z)에서 세 번째 파라미터인 z는 증분을 지정하는 파라미터며, 여기서는 -1이므로 1씩 줄어드는 형태가 된다.<br>
또한 여기서 p는 1부터 차례대로 점점 커지면서 4, 12, 24가 되고, 최종적으로 이 값이 왼쪽 곱셈 결과에 곱해져 out 변수에는 정답인 [24, 12, 8, 6]을 담게 된다.<br>
그림 7-11(193p)에 이 과정이 잘 나와 있으며, 전체 코드는 다음과 같다.
```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
  out = []
  p = 1
  # 왼쪽 곱셈
  for i in range(0, len(nums)):
    out.append(p)
    p = p * nums[i]
  p = 1
  # 왼쪽 곱셈 결과에 오른쪽 값을 차례대로 곱셈
  for i in range(len(nums) - 1, 0 - 1, -1):
    out[i] = out[i] * p
    p = p * nums[i]
  return out
```

---

### 문제 12 주식을 사고팔기 가장 좋은 시점
> 195p








