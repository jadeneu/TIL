# CORS
**CORS**(Cross Origin Resource Sharing) 설정은 언제 필요할까?

* 다른 도메인이나 로컬 환경에서 자바스크립트로 api 등을 호출하는 경우 브라우저에서 동일 출처 위반의 에러가 나타날 수 있다.<br>
-> 동일출처정책(same-origin policy)때문에 일반적인 경우 외부서버에서 온 요청을 차단하게 된다.

이를 해결하기위해 **CORS**를 설정할 수 있다.

CORS는 자바스크립트를 사용한 api 등의 리소스 호출시 **동일 출처(같은 호스트네임)가 아니더라도 정상적으로 사용 가능하도록 도와주는 방법**이다.

<br><br>

## Flask에서 CORS 설정하기
먼저 flask_cors는 파이썬 패키지 모듈이므로 pip를 사용하여 설치한다.
```
pip install flask_cors
```

<br>

이제 설치된 flask_cors를 앱에서 사용하기 위해 import를 하고 아래와 같이 코드를 입력한다.
```python
import flask_cors CORS, cross_origin

CORS(app)
```

<br>

이제 모든 도메인에 대하여 CORS가 설정되었다. 다른 도메인에서 해당 페이지를 스크립트로 호출하는 경우 CORS 에러가 더 이상 발생하지 않을 것이다.

<br><br>


## 원하는 호스트, 도메인 주소만 사용 가능하도록 설정하기
원하는 주소만 호출할 수 있도록 변경할 수도 있다.

CORS()의 두번째 인자에 resources를 사용하고 origin과 그 값으로 허용할 도메인 주소를 입력한다.
```python
CORS(app, resources={r'*': {'origins': '*'}})

CORS(app, resources={r'*': {'origins': 'https://webisfree.com'}})

CORS(app, resources={r'/_api/*': {'origins': 'https://webisfree.com:5000'}})
```
위 코드를 보면 각각 다르게 설정되어 있다. 우선 * 표시는 모든 것을 허용한다는 의미이다.

<br>

### ✅첫 번째 코드 살펴보기
맨 위의 코드는 모든 곳에서 호출하는 것을 허용하게 된다.

### ✅두 번째 코드 살펴보기
두 번째 코드는 `https://webisfree.com`에서의 호출만 허용한다.

### ✅세 번째 코드 살펴보기
마지막은 `https://webisfree.com:5000`으로 5000 포트를 명시했다. 또한 추가적으로 `/_api/`의 하위 주소를 가지는 경우에만 호출이 가능하다. 즉 `https://webisfree.com:5000/_api/` 등의 주소를 호출할 때에만 정상적으로 리소스를 전달할 수 있다.

<br><br>

## 여러개의 호스트 주소 허용하기
이번에는 하나가 아닌 여러개의 도메인 주소를 허용하는 방법이다.
```python
CORS(application, resources={r'*': {'origins': ['https://webisfree.com', 'http://localhost:8080']}})
```
위와 같이 배열로 여러개의 주소를 허용할 수 있다. 위 코드는 두 개의 주소 `https://webisfree.com` 그리고 `http://localhost:8080`에 대하여 허용하도록 설정했다.





<br><br><br>










---

# References
* https://webisfree.com/2020-01-01/python-flask%EC%97%90%EC%84%9C-cors-cross-origin-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
