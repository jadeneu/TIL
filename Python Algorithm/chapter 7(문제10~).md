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

### 문제 11 자신을 제외한 배열의 곱
>193p

* **내가 짠 코드**<br>










