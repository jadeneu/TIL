# 목차
* [아키텍쳐(Architecture)](#아키텍쳐architecture)
* [계층화 아키텍쳐(Layered Architecture)](#계층화-아키텍쳐layered-architecture)
  * [계층화 아키텍쳐의 구성요소](#계층화-아키텍쳐의-구성요소)
  * [클라이언트의 요청에 따른 흐름도](#클라이언트의-요청에-따른-흐름도)
---
* [출처](#출처)
<br><br><br>

# 아키텍쳐(Architecture)
> 시스템 목적을 달성하기 위해 시스템의 상호작용 등의 시스템 디자인에 대한 제약 및 설계이다. 시스템의 구조, 행위, 더 많은 뷰를 정의하는 개념적 모형으로, 시스템 목적을 달성하기 위해 시스템의 각 컴포넌트가 무엇이며 어떻게 상호작용하는지, 정보가 어떻게 교환되는지를 설명한다.

* 즉, 어떤 시스템을 만들기 위한 전체적인 흐름의 디자인이다.
* 개념적으로 시스템의 구성물들이 각각 어떤 역할 수행할지 정의함으로써 세분화 및 구조화를 목적을 이룬다.

<br><br><br>

# 계층화 아키텍쳐(Layered Architecture)
layered architecture란 말 그대로 계층이 나뉘어져 있는 아키텍쳐를 뜻한다. layered architecture의 주된 목표는 어플리케이션을 여러 개의 굵직한 횡단 관심사(cross-cutting concern)로 분리해, 각각의 layer는 하나의 관심사에만 집중할 수 있도록 하는 것이다. 

아키텍쳐의 컴포넌트들은 각각 어플리케이션의 특정한 역할을 수행하도록 가로로 나누어져 계층을 이룬다.<br>
가장 널리 알려진 아키텍쳐로 전통적인 IT workflow 와 조직 구성과 잘 맞아 떨어져서 많은 비즈니스에서 채택된다.<br>
주로 3가지 계층으로 이루어져 있다.
* 유저 + 브라우저와 상호작용하는 로직이 잇는 **Presentation Layer**
* 요청에 맞는 비즈니스 로직을 수행하는 **Business Layer**
* 데이터를 저장하고 관리하는 **persistence Layer**가 있다.

각각의 계층은 다른 계층과 상호작용하지만, 다른 계층에서 발생하는 로직에는 신경쓰지 않아도 된다.<br>
예를 들어, 데이터를 다루는 persistence layer는 그 데이터가 보여지는 presentation layer를 신경쓰지 않아도 되며, 오로지 자신의 역할에만 집중하면 된다.
<br><br>

## 계층화 아키텍쳐의 구성요소
```
<---------------------------------------- Domain Model ------------------------------------->

Presentation(View) <--> Controller <--> Business(Service) <--> Persistence(DAO) <--> Database

```

### 1. Presentation layer : View
> 표현과 이벤트 처리, 데이터 포맷 책임

사용자와의 최종 접점에 위치하여 사용자로부터 데이터를 입력 받거나, 요청된 데이터를 출력해 보이는 계층이다. 외부로부터의 어플리케이션에 대한 요구를 받아들이는 동시에 처리 결과를 사용자에게 보여주는 부분이며, 컨트롤러의 구성 요소와 상호작용한다.
<br><br>

### 2. Control layer : Controller
> 구성 요소간의 처리 흐름을 제어, 데이터 조작 의뢰, 데이터 변환 및 연산, Exception과 Error 처리

사용자로부터 요청을 받고 응답을 처리하는 계층이다. 하위 계층에서 발생하는 Exception이나 Error에 대한 처리를 맡으며, 최종 프레젠테션 계층에 표현 해야할 도메인 모델을 엮는 기능을 수행한다. 사용자로부터 전달 받은 데이터의 유효성 검증을 처리하고, 전체 시스템의 설정상태를 유지한다. 요청에 해당하는 비지니스 로직을 결정하는 역할을 수행한다.

사용자의 요청을 검증, 로직에 요청을 전달, 로직에서 반환된 응답을 적절한 뷰로 연결
<br><br>

### 3. Business layer : Service
> Control layer와 Persistence layer를 연결하는 역할, Transaction 처리

애플리케이션의 비지니스 로직 처리와 비지니스와 관련된 도메인 모델의 적합성을 검증하고, 트랜잭션을 처리한다. Control계층과 Persistence계층을 연결하는 역할로서 두 계층이 직접 통신하지 않게 하여 애플리케이션의 유연성을 증가시킨다.

* ***[Transaction](../DB/Transaction.md)***
<br><br>

### 4. Persistence layer : DAO
영구 데이터를 빼내어 객체화 시키며, 영구 저장소에 데이터를 저장, 수정, 삭제하는 계층이다. 데이터베이스나 파일에 접근하여 데이터를 CRUD하는 계층이다.

* ***DAO란 Data Access Object의 약어로, Database의 data에 access하는 트랜잭션 객체이다.***<br>
  출처 : https://genesis8.tistory.com/214
<br><br>

### 5. Domain Model Layer : VO, DTO
관계형 데이터베이스의 엔티티와 비슷한 개념을 가지는 것으로 데이터 객체를 말한다.

* ***VO는 Value Object(값 객체)의 약어로, 값 그 자체를 표현하는 객체이다. 서로 다른 이름을 가진 VO의 인스턴스가 모든 속성 값이 같다면 같은 객체이다.***<br>
  Ex) color라는 VO가 있고, 그 안에서 red의 RGB값은 (255,0,0)이다. color1이 (255,0,0)이고 color2가 (255,0,0) 속성을 가질 때 둘은 같은 객체로 판단한다.
* ***DTO는 Data Transfer Object(데이터 전송 객체)의 약어로, 계층(Layer) 간 데이터 교환을 위해 사용하는 객체다.***<br>
  출처 : https://www.youtube.com/watch?v=J_Dr6R0Ov8E
<br><br><br>

## 클라이언트의 요청에 따른 흐름도
<img src="https://user-images.githubusercontent.com/55045377/125733262-a3d968d3-8a03-40de-b9d6-5f54ab8b2c51.png" width=70% height=70%>

### 주의점
* 위의 그림에서 나오듯이 많은 계층을 통과하기 때문에 Performance에서 좋지 않을 수 있다.
* 한 번 적용되면 다른 패턴을 섞는다거나 조금 수정하는 유연함은 조금 떨어진다.
* 어플리케이션의 서비스가 커짐에 따라 유지하기 힘든 구조이다.
* 하지만, 각 계층의 역할이 명확하여 개발과 테스트가 편해진다.
<br><br>

위와 같이 계층을 분리하는 이유는 간단하다. 한 곳에서 위의 모든 작업이 한꺼번에 이루어진다면 코드의 복잡성 증가, 유지보수의 어려움과 유연성 부족, 중복 코드의 증가, 낮은 확장성 등의 문제가 발생하기 때문이다.
<br><br><br>


























---
# 출처
* **아키텍쳐(Architecture) [[아키텍쳐(Architecture)](#아키텍쳐architecture)]**
  * https://velog.io/@sj950902/%EA%B3%84%EC%B8%B5%ED%99%94-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90Layered-Architecture
* **계층화 아키텍쳐(Layered Architecture) [[계층화 아키텍쳐(Layered Architecture)](#계층화-아키텍쳐layered-architecture)]**
  * https://velog.io/@sj950902/%EA%B3%84%EC%B8%B5%ED%99%94-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90Layered-Architecture
  * https://riiidtechblog.medium.com/gradle%EA%B3%BC-%ED%95%A8%EA%BB%98%ED%95%98%EB%8A%94-backend-layered-architecture-97117b344ba8
  * **계층화 아키텍쳐의 구성요소 [[계층화 아키텍쳐의 구성요소](#계층화-아키텍쳐의-구성요소)]**
    * https://walbatrossw.github.io/etc/2018/02/26/etc-layered-architecture.html#4-persistence-layer--dao
  * **클라이언트의 요청에 따른 흐름도 [[클라이언트의 요청에 따른 흐름도](#클라이언트의-요청에-따른-흐름도)]**
    * https://velog.io/@sj950902/%EA%B3%84%EC%B8%B5%ED%99%94-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90Layered-Architecture

<br><br><br>
