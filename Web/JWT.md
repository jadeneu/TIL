# 목차

# JWT(JSON Web Token)
> JWT는 다른 JSON 기반 표준에 의존한다: JSON 웹 시그너처 및 JSON 웹 암호화.

> JWT는 선택적 서명 및 선택적 암호화를 사용하여 데이터를 만들기 위한 인터넷 표준으로, 페이로드는 몇몇 클레임(claim) 표명(assert)을 처리하는 JSON을 보관하고 있다.

**JSON Web Token(JWT)** 은 웹표준 (RFC 7519)으로서 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인 (self-contained) 방식으로 정보를 안전성 있게 전달해준다.

**1) JWT 는 수많은 프로그래밍 언어에서 지원되며,** C, Java, Python, C++, R, C#, PHP, JavaScript, Ruby, Go, Swift 등 대부분의 주류 프로그래밍 언어에서 지원된다.<br>
또한 **2) 자가 수용적 (self-contained)이다.** JWT는 필요한 모든 정보를 자체적으로 지니고 있다. JWT 시스템에서 발급된 토큰은 토큰에 대한 기본정보, 전달할 정보(로그인시스템에서는 유저 정보를 나타냄) 그리고 토큰이 검증됐다는것을 증명해주는 signature 를 포함하고있다.<br>
JWT는 자가수용적이므로, 두 개체 사이에서 **3) 손쉽게 전달 될 수 있다.** 웹서버의 경우 HTTP의 헤더에 넣어서 전달 할 수도 있고, URL 의 파라미터로 전달 할 수도 있다.
<br><br><br>

# JWT가 사용되는 경우
다음과 같은 상황에서 JWT가 유용하게 사용 될 수 있다:
* **회원 인증**<br>
  JWT를 사용하는 가장 흔한 시나리오이다. 유저가 로그인을 하면, 서버는 유저의 정보에 기반한 토큰을 발급하여 유저에게 전달해준다. 그 후, 유저가 서버에 요청을 할 때 마다 JWT를 포함하여 전달한다.<br>
  서버가 클라이언트에게서 요청을 받을때 마다 해당 토큰이 유효하고 인증됐는지 검증을 하고, 유저가 요청한 작업에 권한이 있는지 확인하여 작업을 처리한다.<br>
  서버측에서는 유저의 세션을 유지 할 필요가 없다. 즉 유저가 로그인 되어있는지 되어있지 않은지 신경 쓸 필요가 없고, 유저가 요청을 했을때 토큰만 확인하면 되니 세션 관리가 필요 없기 때문에 서버 자원을 많이 아낄 수 있다.
* **정보 교류**<br>
  JWT는 두 개체 사이에서 안정성있게 정보를 교환하기에 좋은 방법이다.<br>
  그 이유는 정보가 sign이 되어있기 때문에 정보를 보낸 이가 바뀌진 않았는지, 또 정보가 도중에 조작되지는 않았는지 검증할 수 있다.
<br><br><br>

# JWT 토큰 구성
JWT 는 . 을 구분자로 3가지의 문자열로 되어있다. 구조는 다음과 같이 이루어져 있다.

<img src="https://user-images.githubusercontent.com/55045377/125295583-55284480-e360-11eb-91c4-6fddea2cc48d.png" width=70% height=70%>

* ***페이로드는 사용에 있어서 전송되는 데이터를 뜻한다.***

자, 그럼 이렇게 3가지 부분으로 나뉘어져 있는 토큰을 하나하나 파헤쳐보자.
<br><br>

* **참고**<br>
JWT 토큰을 만들때는 JWT를 담당하는 라이브러리가 자동으로 인코딩 및 해싱 작업을 해준다.<br>
하지만 이 포스트에서는 JWT 토큰이 만들어지는 과정을 더 잘 파악하기 위해 하나하나 **Node.js** 환경에서 인코딩 및 해싱을 하도록 한다.
<br><br>

## 헤더(Header)
**Header**는 두 가지의 정보를 지니고 있다.

* **typ** : 토큰의 타입을 지정한다. 바로 JWT이다.<br>
* **alg** : 해싱 알고리즘을 지정한다.  해싱 알고리즘으로는 보통 **HMAC SHA256** 혹은 **RSA** 가 사용되며, 이 알고리즘은 토큰을 검증 할 때 사용되는 signature 부분에서 사용된다.

  * ***암호학에서 HMAC(keyed-hash message authentication code, hash-based message authentication code)는 암호화 해시 함수와 기밀 암호화 키를 수반하는 특정한 유형의 메시지 인증 코드(MAC)이다.*** SHA-2, SHA-3 등의 암호화 해시 함수는 HMAC 연산을 위해 사용할 수 있다. 그 결과가 되는 MAC 알고리즘은 HMAC-X이며 여기서 X는 사용이 되는 해시 함수(예: **HMAC-SHA256** 또는 HMAC-SHA3-256)를 의미한다. <br>
    - 출처 : https://ko.wikipedia.org/wiki/HMAC<br><br>
    
  * ***RSA 암호는 공개키 암호시스템의 하나로, 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘으로 알려져 있다.*** RSA가 갖는 전자서명 기능은 인증을 요구하는 전자 상거래 등에 RSA의 광범위한 활용을 가능하게 하였다.<br>
    - 출처 : https://ko.wikipedia.org/wiki/RSA_%EC%95%94%ED%98%B8

아래 예제를 살펴보자.
```
{
  "typ": "JWT",
  "alg": "HS256"
}
```
(위 예제에서는 HMAC SHA256이 해싱 알고리즘으로 사용된다)

이 정보를 base64로 인코딩을 하면 다음과 같다.
* **Node.js 환경에서 인코딩하기**
```js
const header = {
  "typ": "JWT",
  "alg": "HS256"
};

// encode to base64
const encodedHeader = new Buffer(JSON.stringify(header))
                            .toString('base64')
                            .replace('=', '');
console.log('header: ',encodedHeader);

/* Result:
header: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
*/
```
자, 이제 JWT의 첫번째 파트가 완성되었다!
<br><br>

* **참고**<br>
  JSON 형태의 객체가 base64 로 인코딩 되는 과정에서 공백/엔터들이 사라진다. 따라서, 다음과 같은 문자열을 인코딩을 하게 된다.<br><br>
  ```
  {"alg":"HS256","typ":"JWT"}
  ```
<br><br><br>

## 정보(payload)































---
# 출처
* **JWT(JSON Web Token) [[JWT(JSON Web Token)](#jwtjson-web-token)]**
  * https://ko.wikipedia.org/wiki/JSON_%EC%9B%B9_%ED%86%A0%ED%81%B0
