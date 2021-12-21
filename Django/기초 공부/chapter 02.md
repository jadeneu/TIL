# 목차

---

<img src="https://user-images.githubusercontent.com/55045377/146200727-11a9464e-b1c9-48d5-970f-262d53dee12c.png" width=40%>

# chapter02 실전 프로그램 개발 - Bookmark 앱
> 북마크 앱을 자주 방문하는 사이트를 등록해 두었다가 나중에 그 사이트에 재방물할 때 쉽게 찾아갈 수 있게 해주는 앱이다.

## 2.1 애플리케이션 설계하기
### 2.1.1 화면 UI 설계

<br>

### 2.1.2 테이블 설계
테이블 설계 내용은 모델 코딩에 반영되고 models.py 파일에 코딩한다.

<img src="https://user-images.githubusercontent.com/55045377/146387791-f2b3fb8a-66de-4e2b-829f-805837b33ed3.png" width=70%>

<br>

### 2.1.3 로직 설계
로직 설계는 처리 흐름을 설계하는 것으로, 웹 프로그래밍에서는 URL을 받아서 최종 HTML 템플릿 파일을 만드는 과정이 하나의 로직이 된다.<br>
그 과정에서 리다이렉션(redirection)이 일어날 수도 있고, 템플릿 파일에서 URL 요청이 발생할 수도 있다.<br>
이런 과정들을 모두 고려해서 문서로 표현하는 것이 로직 설계 과정이며, 설계의 핵심이라고 할 수 있다.

* 이 책에서의 로직 설계

<img src="https://user-images.githubusercontent.com/55045377/146642882-b975f39b-e1d4-4b54-99d5-3c5c90b60320.png" width=60%>

<br>

### 2.1.4 URL 설계
URL 설계 내용은 URLconf 코딩에 반영되고, urls.py 파일에 코딩한다.<br>
이 단계에서 중요한 점은 어떤 [제네릭 뷰](#제네릭-뷰)를 사용할 것인지 등을 결정하는 것이다.

* 이 책에서의 URL 설계

<img src="https://user-images.githubusercontent.com/55045377/146678037-c9fb616d-419c-4a35-994a-e399270a0115.png" width=70%>

























## 제네릭 뷰
> 제네릭 뷰는 장고에서 기본적으로 제공하는 뷰 클래스를 의미한다. 

장고는 모델(Model), 템플릿(Template), 뷰(View)로 구성된 MTV패턴 웹프레임워크이다.<br>
이 중 뷰는 사용자 요청을 처리하고 응답을 반환하는 역할을 한다.<br>
뷰는 함수로도, 클래스로도 구현할 수 있는데, 클래스로 구현하면 **제네릭 뷰**를 사용할 수 있다.

장고에서는 용도에 따라 다양한 [제네릭 뷰](https://docs.djangoproject.com/ko/3.1/ref/class-based-views/)를 제공하고 있으며, 우리는 이 제네릭 뷰를 상속하고 메서드를 재정의하여 좀 더 편리하게 작업할 수 있다.<br>
제너릭 뷰에는 용도에 따라 ListView, DetailView, FormView, TemplateView 등이 있는데, 전부 **View** 클래스를 상속받고 있다. 그렇기 때문에 **View** 클래스 메서드를 이해하면, 다른 제네릭 뷰들의 공통 메서드도 이해할 수 있을 것이다.

## View
앞서 언급했듯이, 다른 제너릭 뷰가 상속받는 기본 제너릭 뷰이다. 메서드는 다음과 같다.

### 1. setup(request, \*args, \*\*kwargs)
`dispatch()` 전에 초기화를 수행한다. 이 메서드를 재정의하는 경우 `super()`를 호출해야 한다. 아래는 **View**에 정의된 `setup()` 코드이다.<br>
* **NOTE:** `super()`는 기반 class, 즉 superior class, 부모 class를 찾는 과정이라고 생각하자.
```python
    def setup(self, request, *args, **kwargs):
        """Initialize attributes shared by all view methods."""
        self.request = request
        self.args = args
        self.kwargs = kwargs
```

### 2. dispatch(request, \*args, \*\*kwargs)
요청을 받고 HTTP 응답을 반환하는 메서드이다. GET 요청은 `get()`으로, POST 요청은 `post()` 메서드로 호출한다.





## references
* https://velog.io/@reowjd/Djangoview-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0








