## 에러가 발생한 과정
snapd 버전이 최신 버전인지 확인하기위해 다음과 같은 명령을 실행시켰다.
```
$ sudo snap install core; sudo snap refresh core
```
그런데 다음과 같은 결과가 나왔다.
```
error: cannot communicate with server: Post http://localhost/v2/snaps/core: dial unix /run/snapd.socket: connect: connection refused
error: cannot communicate with server: Post http://localhost/v2/snaps/core: dial unix /run/snapd.socket: connect: connection refused
```
서버와 연결할 수 없다는 에러가 출력된다. 그렇다면 해결 방법을 알아보자.

<br><br>

## 에러 해결
에러는 세 줄의 명령어로 해결되었다.
```
$ sudo apt-get update && sudo apt-get install -yqq daemonize dbus-user-session fontconfig
$ sudo daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
$ exec sudo nsenter -t $(pidof systemd) -a su - $LOGNAME
```
위와 같은 명령어를 입력하면 에러가 해결된다.<br>
daemonize, dbus-user-session, fontconfig를 설치하고, 몇 가지를 더 하는 것 같다.

에러가 해결돼서 다행이다!

<br><br>

---
에러 해결 설명: https://discourse.ubuntu.com/t/using-snapd-in-wsl2/12113

<br><br><br>

