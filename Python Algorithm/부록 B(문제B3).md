# 목차
* [부록 B 카카오 공채 문제 풀이](#부록-b-카카오-공채-문제-풀이)
  + [문제 B3 캐시](#문제-b3-캐시)
  + [문제 B3 캐시 풀이](#문제-b3-캐시-풀이)
    - [데크를 이용한 LRU 구현](#데크를-이용한-lru-구현)
    - [✅LRU란?](#lru란)
    - [✅데크의 maxlen](#데크의-maxlen)

---
* [References](#references)

<br><br><br>



# 부록 B 카카오 공채 문제 풀이
## 문제 B3 캐시
> 688p

* 지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다. <br>
이 프로그램의 테스팅 업무를 담당하는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.<br>
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.<br><br>
어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

<br>

* **입력 형식**
  * 캐시 크기(cacheSize)와 도시이름 배열(cities)을 입력받는다.
  * cacheSize는 정수이며, 범위는 0 <= cacheSize <= 30이다.
  * cities는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
  * 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

<br>

* **출력 형식**
  * 입력된 도시이름 배열을 순서대로 처리할 때 "총 실행시간"을 출력한다.

<br>

* **조건**
  * 캐시 교체 알고리즘은 LRU(Least Recently Used)를 사용한다.
  * cache hit일 경우 실행시간은 1 이다.
  * cache miss일 경우 실행시간은 5 이다.

<br>

* **입출력 예제**

<img src="https://user-images.githubusercontent.com/55045377/139656297-861ce7a7-04ff-4724-b18f-3e7cb36b334d.png" width=70%>

<br><br>

## 문제 B3 캐시 풀이
### 데크를 이용한 LRU 구현
### ✅LRU란?
* **LRU**<br>
LRU 알고리즘은 **Least Recently Used**의 약자로 직역하자면 가장 최근에 사용되지 않은 것 정도의 의미를 가지고 있다.<br><br>
캐시에서 메모리를 다루기 위해 사용되는 알고리즘이며,<br>
캐시가 사용하는 리소스의 양은 제한되어 있고<br>
캐시는 제한된 리소스 내에서 데이터를 빠르게 저장하고 접근할 수 있어야 한다.<br><br>
이를 위해 LRU 알고리즘은 메모리 상에서 **가장 최근에 사용된 적이 없는 캐시의 메모리부터 대체**하며 새로운 데이터로 갱신시켜준다.

<br>

* **LRU 알고리즘 그림 설명**<br><br>
**Input: 123145<br>
Output: 5413**

<img src="https://user-images.githubusercontent.com/55045377/139869843-3319dac5-18e9-45b4-8547-ec7c21ce1a1a.jpg" width=60%>


* 4초 : 1은 재참조된 것이므로, 가장 오랫동안 참조되지 않은 순으로 저장된 순서를 변경한다.
* 6초 : cache size가 가득차 5가 들어갈 수 없으므로, 가장 오랫동안 참조되지 않은(Least Recently Used) 2를 제거한 후 저장한다.

<br>

---

<br>

이제 본격적인 풀이에 들어가보자.

<br>

LRU는 캐시 교체 전략 중 하나로, 가장 오래전에 사용된 아이템을 버리는 방식이다. 이 문제에서는 한 가지 주의해야 할 점이 있는데, 입력값에 0이 포함되어 있다는 점이다. <br>
이 경우 예외 처리를 하지 않고 LRU 알고리즘을 구현한다면, 입출력이 달라질 수 있으므로 주의가 필요하다. <br>

또한 LRU를 바닥부터 구현할 수도 있지만 길이가 제한된 자료형이 있다면 구현하기가 한결 쉬울 것 같다.<br>
파이썬에 그런 자료형이 있는데, 바로 데크(Deque)다.
```python
cache = collections.deque(maxlen=cacheSize)
```
데크는 크기를 지정할 수 있는 maxlen 파라미터를 지원하며, 이 경우 최대 크기를 초과할 때 가장 오래된 항목부터 제거된다.

<br>

### ✅데크의 maxlen
maxlen이 지정되지 않거나 None이면, 데크는 임의의 길이로 커질 수 있다. 그렇지 않으면, 데크는 지정된 최대 길이로 제한된다.<br>
일단 제한된 길이의 데크가 가득 차면, 새 항목이 추가될 때 해당하는 수의 항목이 반대쪽 끝에서 삭제된다.<br><br>
  * **예시**<br>
  ```python
  from collections import deque

  deq = deque(maxlen=3)

  for i in range(5):
      deq.append(i)
      print(deq)
  ```
  ```
  deque([0], maxlen=3)
  deque([0, 1], maxlen=3)
  deque([0, 1, 2], maxlen=3)
  deque([1, 2, 3], maxlen=3)
  deque([2, 3, 4], maxlen=3)
  ```

<br>

---

<br>

```python
>>> cache = collections.deque(maxlen=3)
>>> cache.append(l)
>>> cache.append(2)
>>> cache.append(3)
>>> cache
deque([l, 2, 3])
>>> cache.append(4)
>>> cache
deque([2, 3, 4])
```
따라서, LRU를 구현하기 위해서는 가장 최근에 액세스된 아이템을 재삽입해주기만 하면 나머지는 알아서 처리된다.

전체 코드는 다음과 같다.
```python
import collections
from typing import List


def solution(cacheSize: int, cities: List[str]) -> int:
    elapsed: int = 0
    cache = collections.deque(maxlen=cacheSize)

    for c in cities:
        c = c.lower()
        # 캐시 히트 시 재삽입 처리
        if c in cache:
            cache.remove(c)
            cache.append(c)
            elapsed += 1
        else:  # 캐시 미스 시 삽입만
            cache.append(c)
            elapsed += 5
    return elapsed
```


<br><br>

* **실제 돌려볼 수 있는 코드**
```python
import collections
from typing import List


def solution(cacheSize: int, cities: List[str]) -> int:
    elapsed: int = 0
    cache = collections.deque(maxlen=cacheSize)

    for c in cities:
        c = c.lower()
        # 캐시 히트 시 재삽입 처리
        if c in cache:
            cache.remove(c)
            cache.append(c)
            elapsed += 1
        else:  # 캐시 미스 시 삽입만
            cache.append(c)
            elapsed += 5
    return elapsed
    
print(solution(3, ["Jeju","Pangyo","Seoul","Jeju","Pangyo","Seoul","Jeju","Pangyo","Seoul"]))
#print(solution(3, ["Jeju","Pangyo","Seoul","NewYork","LA","Jeju","Pangyo","Seoul","NewYork","LA"]))
```


<br><br>












# References
* https://gomguard.tistory.com/115
* https://j2wooooo.tistory.com/121
* https://docs.python.org/ko/3/library/collections.html
* https://codetorial.net/tips_and_examples/collections_deque.html

<br><br><br>
