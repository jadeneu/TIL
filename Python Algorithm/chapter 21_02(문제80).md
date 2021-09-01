# 목차

# 21장 그리디 알고리즘
## 문제 80 태스크 스케줄러
> 595p

* A에서 Z로 표현된 태스크가 있다. 각 간격마다 CPU는 한 번의 태스크만 실행할 수 있고, n번의 간격 내에는 동일한 태스크를 실행할 수 없다. 더 이상 태스크를 실행할 수 없는 경우 아이들(idle) 상태가 된다. 모든 태스크를 실행하기 위한 최소 간격을 출력하라.
* 입력
```
tasks = ["A", "A", "A", "B", "B", "B"], n = 2
```
* 출력
```
8
```
* 설명<br>
A -> B -> idle -> A -> B -> idle -> A -> B

<br><br>

---
쉽게 말하면 같은 task가 실행될 때 n만큼 쉬어줘야 한다. 쉬는 동안에는 다른 작업을 수행하거나 idle을 수행할 수 있다.

문제의 핵심은 가장 많이 나온 task가 가장 많은 idle 을 만들 수 있다는 것이다.<br>
따라서 가장 많이 나온 task로 idle 수를 구한 다음 남은 task 들을 idle에 최대한 채워넣는걸 목표로 한다.

* **Ex)**<br>
tasks = { A, A, A, B, B }, n = 2 일 때를 생각해보자.<br>
cooling time인 n은 동일한 task가 가장 많은 수와 관련이 있다.
```
A _ _ A _ _ A
```
위의 경우 A가 3개, B가 2개 이므로 A만 채웠을 때 idle(빈 칸)이 4가 될 것이다.<br>
B는 idle들 사이에 넣어주면 된다.
```
A B _ A B _ A
```
따라서 정답은 7이 된다.

여기서 B를 넣기 전(가장 많이 나온 task로 idle을 구할 때) idle의 총 수를 구할 수 있다.
```
idle 수 = (가장 많이 나온 task 수 - 1) x n
```
`task 수 - 1`을 한 이유는 idle은 task들 사이에만 나오기 때문이다.<br>
이 idle에 남은 task들을 집어넣는다고 생각하면 된다. 집어넣으면서 남은 idle 수를 갱신한 뒤,<br>
최종 정답은 **task들의 수 + 남은 idle 수**가 된다.

이제 남은 task들을 idle에 집어넣으면서 idle 수를 갱신하는 걸 보자.
* **Ex)**<br>
tasks = { A, A, A, B, B, B }, n = 2 일 때
```
A B _ A B _ A B
```
idle 빈칸 생성은 같지만 B가 넣어질 때도 쿨타임은 유지해야 하므로 마지막 B는 idle에 넣을 수 없다.<br>
따라서 task i(i는 알파벳 중 하나)를 idle에 끼워넣는다고 할 때 감소되는 idle 수는 **min(i task 수, 가장 많이 나온 task 수 -1)** 과 같다.<br>
위의 예시에서 i를 B라 하면 B task를 idle에 넣는다면 3(B 개수), 2(가장 많이 나온 task(A)의 수 - 1)의 최소값인 2가 감소돼서 idle 수는 4에서 2가 된다.<br>
모든 task 알파벳을 탐색하며 남은 idle의 수를 갱신한 뒤 **tasks 수(알파벳 수 아님) + idle 수**가 정답이 된다.

 
<br><br>

### 문제 80 내가 구현한 코드
```python

```

<br><br>

## 문제 80 태스크 스케줄러 풀이
### 풀이1. 우선순위 큐 이용
이 문제 또한 마찬가지로 우선순위 큐를 이용해 그리디하게 풀 수 있는 문제다.<br>
그런데 아이템을 추출한 이후에는 매번 아이템 개수를 업데이트해줘야 한다.<br>
파이썬의 heapq만으로 구현하기에는 상당히 번거로운 작업들이 필요하다. <br>
따라서 여기서는 가능한 한 파이썬답게 간결한 방식으로 풀어보기 위해 Counter 모듈을 사용해 코드를 구현해보자.

우선, 이 풀이의 전체 코드는 다음과 같다.
```python
import collections
from typing import List


class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        counter = collections.Counter(tasks)
        result = 0

        while True:
            sub_count = 0
            # 개수 순 추출
            for task, _ in counter.most_common(n + 1):
                sub_count += 1
                result += 1

                counter.subtract(task)
                # 0 이하인 아이템을 목록에서 완전히 제거
                counter += collections.Counter()

            if not counter:
                break

            result += n - sub_count + 1

        return result
```
* **subtract**<br>
subtract()는 각 요소의 값을 각각 빼준다.<br>
```python
>>> a = Counter(a=5, b=3)
>>> b = Counter(b=5, c=2)
>>> a.subtract(b)
>>> a
Counter({'a': 5, 'b': -2, 'c': -2})
```
출처: https://dongdongfather.tistory.com/70

<br>

이 문제의 전체 코드는 간단하지만, 사실 여기에는 몇 가지 트릭이 있으며, 직관적으로 알아내기 어려운 부분들이 숨어 있다. 

우선 하나씩 살펴보자. 먼저, 우선순위 큐를 사용해 가장 개수가 많은 아이템부터 하나씩 추출해야 하는데, 문제는 전체를 추출하는 게 아니라 하나만 추출하고 빠진 개수를 업데이트할 수 있는 구조가 필요하다는 점이다.

만약 heapq를 사용한다면 다음과 같은 형태가 될 것이다.
```python
for task, count in collections.Counter(tasks).items():
    heapq.heappush(heap, (-count, task))
    ...
    count, task = heapq.heappop(heap)
    ...
    heapq.heappush(heap, (-count + 1, task))
```
각 태스크의 개수를 Counter로 계산하고 이 값을 힙에 추가한다.<br>
heapq는 최소 힙(Min Heap)만을 지원하기 때문에 최대 힙 효과를 내기 위해 음수로 변환하여 저장한다.<br>
heappop()은 항목 전체가 추출되기 때문에 꺼내서 활용한 이후에는 heappush()로 개수를 줄여(여기서는 음수이기 때문에 +1 을 하여) 다시 추가하는 작업이 필요하다.<br>
이처럼 번거로운 작업들이 필요한데, Counter만으로도 이 같은 일을 간결하게 처리할 수 있다. 파이썬의 Counter 모듈은 매우 막강한 기능들을 지원하기 때문이다.

다음과 같이 간단하게 처리가 가능하다.
```python
counter = collections.Counter(tasks)
for task, _ in counter.most_common():
    counter.subtract(task)
```

* **코드 들여다 보기**<br>
```python
- counter -> Counter({'A': 3, 'B': 3})

- counter.most_common() -> [('A', 3), ('B', 3)]

- task -> key 값 (A,B,..)

- for문 후 counter -> Counter({'A': 2, 'B': 2})
```

<br>

이전의 전체 코드와 같은 역할을 하는 코드인데, 훨씬 더 간결하다.<br>
most_common()은 가장 개수가 많은 아이템부터 출력하는 함수이며, 사실상 최대 힙과 같은 역할을 한다. <br>
그러나 pop()으로 추출되는 것은 아니기 때문에 subtract(task)를 지정해 1개씩 개수를 별도로 줄여나간다. <br>
이처럼 Counter 모듈은 개수를 줄이는 메소드도 지원하기 때문에 매우 편리하다. 

<br><br>

그런데 Counter는 0과 음수도 처리하는 특징이 있다. <br>
원래는 편리한 특징이지만 사실 우리에게는 0 이하는 필요가 없기 때문에, 매번 0 이하인지 체크하거나, 0 이하일 때는 아예 삭제하는 기능이 필요하다. 

다음 코드가 그런 역할을 한다.
```python
counter.subtract(task)
counter += collections.Counter()
```
빈 collection.Counter()를 더하는 것인데, 이렇게 할 경우 0 이하인 아이템을 목록에서 아예 제거해버린다. 매우 유용한 핵(Hack)이다.<br>
물론 실무에서라면 이렇게 코드만 작성해두면 무슨 역할을 하는지 아무도 모를 것이다. <br>
따라서 다음 "# 1"과 같이 간단한 주석을 붙여두는 편이 좋을 것 같다.
```python
# 0 이하인 아이템을 목록에서 완전히 제거  "# 1"
counter += collections.Counter()
```

<br>

또 다른 트릭은 n과 관련된 내용이다. <br>
만약 다음의 입력값을 most_common(n)으로 추출하고, 뒤에 idle을 덧붙이는 형태로 실행한다고 가정해보자.
```python
tasks = ["A", "A", "A", "B", "C", "D"], n = 2
```
이 입력값의 경우 실행 결과는 다음과 같을 것이다.
```
A -> B -> idle -> A -> C -> idle -> A -> D
```
결과는 8 이다. 정답일까? 그렇지 않다. 왜냐면 다음 순서대로 실행할 경우 7 로, 한 번 더 줄일 수 있기 때문이다.
```
A -> B -> idle -> A -> C -> D -> A
```
이 경우 마지막에는 순서가 다르게 나와야 하는데, 앞 부분과 달리 마지막에만 순서가 다르게 나오게 하는 일은 별도의 예외 처리가 필요하다.<br>
이 같은 처리를 구현하는 일은 생각보다 쉽지 않다. 별도 처리가 필요 없이 한 번에 구현하는 방법은 없을까?

n 이 아닌 n + 1만큼을 추출해보자.<br>
즉 most_common(n + 1) 을 추출하고 n + 1개가 추출될 때는 idle 없이 실행한다. 

결과는 다음과 같다.
```
A -> B -> C -> A -> D -> idle -> A
```
입력값이 n = 2 였기 때문에 n + 1을 추출했을 때 3개가 모두 나온다면 idle 없이 계속 진행한다. <br>
그다음에는 A -> D 2개만 추출됐기 때문에 한 번 idle하고 마지막으로 A를 출력한다.<br>
앞서와 순서는 달라졌지만 정답은 7로 동일하다. <br>
이렇게 하면 별도의 예외 처리 없이도 쉽게 계산이 가능하다. n + 1 부분이 핵심이다.

이제 595페이지로 돌아가 다시 한번 전체 코드를 살펴보자. 설명을 꼼꼼히 읽었다면 이제 전체 코드의 모든 부분들이 이해가 될 것이다.

Counter 모듈을 무리하게 사용한 탓에 속도가 높은 편은 아니지만 문제없이 잘 풀린다.






























---
# References
* https://m.blog.naver.com/hyj5419/221962380297
* https://withhamit.tistory.com/419

<br><br><br>
