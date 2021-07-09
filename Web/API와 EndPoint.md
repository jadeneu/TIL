# 목차
* [API](#api)
* [API의 역할](#api의-역할)
* [EndPoint](#endpoint)
---
* [출처](#출처)
<br><br><br>


# API
> API(Application Programming Interface 애플리케이션 프로그래밍 인터페이스, 응용 프로그램 프로그래밍 인터페이스)는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다. <br>
> 주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다.

<img src="https://user-images.githubusercontent.com/55045377/125065290-9d3f3100-e0ec-11eb-8d6c-d38eb8b2f06d.png" width=75% height=75%>

이미지 출처 : https://moonspam.github.io/What-is-an-API/

위 이미지의 상황에서 손님은 메뉴에 있는 음식을 요청하는 프로그램이라면 요리사는 요리를 만들어서 전달하는 운영 체제이다. 그리고 이 둘을 연결해주는 종업원이 바로 API의 역할을 한다.<br>
그리고 식당의 메뉴는 API의 인터페이스라고 할 수 있다.

또 다르게 설명해보자면<br>
점원은 손님에게 메뉴를 알려주고, 주방에 주문받은 요리를 요청한다. 그다음 주방에서 완성된 요리를 손님께 다시 전달한다. API는 점원과 같은 역할을 한다.<br>
API는 손님(프로그램)이 주문할 수 있게 메뉴(명령 목록)를 정리하고, 주문(명령)을 받으면 요리사(응용프로그램)와 상호작용하여 요청된 메뉴(명령에 대한 값)를 전달한다.

쉽게 말해, API는 프로그램들이 서로 상호작용하는 것을 도와주는 매개체로 볼 수 있다.
<br><br>

# API의 역할
### 1. API는 서버와 데이터베이스에 대한 출입구 역할을 한다.
데이터베이스에는 소중한 정보들이 저장된다. 따라서 모든 사람들이 이 데이터베이스에 접근할 수 있으면 안 된다. <br>
API는 이를 방지하기 위해 우리가 가진 서버와 데이터베이스에 대한 출입구 역할을 하며, 허용된 사람들에게만 접근성을 부여해준다.
<br><br>

### 2. API는 애플리케이션과 기기가 원활하게 통신할 수 있도록 한다.
여기서 애플리케이션이란 우리가 흔히 알고 있는 스마트폰 어플이나 프로그램을 말한다.<br>
API는 애플리케이션과 기기가 데이터를 원활히 주고받을 수 있도록 돕는 역할을 한다.
<br><br>

### 3. API는 모든 접속을 표준화한다.
API는 모든 접속을 표준화하기 때문에 기계/ 운영체제 등과 상관없이 누구나 동일한 액세스를 얻을 수 있다.<br>
쉽게 말해, API는 범용 플러그처럼 작동한다고 볼 수 있다.
<br><br>

# EndPoint
> 웹 서비스 EndPoint는 클라이언트 애플리케이션에서 서비스에 액세스할 수 있는 URL이다.

메소드는 같은 URL들에 대해서도 다른 요청을 하게끔 구별하게 해주는 항목이 바로 **Endpoint**이다.
<br><br>

<img src="https://user-images.githubusercontent.com/55045377/125068179-286df600-e0f0-11eb-820b-a35d7764388b.png" width=70% height=70%>

이미지 출처 : https://velog.io/@kho5420/Web-API-%EA%B7%B8%EB%A6%AC%EA%B3%A0-EndPoint
<br><br>

각각 GET, PUT, DELETE 메소드에 따라 다른 요청을 하는 것을 알 수 있다.

결국 Endpoint란 API가 서버에서 자원(resource)에 접근할 수 있도록 하는 URL이다.
<br><br><br>

























---

# 출처
* **API [[API](#api)]**
  + https://ko.wikipedia.org/wiki/API<br>
  + https://velog.io/@kho5420/Web-API-%EA%B7%B8%EB%A6%AC%EA%B3%A0-EndPoint
  + https://velog.io/@djaxornwkd12/API%EC%99%80-ENDPOINT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80<br><br>
* **API의 역할 [[API의 역할](#api의-역할)]**
  + https://velog.io/@djaxornwkd12/API%EC%99%80-ENDPOINT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80<br><br>
* **EndPoint [[EndPoint](#endpoint)]**
  + https://velog.io/@kho5420/Web-API-%EA%B7%B8%EB%A6%AC%EA%B3%A0-EndPoint
  + https://velog.io/@djaxornwkd12/API%EC%99%80-ENDPOINT%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80<br><br>
