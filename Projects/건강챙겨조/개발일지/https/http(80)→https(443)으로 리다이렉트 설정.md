# 구현하게 된 과정
https 적용을 했는데 http 접속도 가능하고 https 접속도 가능했다. 이렇게 되면 http로 접속했을 때 보안 문제가 생길 수 있기 때문에 http로 접근할 경우 https 페이지를 보여주는 작업이 필요하다.

http -> https 접근 방법을 알아보자.

<br><br>

# HTTP(80) → HTTPS(443)

### 1. aws 로드 밸런서로 이동하고 해당 로드 밸런서를 선택한다.

<img src="https://user-images.githubusercontent.com/55045377/130224228-1dd82862-0abb-42d4-8b46-0186f40d2256.png">

<br>

### 2. "리스너" 탭을 선택한 후 HTTP:80을 누르고 편집을 클릭한다.

<img src="https://user-images.githubusercontent.com/55045377/130225901-56d0e749-632b-4eb2-9880-5f12e9b2b5b1.png" width=80% height=80%>

### 3. 기존에 설정되어있던 기본 작업 "다음으로 전달 중: ..."을 삭제한다.

<br>

### 4. "작업 추가"를 누른 후, "리디렉션 대상..."을 선택한다.

<br>

### 5. 다음과 같이 입력 후 "업데이트" 한다.

<img src="https://user-images.githubusercontent.com/55045377/130225116-ba164bf2-c1a7-4ee3-8cb7-56a40eae7a29.png" width=80% height=80%>

<br>

### 이제 http://ggulgguk.shop 으로 접속하면 https://ggulgguk.shop 으로 리다이렉션 된다.

<br><br>

# 새로 알게된 점
처음 https를 적용시키고 url에 입력해볼 때는 https가 정말 적용된건지 확신이 잘 안 서서 여러 가지 의심을 했었는데,<br>
중요한 건 https 적용이 되지 않았다면 url 주소 창에 https라고 쳐도 페이지가 로드 되지 않는다는 것이다.<br>
그러니까 https로 입력해도 페이지가 뜨고, http로 입력해도 페이지가 로드된다면 https 적용이 된 것이다.<br>
여기서 필요한 건 http->https 리다이렉트이다!

<br><br>

# References
* https://medium.com/@yangga0070/aws-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C-http-https-%EB%A6%AC%EB%8B%A4%EC%9D%B4%EB%A0%89%EC%85%98-37c1039be0ab

<br><br><br>
