## try ... except ... 문
오류 처리를 위해 try except 문을 쓸 수 있다.

다음은 오류 처리를 위한 try, except문의 기본 구조이다.
```
try:
    ...
except [발생 오류[as 오류 메시지 변수]]:
    ...
```
try 블록 수행 중 오류가 발생하면 except 블록이 수행된다. 하지만 try 블록에서 오류가 발생하지 않는다면 except 블록은 수행되지 않는다.

except 구문을 자세히 살펴보자.

***except [발생 오류 [as 오류 메시지 변수]]:***

위 구문을 보면 [ ] 기호를 사용하는데, 이 기호는 **괄호 안의 내용을 생략할 수 있다는 관례 표기법**이다. 즉 except 구문은 다음 3가지 방법으로 사용할 수 있다.

**1. try, except만 쓰는 방법**<br>
```
try:
    ...
except:
    ...
```
이 경우는 오류 종류에 상관없이 오류가 발생하면 except 블록을 수행한다.

**2. 발생 오류만 포함한 except문**<br>
```
try:
    ...
except 발생 오류:
    ...
```
이 경우는 오류가 발생했을 때 except문에 미리 정해 놓은 오류 이름과 일치할 때만 except 블록을 수행한다는 뜻이다.

**3. 발생 오류와 오류 메시지 변수까지 포함한 except문**<br>
```
try:
    ...
except 발생 오류 as 오류 메시지 변수:
    ...
```
이 경우는 두 번째 경우에서 오류 메시지의 내용까지 알고 싶을 때 사용하는 방법이다.

이 방법의 예를 들어 보면 다음과 같다.
```python
try:
    4 / 0
except ZeroDivisionError as e:
    print(e)
```
위처럼 4를 0으로 나누려고 하면 ZeroDivisionError가 발생하여 except 블록이 실행되고 변수 e에 담기는 오류 메시지를 다음과 같이 출력한다.

> 결과값: division by zero

<br>

* **출처: https://wikidocs.net/30#try-except**
<br><br>
