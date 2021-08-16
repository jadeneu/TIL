## 에러가 발생한 과정
SSL 인증서를 발급하기 위해 아래와 같은 명령어를 입력했다.
```
$ sudo certbot --nginx -d example.com -d www.example.com
```

<br>

그러나 다음과 같은 에러가 발생했다. 이를 해결해보자.
```
An unexpected error occurred:
...

During handling of the above exception, another exception occurred:

requests.exceptions.ConnectionError: HTTPSConnectionPool(host='acme-v02.api.letsencrypt.org', port=443):
Max retries exceeded with url: /directory (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x7f75ef1b2040>: 
Failed to establish a new connection: [Errno -3] Temporary failure in name resolution'))
Please see the logfiles in /var/log/letsencrypt for more details.
```

<br><br>

## 에러 해결 방법
