# 목차

# Refresh Token
> Access token은 수명이 있다. Access token의 수명이 다했을 때 새로운 access token을 발급 받는 방법이 **Refresh Token**이다. 

Access token의 유효기간이 끝나면 우리가 api에 접속했을 때 api가 데이터를 주지 않는다. 이때 우리는 access token을 다시 발급받아야 하는데, 그때마다 사용자에게 그 과정을 다시 거치게 하는건 힘든 일이다. 그런 경우에 우리가 손쉽게 access token을 발급받을 수 있는 방법이 바로 **Refresh Token**이다.

Refresh Token은 Access Token과 똑같은 형태의 JWT이다. 처음에 로그인을 완료했을 때 Access Token과 동시에 발급되는 Refresh Token은 긴 유효기간을 가지면서, Access Token이 만료됐을 때 새로 발급해주는 열쇠가 된다.(여기서 만료라는 개념은 유효기간이 지났다는 의미이다)
<br><br><br>

# Access Token + Refresh Token 인증 과정

<img src="https://user-images.githubusercontent.com/55045377/125559845-c7ea83fd-2a2e-46b8-ac9b-710fc604be70.png" width=80% height=80%>

이미지 출처 : https://tansfil.tistory.com/59

빨간색은 JWT 인증방식에서 추가로 들어간 과정이다.

1. 사용자가 ID , PW를 통해 로그인한다.
2. 서버에서는 회원 DB에서 값을 비교한다(보통 PW는 일반적으로 암호화해서 들어간다)

3~4. 로그인이 완료되면 Access Token, Refresh Token을 발급한다. 이때 일반적으로 회원DB에 Refresh Token을 저장해둔다.

5. 사용자는 Refresh Token은 안전한 저장소에 저장 후, Access Token을 헤더에 실어 요청을 보낸다.

6~7. Access Token을 검증하여 이에 맞는 데이터를 보낸다.

8. 시간이 지나 Access Token이 만료됐다고 하자.
9. 사용자는 이전과 동일하게 Access Token을 헤더에 실어 요청을 보낸다.

10 ~ 11. 서버는 Access Token이 만료됨을 확인하고 권한없음을 신호로 보낸다.<br>
** Access Token 만료가 될 때마다 계속 과정 9~11을 거칠 필요는 없다.사용자(프론트엔드)에서 Access Token의 Payload를 통해 유효기간을 알 수 있다. 따라서 프론트엔드 단에서 API 요청 전에 토큰이 만료됐다면 바로 재발급 요청을 할 수도 있다. **

12. 사용자는 Refresh Token과 Access Token을 함께 서버로 보낸다.
13. 서버는 받은 Access Token이 조작되지 않았는지 확인한 후, Refresh Token과 사용자의 DB에 저장되어 있던 Refresh Token을 비교한다. Token이 동일하고 유효기간도 지나지 않았다면 새로운 Access Token을 발급해준다.
14. 서버는 새로운 Access Token을 헤더에 실어 다시 API 요청을 진행한다. 

* **참고**<br>
  rfc에서도 refresh token이 사용되는 과정을 볼 수 있다.<br>
  https://datatracker.ietf.org/doc/html/rfc6749#section-1.5






























---
# 출처
* **Refresh Token [[Refresh Token](#refresh-token)]**
  * https://opentutorials.org/course/3405/22010
  * https://tansfil.tistory.com/59
