## 에러가 발생한 과정
nginx가 제대로 작동하는지 확인하기 위해 `$ sudo nginx -t` 를 입력하여 문제가 없는 것을 확인한 뒤,<br>
nginx를 다시 가동시키기 위해 아래 명령어를 입력했다.
```
$ sudo systemctl reload nginx
```

<br>

그런데 오류가 발생했다. 이 오류를 해결해보자.
```
Warning! D-Bus connection terminated.
Failed to reload nginx.service: Connection reset by peer
See system logs and 'systemctl status nginx.service' for details.
```

<br><br>

## 에러 해결 방법
전혀 해결되지 않아서 아래 방법으로 nginx를 재가동 시켰다.
```
$ sudo nginx -s reload
```

<br><br><br>
