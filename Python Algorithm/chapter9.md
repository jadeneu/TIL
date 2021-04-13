# 9장 스택, 큐
스택(Stack)과 큐(Queue)는 프로그래밍이라는 개념이 탄생할 때부터 사용된 가장 고전적인 자료구조 중 하나로, 그중에서도 스택은 거의 모든 애플리케이션을 만들 때 사용되는 자료구조다.<br>
스택은 **LIFO(Last-In-First-Out)**, 큐는 **FIFO(First-In-First-Out)** 로 처리된다.<br>
스택과 큐는 다음과 같은 간단한 비유를 들면 쉽게 이해가 된다. 스택은 잔뜩 쌓아둔 접시를 떠올려보자. 마지막에 쌓은 접시가 맨 위에 놓일 것이라, 가장 마지막에 쌓은 접시를 제일 먼저 꺼내게 된다. 큐는 맛집에 입장하기 위해 줄을 서는 것을 떠올려보자. 가장 먼저 줄을 선 사람이 가장 먼저 입장한다.

파이썬은 스택 자료형을 별도로 제공하지는 않지만, **리스트가 사실상 스택의 모든 연산을 지원한다**. 큐 또한 마찬가지다. **리스트는 큐의 모든 연산을 지원한다**.<br>
다만 리스트는 동적 배열로 구현되어 있어 큐의 연산을 수행하기에는 효율적이지 않기 때문에, 큐를 위해서는 데크(Deque)라는 별도의 자료형을 사용해야 좋은 성능을 낼 수 있다.
<br><br>

## 스택
> 스택은 다음과 같은 2가지 주요 연산을 지원하는 요소의 컬렉션으로 사용되는 추상 자료형이다.
> * push(): 요소를 컬렉션에 추가한다.
> * pop(): 아직 제거되지 않은 가장 최근에 삽입된 요소를 제거한다.
<br>

스택은 콜 스택(Call Stack)이라 하여 컴퓨터 프로그램의 서브루틴에 대한 정보를 저장하는 자료구조에도 널리 활용된다. 컴파일러가 출력하는 에러도 스택처럼 맨 마지막 에러가 가장 먼저 출력되는 순서를 보인다.
<br>

* **서브루틴이란?**<br>
서브루틴은 반복되는 특정 기능을 모아 별도로 묶어 놓아 이름을 붙여 놓은 것으로 메인루틴을 보조하는 역할을 한다. 보통 언어에서는 함수나 메소드 등으로 불리며 사용된다. <br>
(반복되어 사용하는 것을 메모리에 한번 적재하여 여러번 사용할 수 있도록 하는 방법이다.)

이외에도 꽉 찬 스택에 요소를 삽입하고자 할 때 스택에 요소가 넘쳐서 에러가 발생하는 것을 스택 버퍼 오버플로(Stack Buffer Overflow)라고 부른다.

스택 추상 자료형(Abstract Data Type)(이하 ADT)을 연결 리스트로 구현한 것은 그림 9-1(242p)과 같다. 참고로 ADT는 자료형의 연산을 정의한 것으로 실제 구현 방법은 명시하지 않는다. <br>
이 그림에서 ADT는 스택의 연산을 지원하기 위해 1부터 5까지 각 요소가 접시 쌓듯 차곡차곡 놓이겠지만, 실제로 연결 리스트로 구현하게 된다면 물리 메모리 상에는 순서와 관계 없이 여기저기에 무작위로 배치되고 포인터로 가리키게 될 것이다.