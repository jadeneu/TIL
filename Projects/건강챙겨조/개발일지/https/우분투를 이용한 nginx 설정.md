## 구현하게 된 과정
Nginx 설정에서 도메인이 제대로 설정되어 있는지 확인해야한다. 나는 웹서버를 이용하는 SSL 인증 방식을 선택했기 때문에 Nginx 설정에서 도메일이 제대로 설정되어 있어야 했다.

Nginx 설정 파일은 웹서버 설치 방식에 따라 달라지지만 Nginx 기본 설치 방식으로는 설치 시 /etc/nginx/conf.d/에 있고, 우분투에서 권장하는 방식으로 설치 시 /etc/nginx/sites-available/ 디렉토리에 있다.

나는 Nginx 기본 설치 방식으로 설치했고, 이제 도메인을 설정해보자.

<br><br>

## 도메인 설정 및 확인
```
$ sudo vim /etc/nginx/conf.d
```
위 명령어를 입력하여 vim 편집기에 들어간다.

```
server {
    listen       80;
    server_name  localhost;
    ...
}   
```
편집기에서 파일에 들어가면 위와 같은 내용이 적혀져 있다.<br>
`server_name`에 우리의 도메인을 적어주면 된다.

```
server_name example.com www.example.com;
```

`ESC+:wq` 로 저장하고 vim을 나오면 끝이다.

<br><br><br>
