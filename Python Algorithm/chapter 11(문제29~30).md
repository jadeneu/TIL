# 목차


# 11장 해시 테이블
## 리트코드
### 문제 29 보석과 돌
> 298p

* **내가 짠 코드**<br>
```python
import collections

def Jewels(j,s):
  s_dict = collections.Counter(s)
  cnt = 0 # 보석 개수를 세는 변수

  for jewel in j:
    cnt += s_dict[jewel]

  return cnt


J = "aA" # 보석
S = "aAAbbbb" # 갖고 있는 돌
print(Jewels(J,S))
```
<br><br>

### 문제 29 보석과 돌 풀이
#### 풀이1. 해시 테이블을 이용한 풀이
이 문제는 갖고 있는 돌 S의 각각의 개수를 모두 헤아린 다음, J의 각 요소를 키로 하는 각 개수를 합산하면 풀이할 수 있다.<br>
따라서 해시 테이블로 풀이할 수 있는 전형적인 문제다
```python
freqs = {}

for char in S:
    if char not in freqs:
        freqs[char] = 1
    else:
        freqs[char] += 1
```
먼저 freqs라는 해시 테이블을 선언한다. 그리고 돌의 모음인 S를 문자 단위로 하나씩 분리해 반복한다.<br>
만약 처음 등장한 문자라면 1을 선언하고, 기존에 있던 문자라면 1을 더한다.<br>
입력값 S = "aAAbbbb"는 해시 테이블 freqs에 다음과 같이 저장된다.
```
{
    'a': 1,
    'A': 2,
    'b': 4
}
```
각 문자별 빈도 수가 저장된다. 이제 이 중에서 보석을 나타내는 J의 문자를 다음과 같이 꺼내어 해당 문자의 빈도 수를 합하면 최종 결과가 된다.
```python
count = 0
for char in J:
    if char in freqs:
        count += freqsp[char]
```
이 문제에서 보석 J의 입력값은 J = "aA"이므로, freq['a']의 값 1과 freq['A']의 값 2를 더하면 최종 결과는 3이 될 것이다.

전체 코드는 다음과 같다.
```python
def numJewelsInStones(self, J: str, S: str) -> int:
    freqs = {}
    count = 0
    
    # 돌(S)의 빈도 수 계산
    for char in S:
        if char not in freqs:
            freqs[char] = 1
        else:
            freqs[char] += 1
            
    # 보석(J)의 빈도 수 합산
    for char in J:
        if char in freqs:
            count += freqs[char]
            
    return count
```
해시 테이블을 응용하는 기본적인 문제로 어렵지 않게 풀이할 수 있다.
<br><br>

#### 풀이2. defaultdict를 이용한 비교 생략
```python
def numJewelsInStones(self, J: str, S: str) -> int:
    freqs = collections.defaultdict(int)
    count = 0
    
    # 비교 없이 돌(S) 빈도 수 계산
    for char in S:
        freqs[char] += 1
        
    # 비교 없이 보석(J) 빈도 수 합산
    for char in J:
        count += freqs[char]
      
    return count
```
키가 존재하는지 여부를 매번 판별할 필요가 없기 때문에 이처럼 바로 계산할 수 있고, 코드 수도 많이 줄어들어 깔끔해졌다. 실행 속도 또한 이전과 동일하다
<br><br>

#### 풀이3. Counter로 계산 생략
Counter를 사용하면 코드를 더욱 짧게 줄일 수 있다. 각 개수를 계산하는 부분까지 자동으로 처리할 수 있기 때문이다.
```python
def numJewelsInStones(self, J: str, S: str) -> int:
    freqs = collections.Counter(S) # 돌(S) 빈도 수 계산
    count = 0
    
    # 비교 없이 보석(J) 빈도 수 합산
    for char in J:
        count += freqs[char]
        
    return count
```
아울러 Counter는 존재하지 않는 키의 경우 KeyError를 발생하는 게 아니라 0을 출력해주기 때문에, defaultdict와 마찬가지로 에러에 대한 예외 처리를 할 필요가 없다. 단순히 J에 포함된 문자의 개수를 계산하기만 하면 된다. 실행 속도 또한 동일하다.
<br><br>

#### 풀이4. 파이썬다운 방식
해시 테이블과는 관련이 없지만, 이 문제는 파이썬다운 방식으로 단 한 줄로 계산할 수 있다.
```python
def numJewelsInStones(self, J: str, S: str) -> int:
    return sum(s in J for s in S)
```
코드 줄 수는 단 한 줄이다. 정말 간단하게 풀이가 가능하다.<br>
이 코드가 어떻게 동작하는지 잘 이해되지 않는다면 리스트 컴프리헨션을 출력해보면 좀 더 직관적으로 이해할 수 있다.
```python
>>> [s for s in S]
['a', 'A', 'A', 'b', 'b', 'b', 'b']
```
이렇게 하면 각 문자를 하나씩 출력하는 리스트 컴프리헨션이다.
```python
>>> [s in J for s in S]
[True, True, True, False, False, False, False]
```
여기에 약간의 비교 구문을 추가해 S의 문자 s가 J에 포함되어 있는지 여부를 출력할 수 있다.<br>
['a', 'A', 'A', 'b', 'b', 'b', 'b']에서 첫 번째와 두 번째, 세 번째에 각각 J의 값 a와 A가 포함되어 있으므로 True, 나머지는 모두 False다.<br>
이제 이 값을 다음과 같이 sum()으로 합산한다.
```python
>>> sum([s in J for s in S])
3
```
결과는 True의 개수로, 3이 된다.<br>
여기서 리스트 컴프리헨션을 의미하는 앞뒤의 대괄호 []는 다음과 같이 제거할 수 있다.
```python
>>> sum(s in J for s in S)
3
```
<br><br>

### 문제 30 중복 문자 없는 가장 긴 부분 문자열
> 303p

* **내가 짠 코드**<br>
푸는 방법이 떠오르지 않아 풀이를 보고 이해했다.
<br><br>

### 문제 30 중복 문자 없는 가장 긴 부분 문자열 풀이
#### 풀이1. 슬라이딩 윈도우와 투 포인터로 사이즈 조절
입력값이 s ="abcabcbb"인 경우를 살펴보자.<br>
슬라이딩 윈도우(20장 참조)로 한 칸씩 우측으로 이동하면서 윈도우 내에 모든 문자가 중복이 없도록 투 포인터로 윈도우 사이즈를 조절하면서 풀이해보면 결과는 그림 11-10(304p)과 같다.
> 그림 11-10

이 그림에는 슬라이딩 윈도우의 각 단계별로 중복 문자가 없는 윈도우의 최대 길이를 순서대로 표시했다.<br>
이 경우 세 번째부터 길이가 3으로 최대 길이가 되며, 이 길이는 여섯 번째까지 변경 없이 계속 유지된다. <br>
정답은 3이다. 이제 정답을 찾는 과정을 코드로 한번 구현해보자.

먼저, 투 포인터로 문제를 풀이하되, 포인터 2개 모두 왼쪽에서 출발한다.<br>
각각 왼쪽 시작점에서 출발해 두 번째 포인터(우측 포인터로 여기서는 index 변수)는 다음과 같이 계속 오른쪽으로 확장한다.
```python
for index, char in enumerate(s):
    if char in used:
        start = used[char] + 1
    else:
        ...
```
여기서 만약 이미 등장한 문자라면 used에 있을 것이고 이 경우 첫 번째 포인터인 start를 used[char] + 1 위치로 갱신한다.<br>
등장하지 않았던, 처음 보는 문자라면 다음과 같이 처리한다.
```python
for index, char in enumerate(s):
    if char in used:
        ...
    else:
        max_length = max(max_length, index - start + 1)
```
처음 보는 문자인 경우, 매번 max()로 부분 문자열의 길이를 확인하면서 더 큰 값인 경우 갱신한다.<br>
그림 11-10(304p)처럼 세 번째 위치부터는 계속 최댓값인 3을 유지하게 된다.<br>
또한 다음과 같이 현재 문자의 위치는 계속 갱신해준다.
```python
for index, char in enumerate(s):
    if char in used:
        ...
    else:
        ...
        
    used[char] = index
```
이제 used[char]는 현재 문자를 키로 하는 해시 테이블이며, 여기에는 현재 위치를 값으로 삽입한다.<br>
즉 앞서 살펴본 start = used[char] + 1는 '현재 위치 +1'이 되고 이미 등장했던 문자인 경우 왼쪽 포인터인 start를 현재 위치까지 옮기게 된다.<br>
그런데 이미 등장했다고 무작정 옮겨 버리면 곤란하다. 현재 슬라이딩 윈도우의 바깥에 있는 문자는 예전에 등장한 적이 있더라도 지금은 무시해야 하기 때문이다.<br>
따라서 비교 구문에 다음과 같이 and 이후에 조건 start <= used[char]를 추가한다.
```python

```













































