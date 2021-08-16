## 에러가 발생한 과정
서버에서 SSL 인증서를 설치할 nginx 웹서버용 인증서 설치 툴인 Certbot을 설치하려고 했다. 
```
$ sudo apt install certbot python3-certbot-nginx
```

<br>

그런데 아래와 같은 에러가 떴다.

<img src="https://user-images.githubusercontent.com/55045377/129504885-7149c745-6aa7-4cd3-8c58-4ab413deb743.png" width=80% height=80%>

이는 다음과 같이 해결할 수 있다.

<br><br>

## 에러 해결 방법
```
$ sudo rm /var/lib/dpkg/info/*
$ sudo dpkg --configure -a
$ sudo apt update -y
```
먼저 /var/lib/dpkg/info 경로에 있는 모든 파일들을 제거해준 다음<br>
--configure -a 옵션을 주면서 dpkg 명령을 실행한다.<br>
이후부터는 업데이트나 설치 문제가 발생하지 않는다.

<br><br>

## References
* https://corona-world.tistory.com/83

<br><br><br>
