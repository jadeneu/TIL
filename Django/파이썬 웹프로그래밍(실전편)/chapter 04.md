# 목차

# chapter 04 프로젝트 첫 페이지 만들기
> ※ 첫 페이지는 사이트 전쳬의 이미지를 대표하므로, 기능보다는 디자인 측면이 중요하므로 HTML, 스타일시트, 자바스크립트 등의 지식이 필요한 분야이다.

## 4.1 첫 페이지 설계하기
첫 페이지를 개발하는 것이므로 화면 UI를 설계하는 템플릿 파일 설계가 주된 작업이고, URL 설계는 아주 간단하다. 테이블 설계는 필요하지 않고, 이 책에서는 화면 UI 개발을 부트스트랩 4.3.1 버전으로 개발한다.

### 4.1.1 화면 UI 설계
장고의 템플릿 상속 기능을 사용할 예정이고, 이에 따라 페이지는 하나지만 base.html과 home.html 두 개의 파일을 개발한다.

### 4.1.2 테이블 설계
테이블 변경 사항은 없다.

### 4.1.3 URL 설계
프로젝트의 첫 페이지는 루트(/) URL에 대한 처리 로직을 개발하는 것이다. 그래서 기존 URL에 루트 URL만 추가하면 된다.

* 이 책에서의 프로젝트 첫 페이지 - URL 설계
<img src="https://user-images.githubusercontent.com/55045377/147815170-de3a5eeb-f9ad-46e5-90cd-cea21292630a.png">

### 4.1.4 작업/코딩 순서
* 이 책에서의 프로젝트 첫 페이지 - 작업/코딩 순서

<img src="https://user-images.githubusercontent.com/55045377/147815250-06af000b-e341-48d1-85b0-ce542d8a412c.png">

<br><br>

## 4.2 개발 코딩하기
테이블 변경 사항이 없으므로 모델 코딩은 불필요하고, URLconf 코딩부터 시작한다.

### 4.2.1 뼈대 만들기
앱을 신규로 만드는 것이 아니므로 뼈대 작업은 없다.

### 4.2.2 모델 코딩하기
테이블에 대한 변경 사항은 없다.

### 4.2.3 URLconf 코딩하기
[4.1.3 URL 설계](#413-url-설계)의 내용을 참고해서 URLconf를 정의하면 된다. 애플리케이션에 대한 URL이 아니라 프로젝트에 대한 URL이므로 projects/urls.py 파일에 임포트 문장 및 루트(/) URL 두 줄만 추가하면 된다. 뷰 클래스는 HomeView, URL 패턴명은 'home'이라고 정의했다.
```python
from django.contrib import admin
from django.urls import path, include
from projects.views import HomeView    # 추가

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', HomeView.as_view(), name='home'),    # 추가
    path('bookmark/', include('bookmark.urls')),
    path('blog/', include('blog.urls')),
]
```

### 4.2.4 뷰 코딩하기
이제 URLconf에서 지정한 HomeView를 코딩한다. HomeView는 특별한 처리 로직 없이 단순히 템플릿만 보여주는 로직이므로, TemplateView 제네릭 뷰를 상속받아 코딩한다.

파일의 위치는 프로젝트와 관련된 뷰이므로, projects/views.py 파일에 코딩하는 것이 적절하다. 다음과 같은 내용으로 views.py 파일을 새로 만든다.
```python
from django.views.generic import TemplateView

class HomeView(TemplateView):                  # 1
    template_name = 'home.html'                # 2
```

위 소스를 라인별로 설명하면 다음과 같다.
* **# 1**: TemplateView 제네릭 뷰를 상속받아 사용하고 있다. TemplateView를 사용하는 경우에는 필수적으로 template_name 클래스 변수를 오버라이딩으로 지정해줘야 한다.<br>
TemplateView는 urls.py에서 view를 지정하지 않고 템플릿을 바로 지정해야 할 때 사용한다. 템플릿이 주어지면 해당 템플릿을 렌더링한다.
* **# 2**: projects 프로젝트의 첫 화면을 보여주기 위한 템플릿 파일을 home.html로 지정했다. 템플릿 파일이 위치하는 디렉토리 settings.py 파일의 TEMPLATE_DIRS 항목으로 지정되어 있다.

<br>

### 4.2.5 템플릿 코딩하기 - 부트스트랩 메인 메뉴: home.html
첫 페이지의 템플릿을 코딩할 파일의 이름은 home.html이다. 이는 앞의 views.py 파일에 있는 HomeView에서 지정한 파일명이다. 파일의 위치는 개별 애플리케이션 템플릿이 아니라 프로젝트 템플릿이므로, 프로젝트 템플릿 디렉토리에 생성한다. 프로젝트 템플릿 디렉토리는 settings.py 파일에 다음과 같이 정의한 바 있다.
```python
TEMPLATE_DIRS = [os.path.join(BASE_DIR, 'templates')]
```
home.html에는 모든 페이지에서 공통으로 사용하는 제목과 메뉴도 포함되는데, 이들은 4.2.6절에서 장고의 상속 기능을 활용하여 base.html로 옮길 것이다.

새로운 디렉토리를 만들고 home.html 템플릿 파일에 다음과 같이 입력한다.
* 부트스트랩 메인 메뉴 - templates/home.html
```html
<!DOCTYPE html>
<html lang="ko">  # 1
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Django WEb Programming</title>  # 2

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">  # 3
</head>

<body style="padding-top:90px;">  # 4
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">  --------------------------------------------- # 5
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Link</a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown">
                        Dropdown
                    </a>
                    <div class="dropdown-menu">
                        <a class="dropdowm-item" href="#">Action</a>
                        <a class="dropdowm-item" href="#">Another action</a>
                        <div class="dropdown-divider"></div>
                        <a class="dropdowm-item" href="#">Something else here</a>
                    </div>
                </li>
                <li class="nav-item">
                    <a class="nav-link disabled" href="#" tabindex="-1">Disabled</a>
                </li>
            </ul>
            <form class="form-inline my-2 my-lg-0">
                <input class="form-control mr-sm-2" type="search" placeholder="Search">
                <button class="btn btn-outline-successs my-2 ny-sm-0" type="submit">Search</button>
            </form>
        </div>
    </nav>  ------------------------------------------------------------------------------------------------------------- # 5

    <div class="container bg-warning">  --------- # 6
        <h4>This is CONTENT area.</h4>
    </div>  ------------------------------------- # 6

    <footer class="fixed-bottom bg-info">  ------ # 7
        <h4>This is FOOTER area.</h4>
    </footer>  ---------------------------------- # 7

    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/und/popper.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
</body>
</html>
```

라인별로 설명하면 다음과 같다.
* **# 1**: HTML5 스펙을 준수하는, 일반적인 head 태그 내용이다.
* **# 2**: 페이지 제목을 지정한다. 브라우저의 최상단 탭에 표시되는 내용이다.
* **# 3**: 부트스트랩의 CDN 리크 중 CSS 링크 복사 (보안과 관련된 integrity/crossorigin 속성은 삭제했다)
* **# 4**: \<body\>는 크게 3개 영역 즉 메인 메뉴 영역, CONTENT 영역, FOOTER 영역으로 나누었다. 그리고 '# 5'의 메인 메뉴에 fixed-top 클래스를 사용한 경우에는 \<body\>에 padding-top 스타일 속성을 충분히 줘야 메뉴 영역과 CONTENT 영역이 겹치지 않는다.
* **# 5**: 메인 메뉴 영역이다. 메뉴를 만드는 navbar 클래스를 사용했고, 배경색을 bg-primary로 지정했다. 클래스 fixed-top을 사용하면, 상하 스크롤이 되어도 메뉴 영역이 브라우저 상단에 고정된다.
* **# 6**: CONTENT 영역이다. 부트스트랩의 container 클래스를 사용하면 내용이 화면 중앙에 위치하고, 사용하지 않으면 화면을 좌우로 모두 채운다. 화면 영역을 보여주기 위해 임시로 배경색을 bg-warning으로 지정했다.
* **# 7**: FOOTER 영역이다. 부트스트랩의 fixed-bottom 클래스를 사용하면 화면 맨 아래에 위치한다. 화면의 세로 길이가 한 페이지 분량보다 작을 때 FOOTER로 많이 사용하는 클래스이다. FOOTER 영역의 배경색을 bg-info로 지정했다.

<br>

### 4.2.6 템플릿 코딩하기 - 상속 기능: base.html
> 상속 기능을 사용하는 이유는 모든 페이지에 공통인 메인 메뉴를 base.html 파일에 코딩하고, 각 페이지에서는 base.html 코드를 재활용하기 위함이다.

이번에는 home.html 코드에 장고의 상속 기능을 적용해서, base.html과 home.html 두 개의 파일로 나눈다.

다음과 같이 base.html 파일에 입력한다. base.html에는 모든 페이지에서 공통으로 사용하는 제목과 메뉴, 그리고 상속 기능에 맞춰 페이지 구성 요소들을 배치하는 [{% block %}](#--block-) 태그 기능이 들어 있다. 

* 상속 기능 적용 - base.html 코딩
```html
<!DOCTYPE html>  ---------------------------------------------------------------- # 1
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  ----- # 1
    
    <title>{% block title %}Django Web Programming{% endblock %}</title>  # 2

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">

    {% block extra-style %}{% endblock %}  # 3
</head>

<body style="padding-top: 90px;">  ----------------------------------------------------------------------------------- # 4 
    
    ######## home.html 의 중간 내용과 동일 ########
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">Link</a>
                </li>
                <li class="nav-item dropdown">
                    <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown">
                        Dropdown
                    </a>
                    <div class="dropdown-menu">
                        <a class="dropdowm-item" href="#">Action</a>
                        <a class="dropdowm-item" href="#">Another action</a>
                        <div class="dropdown-divider"></div>
                        <a class="dropdowm-item" href="#">Something else here</a>
                    </div>
                </li>
                <li class="nav-item">
                    <a class="nav-link disabled" href="#" tabindex="-1">Disabled</a>
                </li>
            </ul>
            <form class="form-inline my-2 my-lg-0">
                <input class="form-control mr-sm-2" type="search" placeholder="Search">
                <button class="btn btn-outline-successs my-2 ny-sm-0" type="submit">Search</button>
            </form>
        </div>
    </nav>  ---------------------------------------------------------------------------------------------------------- # 4

    <div class="container bg-warning">  ------- # 5
        {% block content %}{% endblock %}
    </div>  ----------------------------------- # 5

    {% block footer %}{% endblock %}  # 6

    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/und/popper.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>

    {% block extra-script %}{% endblock %}
    
</body>
</html>
```

라인별로 설명하면 다음과 같다.
* **# 1**: 이 부분은 {% block %} 태그가 없으므로, 상속을 받는 하위 html 파일, 이번 절에서는 home.html에도 동일하게 들어간다.
* **# 2**: \<title\> 태그 부분은 각 페이지마다 달라지는 부분이므로 {% block %} 태그를 사용했다. 하위 html 파일에서 오버라이딩하지 않으면, 즉 {% block title %} 태그를 사용하지 않으면 Django Web Programming이라는 문구가 디폴트로 사용된다. 아래와 같이 코딩하는 것과의 차이점을 이해하자.<br>
    ```python
    <title>{% block title %}{% endblock %}</title>
    ```
* **# 3**: 하위 html 파일에서 이 부분에 \<style\> 태그를 추가할 가능성이 있으므로, {% block %} 태그를 기입했다. 이때 블록 태그 이름 extra-style은 변경해도 된다.
* **# 4**: 변경 사항 없음. {% block %} 태그가 없으므로 하위 html 파일에 동일하게 나타난다.
* **# 5**: 본문 내용은 각 페이지마다 달라질 수 있으므로, {% block %} 태그를 사용했다. 블록 태그 이름은 content이다. 하위 html 파일에서 채우는 {% block content %} 내용에는 container 클래스가 적용되도록 했다. 즉 아래와 같이 코딩하는 것과의 차이점을 이해하자.<br>
    ```python
    {% block content %}{% endblock %}
    ```
* **# 6**: 하위 html 파일에서 이 부분에 FOOTER 내용을 추가할 가능성이 있으므로, {% block %} 태그를 기입했다. 블록 태그 이름은 footer이다. '# 5'와 유사한 설명인데, 하위 html 파일에서는 FOOTER 내용이 없을 수도 있으므로, 아래와 같이 코딩하지 않았다.
    ```python
    <footer class="fixed-bottom bg-info">
    {% block footer %}{% endblock %}
    </footer>
    ```

<br>

이번 base.html 템플릿 파일에는 하위 템플릿 파일에서 재정의할 수 있도록 다음 4가지 블록을 정의하고 있다.
* **{% block title %}**: 하위 페이지마다 페이지 제목을 다르게 정의할 수 있다.
* **{% block extra-style %}**: base.html 파일에서 사용하는 base.css 파일 이외에, 하위 페이지에서 필요한 CSS 파일을 정의할 수 있다.
* **{% block content %}**: 하위 페이지마다 실제 본문 내용을 정의할 수 있다.
* **{% block footer %}**: 하위 페이지마다 FOOTER 내용을 다르게 정의할 수 있다. 여기에서는 home.html 파일에서만 footer 블록을 사용하고, 나머지 페이지에서는 사용하지 않는다.

<br>

### 4.2.7 템플릿 코딩하기 - 상속 기능: home.html
> 템플릿 상속 기능을 사용함으로써 home.html 코드가 간단해지고 base.html 코드를 재사용하고 있다는 점이 중요하다.

아래와 같이 home.html 파일의 내용을 변경한다. 상위 base.html 파일에서 정의한 블록 태그들을 하위 home.html 파일에서 오버라이딩하면 된다.

* 상속 기능 적용 - home.html 코딩
```html
{% extends 'base.html' %}  # 1

{% block title %}home.html{% endblock %}  # 2

{% block content %}  -------------------- # 3
    <h4>This is CONTENT area.</h4>
{% endblock %}   ------------------------ # 3

{% block footer %}  --------------------- # 4
<footer class="fixed-bottom bg-info">
    <h4>This is FOOTER area.</h4>
</footer>
{% endblock %}  ------------------------- # 4
```

라인별로 설명하면 다음과 같다.
* **# 1**: base.html 템플릿 파일을 상속받는다. {% extends %} 태그 문장은 항상 첫 줄에 작성해야 한다.
* **# 2**: title 블록을 재정의한다. 즉, 페이지 title을 home.html이라고 정의한다.
* **# 3**: content 블록을 재정의한다. 블록의 내용은 [4.2.5 템플릿 코딩하기](#425-템플릿-코딩하기---부트스트랩-메인-메뉴-homehtml)와 동일하다.
* **# 4**: footer 블록을 재정의한다. 블록의 내용은 [4.2.5 템플릿 코딩하기](#425-템플릿-코딩하기---부트스트랩-메인-메뉴-homehtml)와 동일하다.

상위 base.html 파일에서 정의한 extra-style 블록과 extra-script 블록처럼, 하위 home.html 파일에서 필요하지 않으면 사용하지 않아도 된다.

이렇게 [4.2.5 템플릿 코딩하기](#425-템플릿-코딩하기---부트스트랩-메인-메뉴-homehtml)에서 작성한 home.html 파일을, 템플릿 상속 기능을 활용하여 base.html과 home.html 두 개의 파일로 나누어 작성했다.

<br>

### 4.2.8 템플릿 코딩하기 - base.html 완성
앞에서 작성한 base.html 파일은 부트스트랩의 샘플 코드를 사용한 것이므로, 이제 우리 프로젝트에 맞게 수정하자. 주요 내용은 메인 메뉴의 모습을 변경하는 것이다. 변경된 최종 코드는 다음과 같다.

* base.html 완성
```html
<!DOCTYPE html>  ------------------------------------------------------------------------------------------------- # 1
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Django Web Programming{% endblock %}</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">

    {% block extra-style %}{% endblock %}
</head>

<body style="padding-top: 90px;">
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">  --------------------------------------- # 1
        <span class="navbar-brand mx-5 nb-0 font-weight-bold font-italic">Django - Python Web Programming</span>  # 2
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <li class="nav-item mx-1 btn btn-primary">  ------------------------------------------------------- # 3
                    <a class="nav-link text-white" href="{% url 'home' %}">Home</a>
                </li>
                <li class="nav-item mx-1 btn btn-primary">
                    <a class="nav-link text-white" href="{% url 'bookmark:index' %}">Bookmark</a>
                </li>
                <li class="nav-item mx-1 btn btn-primary">
                    <a class="nav-link text-white" href="{% url 'blog:index' %}">Blog</a>
                </li>
                <li class="nav-item mx-1 btn btn-primary">
                    <a class="nav-link text-white" href="">Photo</a>
                </li>  -------------------------------------------------------------------------------------------- # 3

                <li class="nav-item dropdown mx-1 btn btn-primary">  ---------------------------------------------- # 4
                    <a class="nav-link dropdown-toggle text-white" href="#" data-toggle="dropdown">Util</a>
                    <div class="dropdown-menu">
                        <a class="dropdowm-item" href="{% url 'admin:index' %}">Admin</a>
                        <div class="dropdown-divider"></div>
                        <a class="dropdowm-item" href="{% url 'blog:post_archive' %}">Archive</a>
                        <a class="dropdowm-item" href="">Search</a>
                    </div>
                </li>  -------------------------------------------------------------------------------------------- # 4
            </ul>

            <form class="form-inline my-2" action="" method="post"> {% csrf_token %}  ----------------------------- # 5
                <input class="form-control mr-sm-2" type="search" placeholder="global search" name="search_word">
            </form>  ---------------------------------------------------------------------------------------------- # 5
        </div>
    </nav>

    <div class="container">  ----------------- # 6
        {% block content %}{% endblock %}
    </div>  ---------------------------------- # 6

    {% block footer %}{% endblock %}

    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/und/popper.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
    <script src="https://kit.fontawesome.com/c998a172fe.js"></script>  # 7

    {% block extra-script %}{% endblock %}

</body>
</html>
```

라인별로 설명하면 다음과 같다.
* **# 1**: 변경사항 없음
* **# 2**: 사이트 제목의 문구를 바꾸었고, 스타일을 bold/italic 체로, 마진(mx, mb)을 넓게 조정했다. \<a\> 태그 대신에 \<span\> 태그로 변경해서, 제목에는 링크를 달지 않았다.
* **# 3**: 메뉴 이름을 Home, Bookmark, Blog, Photo로 변경했고, 각 제목에는 {% url %} 태그를 활용해 링크를 달았다. 글자색을 흰색(text-white)으로, 메뉴 간 간격 조정(mx-1), 마우스 오버시 버트(btn) 모양이 되도록 변경했다. Photo 앱은 9장에서 개발 예정.
* **# 4**: 드롭다운 메뉴명을 Util이라고 했고, Admin/Archive/Search 서브 메뉴를 만든다. 각 서브 메뉴에는 해당 URL 링크를 연결한다. Search 기능은 8장에서 개발 예정.
* **# 5**: 폼 메뉴에는 서버로 요청을 보낼 수 있도록 action/method/name 속성을 추가했고, [{% csrf_token %}](#--csrf_token-) 태그도 추가했다. Search 버튼은 없어도 되므로 삭제했다.
* **# 6**: 테스트 용도로 기입했던 bg-warning 클래스는 삭제한다.
* **# 7**: 아이콘을 사용하기 위해 폰트어썸(FontAwesome) CDN 링크를 추가했다.

<br>

### 4.2.9 템플릿 코딩하기 - home.html 완성






































# 한 걸음 더
## ✅ {% block %}
```python
{% block content %}
{% endblock %}
```
위 코드는 block을 만든 것과 같다. 템플릿 태그 {% block %}으로 HTML 내에 들어갈 수 있는 공간을 만든 것이다. 

<br>

## ✅ {% csrf_token %}
> Django의 CSRF공격에 대한 방어

### 1. CSRF(Cross Site Request Forgery)
웹사이트 취약점 공격의 하나로, 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 하는 공격이다.

### 2. Django의 CSRF공격에 대한 방어
CSRF 공격을 방어하기 위한 다양한 방법이 있지만 Django에서는 기본적으로 csrf token을 이용한다.

POST 요청에 대해서만 csrf token을 발급하고 체크한다. POST 양식을 사용하는 템플릿에서 \<form\> 태그 안에 {% csrf_token %} 태그를 사용.

### 3. 동작 과정
1. 사용자가 해당 페이지에 접속하면 Django에서 자동으로 csrf_token을 클라이언트로 보내어 cookie에 저장
2. 사용자가 form을 모두 입력한 후 제출버튼을 클릭한다.
3. form과 cookie의 csrf_token을 함께 POST로 전송한다.
4. 전송된 token의 유효성을 검증
5. 유효한 요청이면 요청을 처리
    * token이 유효하지 않거나(없거나 값이 잘못된 경우) 검증 오류 시에는 403 Forbidden Response 반환

### Reference
https://chagokx2.tistory.com/49


