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
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">  # 5 (<nav> 전체)
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent">  # 6
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
    </nav>  # 5 (<nav> 전체)

    <div class="container ng-warning">  # 7 (<div> 전체)
        <h4>This is CONTENT area.</h4>
    </div>

    <footer class="fixed-bottom bg-info">  # 8 (<footer> 전체)
        <h4>This is FOOTER area.</h4>
    </footer>

    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"></script>                        # 9 (<script> 전체)
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/und/popper.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>  # 9 (<script> 전체)
</body>
</html>
```

라인별로 설명하면 다음과 같다.
* **# 1**: HTML5 스펙을 준수하는, 일반적인 head 태그 내용이다.
* **# 2**: 페이지 제목을 지정한다. 브라우저의 최상단 탭에 표시되는 내용이다.
* **# 3**: 부트스트랩의 CDN 리크 중 CSS 링크 복사 (보안과 관련된 integrity/crossorigin 속성은 삭제했다)
* **# 4**: <body>는 크게 3개 영역 즉 메인 메뉴 영역, CONTENT 영역, FOOTER 영역으로 나누었다. 그리고 '# 5'의 메인 메뉴에 fixed-top 클래스를 사용한 경우에는 \<body\>에 padding-top 스타일 속성을 충분히 줘야 메뉴 영역과 CONTENT 영역이 겹치지 않는다.
* **# 5**: 메인 메뉴 영역이다. 메뉴를 만드는 navbar 클래스를 사용했고, 배경색을 bg-primary로 지정했다. 클래스 fixed-top을 사용하면, 상하 스크롤이 되어도 메뉴 영역이 브라우저 상단에 고정된다.






















 

