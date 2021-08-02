API는 [postman](https://www.postman.com/)에서 만들었고, 작성한 API를 테스트할 수 있었다.<br>
API를 작성하면서 느꼈던 점을 정리해보려고 한다.
<br><br>

## 새로 배운 것
### 1. $ref의 용도
처음 `$ref`를 쓸 때는 그저 스키마 경로를 참조해주는 편의 기능이라고 생각했다.<br>
그래서 `$ref`로 스키마를 참조한 뒤에도 `properties`를 작성해서 스키마를 정의해줬다. (components의 schemas가 정의되어있는 상태였음)<br>
그러나 `$ref`는 components의 schemas를 가져오는 역할을 하는 것이었다.<br>
즉 정의되어있는 스키마를 참조함으로써 다시 스키마 정의를 하지 않아도 되는 것이다.

처음 실수했던 `$ref` 사용 :
```
      requestBody:
        required: true
        content:
          application/json:
            schema:
                $ref: '#/components/schemas/Signup'
                properties:
                    email:
                        type: string
                    username:
                        type: string
                    password:
                        type: string
                    gender:
                        type: string
```

<br><br>

## 참고하기 좋은 링크
모두 API builder를 사용하여 OpenAPI로 정의하는데 도움이 되는 사이트이다.
* *API Builder를 사용하면 모든 클라이언트 애플리케이션에서 사용할 수 있는 API 엔드포인트로 구성된 프로젝트를 구축하고 배포할 수 있다.*

1. https://help.liferay.com/hc/en-us/articles/360039425671-REST-Builder-OpenAPI
2. https://swagger.io/docs/specification/about/

<br><br>























