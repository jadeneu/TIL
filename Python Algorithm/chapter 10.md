# 목차


# 10장 데크, 우선순위 큐
10장에서는 스택과 큐의 연산을 모두 갖고 있는 복합 자료형인 **데크**와 추출 순서가 일정하게 정해져 있지 않은 **우선순위 큐**에 대해 살펴보자.
<br><br>
## 데크
이전 장에서 원형 큐를 구현하면서 Front()와 Rear()를 모두 구현한 바 있다.

사실 Front()는 큐의 연산이 맞지만 Rear()는 큐에 정의되어 있지 않은 연산이라는 점도 얘기한 바 있다. 이 연산은 데크에 존재하는 연산으로, 데크의 정의는 다음과 같다.

> 데크(Deque)는 더블 엔디드 큐(Double-Ended Queue)의 줄임말로, 글자 그대로 양쪽 끝을 모두 추출할 수 있는, 큐를 일반화한 형태의 추상 자료형(ADT)이다.

<br>

데크는 양쪽에서 삭제와 삽입을 모두 처리할 수 있으며, 스택과 큐의 특징을 모두 갖고 있다.<br>
이 추상 자료형(ADT)의 구현은 배열이나 연결리스트 모두 가능하지만, 특별히 다음과 같이 이중 연결 리스트(Doubly Linked List)로 구현하는 편이 가장 잘 어울린다.

> 그림 10-1

이중 연결 리스트로 구현하게 되면, 그림 10-1(266p)처럼 양쪽으로 head와 tail이라는 이름의 두 포인터를 갖고 있다가 새로운 아이템이 추가될 때마다 앞쪽 또는 뒤쪽으로 연결시켜 주기만 하면 된다. 당연히 연결 후에는 포인터를 이동하면 된다.

파이썬은 데크 자료형을 다음과 같이 collections 모듈에서 deque라는 이름으로 지원한다.
```python
>>> import collections
>>> d = collections.deque()
>>> type(d)
<class 'collections.deque'>
```
이 collections.deque는 그림 10-1(266p)과 마찬가지로 이중 연결 리스트로 구현되어 있다.

CPython에서는 고정 길이 하위 배열(Subarray)을 지닌 이중 연결 리스트로 구현되어 있으며, 내부 구현을 살펴보면 마친가지로 다음과 같이 구조체인 dequeobject가 block노드의 이중 연결 리스트로 구현되어 있는 것을 확인할 수 있다.<br>
또한 dequeobject는 왼쪽, 오른쪽 인덱스 정보와 최대 길이 등 여러 가지 부가 정보를 함께 보관하고 있는 풍부한 구조체임을 확인할 수 있다.
```c
// cpython/Modules/_collectionsmodule.c
typedef struct BLOCK {
    struct BLOCK *leftlink;
    PyObject *data[BLOCKLEN];
    struct BLOCK *rightlink;
} block;

typedef struct {
    ...
    block *leftblock;
    block *rightblock;
    Py_ssize_t leftindex;
    Py_ssize_t rightindex;
    Py_ssize_t maxlen;
    ...
} dequeobject;
```
<br>

이번에는 이중 연결 리스트를 이용해 데크 자료형을 직접 구현하는 문제를 한번 풀이해보자.
<br><br>

## 리트코드
### 문제 26 원형 데크 디자인
* **내가 짠 코드**<br>



























