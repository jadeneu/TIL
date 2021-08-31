# 목차
* [람다(lambda)](#람다lambda)
  + [람다함수의 형식](#람다함수의-형식)
  + [람다함수 응용법](#람다함수-응용법)
    - [1. 할당하지 않고 쓰기](#1-할당하지-않고-쓰기)
    - [2. 인자 두 개 쓰기](#2-인자-두-개-쓰기)
    - [3. if 사용하기](#3-if-사용하기)
    - [4. 리스트를 정렬할 때 key로 사용(sort, sorted)](#4-리스트를-정렬할-때-key로-사용sort-sorted)
    - [5. 람다 표현식을 인수로 활용(map, filter, reduce)](#5-람다-표현식을-인수로-활용map-filter-reduce)
---
* [References](#references)

<br><br><br>


# 람다(lambda)
> 람다(lambda)는 익명함수를 지칭하는 용어이다. 즉 람다함수를 통해 이름 없는 함수를 만들 수 있다.

람다함수는 기존의 함수(명 등)을 선언하고 사용하던 방식과는 달리 **바로 정의하여 사용할 수 있는 함수**이다.<br>
-> 귀찮은 과정을 생략하고 쉽게 임시로 생성하여 쓰고 버리기 용이한 함수!

<br>

또한, 람다함수는 `return` 키워드 없이 자동으로 `return`해준다.

<img src="https://user-images.githubusercontent.com/55045377/131331740-2821e930-cf48-4642-a6a2-d9152e16133b.png" width=70% height=70%>

<br><br>

## 람다함수의 형식
```python
lambda 인자 : 표현식
```

<br>

* **Ex)**<br>
```python
add_num = lambda x: x+1
```

<br><br>

## 람다함수 응용법
### 1. 할당하지 않고 쓰기
람다 표현식을 괄호로 묶은 뒤에 다시 괄호를 붙이고 인수를 넣어 호출한다.
```python
>>>(lambda x: x + 10)(1)
11
```

### 2. 인자 두 개 쓰기
```python
>>> (lambda x,y: x + y)(10, 20)
30
```

### 3. if 사용하기
```python
check_pass = lambda x: 'pass' if x>=70 else 'fail'
```

<br>

### 4. 리스트를 정렬할 때 key로 사용(sort, sorted)
#### 1.
```python
a = [(1, 2), (5, 1), (0, 1), (5, 2), (3, 0)]

# key 인자를 정하지 않은 기본적인 sort에선, 튜플 순서대로 우선순위 기본 할당.
b = sorted(a)
# b = [(0, 1), (1, 2), (3, 0), (5, 1), (5, 2)]

# key 인자에 함수를 넘겨주면 우선순위가 정해짐.
c = sorted(a, key = lambda x : x[0])
# c = [(0, 1), (1, 2), (3, 0), (5, 1), (5, 2)]
d = sorted(a, key = lambda x : x[1])
# d = [(3, 0), (5, 1), (0, 1), (1, 2), (5, 2)]

# 비교할 아이템이 요소가 복수 개일 경우, 튜플로 우선순위를 정해줄 수 있다.
# -를 붙이면, 현재와 반대차순으로 정렬된다.
e = sorted(a, key = lambda x : (x[0], -x[1]))
# e = [(0, 1), (1, 2), (3, 0), (5, 2), (5, 1)]
f = sorted(a, key = lambda x : -x[0])
# f = [(5, 1), (5, 2), (3, 0), (1, 2), (0, 1)]
```
* **tip**<br>
`key`로 우선순위를 정하고 정렬할 때, 겹치는 수가 있으면 처음 선언된 순서대로 출력된다. (`d` 출력 참고)<br>
만약 두번째 원소 순으로 정렬하고, 요소가 같을 땐 첫번째 원소 순으로 정렬하고 싶으면<br>
  ```python
  key = lambda x : (x[1], x[0])
  ```
  이렇게 하면 된다.
  
#### 2.
```python
s = ['2 A', '1 B', '4 C', '1 A']

>>>sorted(s)
['1 A', '1 B', '2 A', '4 C']

# 하지만 나는 뒤에 문자 순 정렬을 원한다!
>>> s.sort(key=lambda x: (x.split[1], x.split[0]))
['1 A', '2 A', '1 B', '4 C']
```

<br>

### 5. 람다 표현식을 인수로 활용(map, filter, reduce)
#### 1. map()과 함께
```python
>>> list(map(lambda x: x+10, [1,2,3]))
[11, 12, 13]
```
* **map**<br>
map은 리스트의 요소를 지정된 함수로 처리해주는 함수이다(map은 원본 리스트를 변경하지 않고 새 리스트를 생성함).<br>
  * list(map(함수, 리스트))
  * tuple(map(함수, 튜플))
  * 예시
    ```python
    >>> a = [1.2, 2.5, 3.7, 4.6]
    >>> a = list(map(int, a))
    >>> a
    [1, 2, 3, 4]
    ```
 
  출처: https://dojang.io/mod/page/view.php?id=2286
  
<br>

#### 2. filter()와 함께
filter()는 boolean 값을 리턴하는 함수를 받아 조건에 맞으면 데이터를 반환하고 그렇지 않으면 반환하지 않는다.
```python
>>> a = [8, 4, 2, 5, 2, 7, 9, 11, 26, 13] 
>>> result = list(filter(lambda x : x > 7 and x < 15, a)) 
[8, 9, 11, 13]
```
* **filter**<br>
filter는 특정 조건으로 걸러서 걸러진 요소들로 iterator 객체를 만들어서 리턴해준다.
  * filter(적용시킬 함수, 적용할 요소들)
  * 예시
    ```python
    target = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

    def is_even(n):
        return True if n % 2 == 0 else False

    result = filter(is_even, target)

    print(list(result))
    # [2, 4, 6, 8, 10]
    ```
    * 람다 사용
    ```python
    target = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    result = filter(lambda x : x%2==0, target)
    print(list(result))
    # [2, 4, 6, 8, 10]
    ```

  출처: https://wikidocs.net/22803
  
<br>

#### 3. reduce()와 함께
```python
# reduce는 functools 모듈을 불러와야 사용가능.
>>> from functools import reduce t = [47, 11, 42, 13] 
>>> result = reduce(lambda x, y : x + y, t)
113
```
* **reduce**<br>
reduce는 여러 개의 데이터를 대상으로 주로 누적 집계를 내기 위해서 사용한다.
  * reduce(함수, 순회 가능한 데이터[, 초기값])
  * 함수와 순회 가능한 데이터는 반드시 전달되어야 하고, 초기값은 선택이다.
  * 예시
    ```python
    >>> def sum(x, y): 
    ... return x+y 
    ... 
    >>> reduce(sum, [1, 2, 3, 4, 5])
    15
    ```
    * 람다 사용
    ```python
    >>> reduce(lambda x, y: x+y, [1, 2, 3, 4, 5])
    15
    ```
    * 초기값 사용
    ```python
    >>> reduce(lambda x, y: x+y, [1, 2, 3, 4, 5], 10)
    25
    ```
    
  출처: https://www.daleseo.com/python-functools-reduce/,<br>
  https://codepractice.tistory.com/86

<br><br>



















---
# References
* https://velog.io/@oaoong/Python-lambda-%ED%91%9C%ED%98%84%EC%8B%9D
* https://wikidocs.net/22804
* https://wikidocs.net/64

<br><br><br>
