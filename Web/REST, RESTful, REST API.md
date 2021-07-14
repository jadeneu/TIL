# 목차
* [REST](#rest)
  + [REST 구성 요소](#rest-구성-요소)
  + [REST의 특징](#rest의-특징)
  + [REST의 장단점](#rest의-장단점)
* [REST API](#rest-api)
  + [REST API 설계 규칙](#rest-api-설계-규칙)
  + [REST API 설계 예시](#rest-api-설계-예시)
* [RESTful](#restful)
---
* [출처](#출처)
<br><br><br>

# REST
<img src="https://user-images.githubusercontent.com/55045377/125571401-288f73fd-134a-4b5f-883b-80eda6d0d93f.png">

이미지 출처 : https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
<br><br>

**REST(Representational State Transfer)** 는 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다.

* 즉, 자원(resource)의 표현(representation)에 의한 상태 전달
  + 자원(resource)의 표현(representation)
    - 자원: 해당 소프트웨어가 관리하는 모든 것
    - -> Ex) 문서, 그림, 데이터, 해당 소프트웨어 자체 등
    - 자원의 표현: 그 자원을 표현하기 위한 이름
    - -> Ex) DB의 학생 정보가 자원일 때, ‘students’를 자원의 표현으로 정한다.
  + 상태(정보) 전달
    - 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달한다.
    - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
<br><br>

즉 REST란 
1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미한다.

* **CRUD Operation이란**<br>
  CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로 
REST에서의 CRUD Operation 동작 예시는 다음과 같다.<br>

  ```
  Create : 데이터 생성(POST)
  Read : 데이터 조회(GET)
  Update : 데이터 수정(PUT)
  Delete : 데이터 삭제(DELETE)
  ```
<br><br>

## REST 구성 요소
REST는 다음과 같은 3가지로 구성이 되어있다. 

1. **자원(Resource) : URI**
    * 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
    * 자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
    * Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
2. **행위(Verb) : HTTP Method**
    * HTTP 프로토콜의 Method를 사용한다.
    * HTTP 프로토콜은 **GET, POST, PUT, DELETE** 와 같은 메서드를 제공한다.
3. **표현(Representation of Resource)**
    * Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
    * REST에서 하나의 자원은 **JSON, XML, TEXT, RSS** 등 여러 형태의 Representation으로 나타내어질 수 있다.
    * JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
<br><br>

## REST의 특징
**1. Server-Client(서버-클라이언트 구조)**
  * 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다.
    * REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    * Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
  * 서로 간 의존성이 줄어든다.<br><br>

**2. Stateless(무상태)**
  * HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다.
  * Client의 context를 Server에 저장하지 않는다.
    * 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
  * Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
    * 각 API 서버는 Client의 요청만을 단순 처리한다.
    * 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
    * 물론 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.
    * Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.<br><br>
 
**3. Cacheable(캐시 처리 가능)**
  * 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
    * 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
    * HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
  * 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
  * 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.<br><br>

**4. Layered System(계층화)**
  * Client는 REST API Server만 호출한다.
  * REST Server는 다중 계층으로 구성될 수 있다.
    * API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
    * 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다.
  * PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.<br><br>

**5. Uniform Interface(인터페이스 일관성)**
  * URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
  * HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
    * 특정 언어나 기술에 종속되지 않는다.
<br><br>

## REST의 장단점
### 장점
* HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.
* HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.
* HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
* Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
* REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
* 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
* 서버와 클라이언트의 역할을 명확하게 분리한다.

### 단점
* 표준이 자체가 존재하지 않아 정의가 필요하다.
* 사용할 수 있는 메소드가 4가지밖에 없다.
* HTTP Method 형태가 제한적이다.
* 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
* 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스플로러)
<br><br><br>

# REST API
<img src="https://user-images.githubusercontent.com/55045377/125569508-db58f48d-26fd-4f2b-a58f-4b4282520816.png" width=75% height=75%>

RESPT API란 REST를 기반으로 만들어진, REST의 원리를 따르는 API를 의미한다.

하지만 REST API를 올바르게 설계하기 위해서는 지켜야 하는 몇가지 규칙이 있다. 해당 규칙을 알아보자.
<br><br>

## REST API 설계 규칙

1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.<br>

```
Bad Example → http://khj93.com/Running/
Good Example → http://khj93.com/run/  
```

2. 마지막에 슬래시 (/)를 포함하지 않는다.<br>

```
Bad Example → http://khj93.com/test/  
Good Example → http://khj93.com/test
```

3. 언더바 대신 하이폰을 사용한다.<br>

```
Bad Example → http://khj93.com/test_blog
Good Example → http://khj93.com/test-blog  
```

4. 파일확장자는 URI에 포함하지 않는다.<br>

```
Bad Example → http://khj93.com/photo.jpg  
Good Example → http://khj93.com/photo  
```

5. 행위를 포함하지 않는다.<br>

```
Bad Example → http://khj93.com/delete-post/1  
Good Example → http://khj93.com/post/1  
```

6. 슬래시 구분자(/ )는 계층 관계를 나타내는데 사용한다.<br>

```
Ex → http://restapi.example.com/houses/apartments
```
<br><br>

## REST API 설계 예시
<img src="https://user-images.githubusercontent.com/55045377/125580497-2b03d759-7737-476d-b3d7-ff3611194f02.png" width=80% height=80%>

* **참고**<br>
  응답 상태 코드<br>
  * 1xx : 전송 프로토콜 수준의 정보 교환
  * 2xx : 클라어인트 요청이 성공적으로 수행됨
  * 3xx : 클라이언트는 요청을 완료하기 위해 추가적인 행동을 취해야 함
  * 4xx : 클라이언트의 잘못된 요청
  * 5xx : 서버쪽 오류로 인한 상태코드
<br><br><br>

# RESTful

<img src="https://user-images.githubusercontent.com/55045377/125570375-45330654-a8d5-4f60-b251-fb73f1fb0f23.png">

RESTful이란 REST의 원리를 따르는 시스템을 의미하며, 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.<br>
하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아니다.<br>
REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있으며<br>
URI 규칙을 올바르게 지키지 않은 API는(REST API의 설계 규칙을 올바르게 지키지 못한 시스템은) REST API를 사용하였지만 RESTful 하지 못한 시스템이라고 할 수 있다.

예를 들면<br>
Ex1) CRUD 기능을 모두 POST로만 처리하는 API,<br>
Ex2) route에 resource, id 외의 정보가 들어가는 경우(/students/updateName)<br>
를 RESTful 하지 못하다고 할 수 있다.
<br><br><br>

























---
# 출처
* **REST [[REST](#rest)]**
  * https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
  * https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
* **REST API [[REST API](#rest-api)]**
  * https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
  * https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
* **RESTful [[RESTful](#restful)]**
  * https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
  * https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
<br><br><br>
