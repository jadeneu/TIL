# 목차
* [JWT(JSON Web Token)](#jwtjson-web-token)
* [JWT가 사용되는 경우](#jwt가-사용되는-경우)
* [JWT 토큰 구성](#jwt-토큰-구성)
  + [1. 헤더(Header)](#1-헤더header)
  + [2. 정보(Payload)](#2-정보payload)
    - [등록된 (registered) 클레임](#-1-등록된-registered-클레임)
    - [공개 (public) 클레임](#-2-공개-public-클레임)
    - [비공개 (private) 클레임](#-3-비공개-private-클레임)
    - [예제 Payload](#예제-payload)
  + [3. 서명(Signature)](#3-서명signature)
---
* [출처](#출처)
<br><br><br>

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

## 1. 헤더(Header)
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
<br><br>

## 2. 정보(Payload)
**Payload** 부분에는 토큰에 담을 정보가 들어있다.<br>
여기에 담는 정보의 한 ‘조각’ 을 **클레임(claim)** 이라고 부르고, 이는 name / value 의 한 쌍으로 이루어져있다.<br>
토큰에는 여러 개의 클레임들을 넣을 수 있다.

클레임의 종류는 다음과 같이 크게 세 분류로 나뉘어져있다
* 등록된 (**registered**) 클레임
* 공개 (**public**) 클레임
* 비공개 (**private**) 클레임

클레임을 하나 하나 알아보자.
<br><br>

### # 1 등록된 (registered) 클레임
등록된 클레임들은 서비스에서 필요한 정보들이 아닌, 토큰에 대한 정보들을 담기 위하여 이름이 이미 정해진 클레임들이다.<br>
등록된 클레임의 사용은 모두 선택적(optional)이며, 이에 포함된 클레임 이름들은 다음과 같다.
* **iss** : 토큰 발급자 (issuer)
* **sub** : 토큰 제목 (subject)
* **aud** : 토큰 대상자 (audience)
* **exp** : 토큰의 만료시간 (expiration), 시간은 NumericDate 형식으로 되어있어야 하며 (예: 1480849147370) 언제나 현재 시간보다 이후로 설정되어 있어야 한다.
* **nbf** : Not Before 를 의미하며, 토큰의 활성 날짜와 비슷한 개념이다. 여기에도 NumericDate 형식으로 날짜를 지정하며, 이 날짜가 지나기 전까지는 토큰이 처리되지 않는다.
* **iat** : 토큰이 발급된 시간 (issued at), 이 값을 사용하여 토큰의 age 가 얼마나 되었는지 판단할 수 있다.
* **jti** : JWT의 고유 식별자로서, 주로 중복적인 처리를 방지하기 위하여 사용된다. 일회용 토큰에 사용하면 유용하다.
<br><br>

### # 2 공개 (public) 클레임
공개 클레임들은 충돌이 방지된(collision-resistant) 이름을 가지고 있어야 한다.<br>
충돌을 방지하기 위해서는 클레임 이름을 URI 형식으로 짓는다.
```js
{
    "https://velopert.com/jwt_claims/is_admin": true
}
```
<br><br>

### # 3 비공개 (private) 클레임
등록된 클레임도 아니고, 공개된 클레임들도 아니다. 양 측간에(보통 클라이언트<->서버) 협의하에 사용되는 클레임 이름들이다.<br>
공개 클레임과는 달리 이름이 중복되어 충돌이 될 수 있으니 사용할 때에 유의해야한다.
```js
{
    "username": "velopert"
}
```
<br><br>

### 예제 Payload
```js
{
    "iss": "velopert.com",
    "exp": "1485270000000",
    "https://velopert.com/jwt_claims/is_admin": true,
    "userId": "11028373727102",
    "username": "velopert"
}
```
위 예제 payload는 **2**개의 **등록된 클레임**, **1**개의 **공개 클레임**, **2**개의 **비공개 클레임**으로 이뤄져있다.

위 데이터를 base64 로 인코딩을 해보자!
* **Node.js 환경에서 인코딩하기**
```js
const payload = {
    "iss": "velopert.com",
    "exp": "1485270000000",
    "https://velopert.com/jwt_claims/is_admin": true,
    "userId": "11028373727102",
    "username": "velopert"
};

// encode to base64
const encodedPayload = new Buffer(JSON.stringify(payload))
                            .toString('base64')
                            .replace('=', '');

console.log('payload: ',encodedPayload);

/* result
payload:  eyJpc3MiOiJ2ZWxvcGVydC5jb20iLCJleHAiOiIxNDg1MjcwMDAwMDAwIiwiaHR0cHM6Ly92ZWxvcGVydC5jb20vand0X2NsYWltcy9pc19hZG1pbiI6dHJ1ZSwidXNlcklkIjoiMTEwMjgzNzM3MjcxMDIiLCJ1c2VybmFtZSI6InZlbG9wZXJ0In0
*/
```
<br>
JWT 토큰의 두번째 파트가 완성되었다!
<br><br>

* **주의**<br>
  **base64**로 인코딩을 할 때 **dA==** 처럼 뒤에 **=** 문자가 한두 개 붙을 때가 있다. 이 문자는 **base64** 인코딩의 **padding** 문자라고 부른다.<br>
  JWT 토큰은 가끔 URL의 파라미터로 전달 될 때도 있는데, 이 **=** 문자는 **url-safe** 하지 않으므로 제거되어야 한다.<br>
  패딩이 한 개 생길 때도 있고 두 개 생길 때도 있는데, **전부 지워도**(제거해줘도) 디코딩 할 때 전혀 문제가 되지 않는다.
<br><br>

## 3. 서명(Signature)
JSON Web Token 의 마지막 부분은 바로 **서명(signature)** 이다.<br>
이 서명은 헤더의 인코딩값과 정보의 인코딩값을 합친 후 주어진 비밀키로 해쉬를 하여 생성한다.

서명 부분을 만드는 수도코드(pseudocode)의 구조는 다음과 같다.
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```
이렇게 만든 해쉬를, **base64** 형태로 나타내면 된다. (문자열을 인코딩 하는게 아닌 **hex → base64** 인코딩을 해야한다)

이 포스트에서 사용된 예제 헤더와 정보를 해싱 해보자!<br>
먼저, 헤더와 정보의 인코딩 값 사이에 **.** 을 넣어주고 합친다.<br>
**eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.** eyJpc3MiOiJ2ZWxvcGVydC5jb20iLCJleHAiOiIxNDg1MjcwMDAwMDAwIiwiaHR0cHM6Ly92ZWxvcGVydC5jb20vand0X2NsYWltcy9pc19hZG1pbiI6dHJ1ZSwidXNlcklkIjoiMTEwMjgzNzM3MjcxMDIiLCJ1c2VybmFtZSI6InZlbG9wZXJ0In0

이 값을 비밀키의 값을 **secret** 으로 해싱을 하고 **base64로 인코딩**하면 다음과 같은 값이 나온다.
* **Node.js 환경에서 해싱 및 인코딩하기**
```js
const crypto = require('crypto');
const signature = crypto.createHmac('sha256', 'secret')
             .update(encodedHeader + '.' + encodedPayload)
             .digest('base64')
             .replace('=', '');

console.log('signature: ',signature);

//signature: WE5fMufM0NDSVGJ8cAolXGkyB5RmYwCto1pQwDIqo2w
```
이 부분 또한 padding 이 생기면 지워준다.

3번째 부분까지 끝났다!<br>
지금까지 구한 값들을 **.** 을 중간자로 다 합쳐주면, 하나의 토큰이 완성된다.

**eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.** **eyJpc3MiOiJ2ZWxvcGVydC5jb20iLCJleHAiOiIxNDg1MjcwMDAwMDAwIiwiaHR0cHM6Ly92ZWxvcGVydC5jb20vand0X2NsYWltcy9pc19hZG1pbiI6dHJ1ZSwidXNlcklkIjoiMTEwMjgzNzM3MjcxMDIiLCJ1c2VybmFtZSI6InZlbG9wZXJ0In0.** **WE5fMufM0NDSVGJ8cAolXGkyB5RmYwCto1pQwDIqo2w**<br>
(위 토큰에선 **.** 을 기준으로 줄이 나뉘어 있지만 실제론 전부 이어져 있음)

이 값을 https://jwt.io/ 의 디버거에 붙여 넣으면 아래와 같다. (JWT.IO 는 브라우저 상에서 JWT 토큰을 검증하고 생성할 수 있게 해주는 디버거 서비스이다)

<img src="https://user-images.githubusercontent.com/55045377/125446155-69feec07-5dd8-4247-a73c-a11cf19713a3.png" width=80% height=80%>

하단의 텍스트가 파란색으로 Signature Verified 라고 뜨면 JWT 토큰이 검증되었다는 것이다.

---
이번 포스트에서는 JWT의 구조가 어떻게 되어있는지, 그리고 어떤 과정을 거쳐서 만들어지는지 배울 수 있었다.<br>
토큰에서 필요한 정보를 하나하나 인코딩하고 해싱하는것은 그렇게 대단한 작업은 아니지만 조금은 귀찮기도 하다.<br>
실제로 JWT 를 서비스에서 사용할 때는 우리가 직접 base64 인코딩을 하거나 SHA256 해싱을 할 일은 없다.<br>
JWT 담당 라이브러리에 설정만 해주면 자동으로 손쉽게 만들고, 또 검증도 쉽게 해주기 때문이다.
<br><br><br>
























---
# 출처
* **JWT(JSON Web Token) [[JWT(JSON Web Token)](#jwtjson-web-token)]**
  * https://velopert.com/2389 
  * https://ko.wikipedia.org/wiki/JSON_%EC%9B%B9_%ED%86%A0%ED%81%B0
<br><br>
