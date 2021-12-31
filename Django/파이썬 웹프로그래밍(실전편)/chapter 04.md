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



























