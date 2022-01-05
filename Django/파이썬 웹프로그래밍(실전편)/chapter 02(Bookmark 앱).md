# 목차
* [chapter02](#chapter02-실전-프로그램-개발---bookmark-앱)
  * [2.1 애플리케이션 설계하기](#21-애플리케이션-설계하기)
    * [테이블 설계](#테이블-설계)
    * [로직 설계](#로직-설계)
    * [URL 설계](#url-설계)
    * [작업/코딩 순서](#작업코딩-순서)
  * [2.2 개발 코딩하기 - 뼈대](#22-개발-코딩하기---뼈대)
    * [프로젝트 설정 파일 변경](#프로젝트-설정-파일-변경)
    * [기본 테이블 생성](#기본-테이블-생성)
  * [2.3 개발 코딩하기 - 모델](#23-개발-코딩하기---모델)
    * [테이블 정의](#테이블-정의)
    * [Admin 사이트에 테이블 반영](#admin-사이트에-테이블-반영)
    * [테이블 모습 확인하기](#테이블-모습-확인하기)
  * [2.4 개발 코딩하기 -URLconf](#24-개발-코딩하기---urlconf)
  * [2.5 개발 코딩하기 - 뷰](#25-개발-코딩하기---뷰)
  * [2.6 개발 코딩하기 - 템플릿](#26-개발-코딩하기---템플릿)
    * [bookmark_list.html 템플릿 작성하기](#bookmark_listhtml-템플릿-작성하기)

---

* [개념 정리](#개념-정리)
  * [✅ 제네릭 뷰](#-제네릭-뷰)
  * [✅ as_view()](#-as_view)
  * [✅ ListView와 DetailView](#-listview와-detailview)
  * [✅ {% url %}](#--url-)

<br><br><br>

---

<img src="https://user-images.githubusercontent.com/55045377/146200727-11a9464e-b1c9-48d5-970f-262d53dee12c.png" width=40%>

# chapter02 실전 프로그램 개발 - Bookmark 앱
> 북마크 앱은 자주 방문하는 사이트를 등록해 두었다가 나중에 그 사이트에 재방물할 때 쉽게 찾아갈 수 있게 해주는 앱이다.

## 2.1 애플리케이션 설계하기
* 설계해야할 목록
  * 화면 UI
  * 화면에 접속하기 위한 URL
  * 서버에서 필요한 테이블
  * 처리 로직

### 테이블 설계
테이블 설계 내용은 모델 코딩에 반영되고 models.py 파일에 코딩한다.

### 로직 설계
로직 설계는 처리 흐름을 설계하는 것으로, 웹 프로그래밍에서는 URL을 받아서 최종 HTML 템플릿 파일을 만드는 과정이 하나의 로직이 된다.<br>
그 과정에서 리다이렉션(redirection)이 일어날 수도 있고, 템플릿 파일에서 URL 요청이 발생할 수도 있다.<br>
이런 과정들을 모두 고려해서 문서로 표현하는 것이 로직 설계 과정이며, 설계의 핵심이라고 할 수 있다.

### URL 설계
URL 설계 내용은 URLconf 코딩에 반영되고, urls.py 파일에 코딩한다.<br>
이 단계에서 중요한 점은 어떤 [제네릭 뷰](#-제네릭-뷰)를 사용할 것인지 등을 결정하는 것이다.

### 작업/코딩 순서
* 작업/코딩 순서
  * 뼈대 만들기
  * 모델 코딩하기
  * URLconf 코딩하기
  * 뷰 코딩하기
  * 템플릿 코딩하기
  * 그 외 코딩하기


<br><br>

## 2.2 개발 코딩하기 - 뼈대
* 프로젝트에 필요한 디렉토리 및 파일을 구성한다.
* 설정 파일을 세팅한다.
* 기본 테이블을 생성한다.
* 관리자 계정인 슈퍼유저를 생성한다.
* 프로젝트가 만들어지면 그 하위에 애플리케이션 디렉토리 및 파일을 구성한다.

<br>

### 프로젝트 설정 파일 변경
프로젝트 생성 후,<br>
프로젝트 설정 파일인 settings.py 파일에 필요한 사항을 지정한다. (Database, INSTALLED_APPS, TIME_ZONE 항목 등)

**첫 번째로** ALLOWED_HOSTS 항목을 적절하게 지정해야 한다. 장고는 DEBUG=True면 개발 모드로, False면 운영 모드로 인식한다. 운영 모드인 경우는 ALLOWED_HOSTS에 반드시 서버의 IP나 도메인을 지정해야 하고, 개발 모드인 경우에는 값을 지정하지 않아도 ['localhost', '127.0.0.1']로 간주한다.

지금은 개발 모드이므로 아래와 같이 지정한다.
```
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
```

<br>

**두 번째로** 프로젝트에 포함되는 애플리케이션들은 모두 설정 파일에 등록되어야 한다. 따라서 startapp 명령으로 애플리케이션을 생성했다면, 설정 파일에 꼭 등록해야 한다.

<br>

**세 번째로** 템플릿 관련 사항도 확인한다. 보통 DIRS 항목을 제외한 나머지 항목들은 변경하지 않고 사용한다. DIRS 항목은 프로젝트 템플릿 파일이 위치할 디렉토리를 지정한다. 템플릿 파일을 찾을 때, 프로젝트 디렉토리는 애플리케이션 템플릿 디렉토리보다 먼저 검색한다.
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],   # 수정
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

<br>

**네 번째로** 프로젝트에 사용할 데이터베이스 엔진이다. 장고는 디폴트로 SQLite3 데이터베이스 엔진을 사용하도록 설정되어 있다. 물론 다른 데이터베이스 엔진으로 변경할 수도 있는데, 만일 MySQL이나 Oracle, PostgreSQL 등 다른 데이터베이스로 변경하고 싶다면 settings.py 파일에서 수정해주면 된다. 이 책에서는 SQLite3 데이터베이스를 사용할 것이므로, 설정된 사항을 변경하지 않고 확인만 한다.

파일 중간에 다음과 같은 데이터베이스 설정 항목을 확인할 수 있다.
```python
# Database
# https://docs.djangoproject.com/en/3.2/ref/settings/#databases
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

<br>

**다섯 번째는** 타임존 지정이다. 처음에는 세계표준시(UTC)로 되어 있는데, 한국 시간으로 변경한다.
```python
# TIME_ZONE = 'UTC'
TIME_ZONE = 'Asia/Seoul'
```

<br>

**여섯 번째는** 정적 파일에 관한 설정이다. STATIC_URL 항목은 최초 settings.py 파일이 만들어질 때 장고가 지정해준 그대로이고, STATICFILES_DIRS 항목은 프로젝트 정적 파일이 위치할 디렉토리를 의미하는데, 개발자가 직접 지정한다. 

템플릿 파일을 찾는 순서와 비슷하게, 정적 파일을 찾을 때도 각 애플리케이션의 static/ 디렉토리보다 STATICFILES_DIRS 항목으로 지정한 디렉토리를 먼저 검색한다.
```python
STATIC_URL = '/static/'

STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]    # 추가
```

<br>

**일곱 번째는** 미디어 관련 사항을 지정하는 것이다. 이 항목들은 파일 업로드 기능을 개발할 대 필요한 설정이다. 
```python
# 추가
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

<br>

지금까지 설정 파일에 등록한 7개 항목 이외에도 특정 앱에서 필요한 항목이나 임의로 원하는 항목을 추가, 삭제해도 된다. 한 예로 한국 시간대만 사용하는 프로젝트인 경우 아래와 같이 지정하면, DB에 저장되는 시간도 UTC가 아니라 한국 시간으로 저장되어 편리하다.
```python
# USE_TZ = True
USE_TZ = False
```

<br>

또 다른 예로 장고의 사용 언어를 아래처럼 한글로 지정하면, Admin 사이트 화면의 메뉴 및 설명 등이 한글로 표시된다. 이 설정은 날짜/시간 등의 표현이 달라지므로 주의해야 한다. 예를 들어, `date:'b'`의 의미가 en-us인 경우는 'jan'이 되지만, ko-kr인 경우는 '1월'로 변경된다는 점을 유의하자.
```python
# LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'ko-kr'
```

<br>

### 기본 테이블 생성
다음은 기본 테이블 생성을 위해 `migrate` 명령을 실행한다. migrate 명령은 데이터베이스에 변경사항이 있을 때 이를 반영해주는 명령이다.

그런데 아직 데이터베이스 테이블을 만들지도 않았는데, 왜 이 명령이 필요할까? 

장고는 모든 웹 프로젝트 개발 시 반드시 사용자와 그룹 테이블 등이 필요하다는 가정 하에 설계되었다. 그래서 우리가 테이블을 만들지 않았더라도, 사용자 및 그룹 테이블 등을 만들어주기 위해 프로젝트 개발 시작 시점에 이 명령을 실행하는 것이다. 명령을 실행하면 migrate 명령에 대한 로그가 보이고, 실행 결과로 SQLite3 데이터베이스 파일인 db.sqlite3 파일이 생성된 것을 확인할 수 있다.

<br><br>

## 2.3 개발 코딩하기 - 모델
> 모델 작업은 데이터베이스에 테이블을 생성하도록 해주는 작업이다.

### 테이블 정의
앞에서 설계한 것처럼, 북마크 앱은 Bookmark 테이블 하나만 필요하다. 테이블은 models.py 파일에 정의한다. 앞에서 테이블을 설계한 내용에 따라 다음과 같이 입력하면 된다.
```python
from django.db import models

class Bookmark(models.Model):
    title = models.CharField('TITLE', max_length=100, blank=True)
    url = models.URLField('URL', unique=True)
    
    def __str__(self):
        return self.title
```
장고에서는 테이블을 하나의 클래스로 정의하고, 테이블의 컬럼은 클래스의 변수로 매핑한다. 테이블 클래스는 django.db.models.Model 클래스를 상속받아 정의하고, 각 클래스 변수의 타입도 장고에서 미리 정의해 둔 필드 클래스를 사용한다.

위의 Bookmark 모델 클래스 정의를 자세히 알아보자.
* **title** 컬럼은 공백(blank) 값을 가질 수 있다.
* **URLField()** 필드 클래스의 첫 번째 파라미터인 'URL'문구는 url 컬럼에 대한 별칭(verbose_name)이다. 뒤에서 설명할 Admin 사이트에서 이 문구를 보게 될 것이다.
* **\_\_str\_\_()** 함수는 객체를 문자열로 표현할 때 사용하는 함수이다. 장고에서 모델 클래스의 객체는 테이블에 들어 있는 레코드 하나를 의미한다. 나중에 보게 될 Admin 사이트나 장고 셸 등에서 테이블의 레코드명을 보여줘야 하는데, 이때 \_\_str\_\_() 함수를 정의하지 않으면 레코드명이 제대로 표현되지 않는다.

<br>

### Admin 사이트에 테이블 반영
앞서 models.py 파일에서 정의한 테이블을 Admin 사이트에 보이도록 등록하자. 다음처럼 admin.py 파일에 등록해주면 된다.
```python
from django.contrib import admin
from bookmark.models import Bookmark

@admin.register(Bookmark)
class BookmarkAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'url')
```
BookmarkAdmin 클래스는 Bookmark 클래스가 Admin 사이트에서 어떤 모습으로 보여줄지를 정의하는 클래스이다.
* Bookmark 내용을 보여줄 때, **id와 title, url**을 화면에 출력하라고 지정했다.
* **@admin.register()** 데코레이터를 사용하여 어드민 사이트에 등록한다.

### 테이블 모습 확인하기
* **NOTE_** Admin 사이트의 이름 표기 방식
  * **애플리케이션명**: startapp appname 명령 시 사용한 appname을 대문자로 표시한다.
  * **테이블명**: 아래 '객체명'에 's'를 추가하고 첫 글자를 대문자로 표시한다. 예를 들어, 모델 클래스명이 MyCooky라면 테이블명은 My cookys가 된다. 이는 verbose_name_plural 메타 옵션으로 변경할 수 있다.
  * **객체명**: models.py 파일에 정의한 모델 클래스 이름을 소문자와 공백으로 바꾼 것이다. 예를 들어, 모델 클래스명이 MyCooly라면 my cooky가 된다. 이는 verbose_name 메타 옵션으로 지정할 수 있다.

<br><br>

## 2.4 개발 코딩하기 - URLconf
북마크 앱의 URL은 간단하다. Admin 사이트까지 포함해서 3개의 URL과 뷰가 필요한데, 그 내용을 urls.py 파일에 코딩하면 된다.
* bookmark/urls.py
```python
from django.contrib import admin
from django.urls import path

from bookmark.views import BookmarkLV, BookmarkDV


urlpatterns = [
    path('admin/', admin.site.urls),  # 1
    
    # class-based views
    path('bookmark/', BookmarkLV.as_view(), name='index'),  # 2
    path('bookmark/<int:pk>/', BookmarkDV.as_view(), name='detail'),  # 3
]
```
코드 설명은 다음과 같다.
* path() 함수는 route, view 2개의 필수 인자와 kwargs, name 2개의 선택 인자를 받는다. 여기서 정해준 name 인자값은 템플릿 파일에서 많이 사용된다.
* **# 1**: 장고의 Admin 사이트에 대한 URLconf는 이미 정의되어 있는데, Admin 사이트를 사용하기 위해서는 항상 이렇게 정의한다.
* **# 2**: URL /bookmark/ 요청을 처리할 뷰 클래스를 BookmarkLV로 지정한다. URL 패턴의 이름은 'index'로 명명한다.
* **# 3**: URL /bookmark/99/ 요청을 처리할 뷰 클래스를 BookmarkDV로 지정한다. URL 패턴의 이름은 'detail'이라고 명명한다. BookmarkDV 뷰 클래스에 pk=99라는 인자가 전달된다.

<br><br>

## 2.5 개발 코딩하기 - 뷰
앞에서 URLconf를 코딩하면서 뷰를 클래스형 뷰로 정의하기 위해 각 URL에 따른 해당 클래스 및 [as_view()](#-as_view) 메소드를 지정했다. 이제 URLconf에서 지정한 클래스형 뷰를 코딩해보자.

클래스형 뷰를 코딩할 때 가장 먼저 고려해야 할 사항은, 어떤 제네릭 뷰를 사용할 것인가이다. 여기에서는 [ListView와 DetailView](#-listview와-detailview) 제네릭 뷰를 선택해 사용할 것이다.

* Bookmark 테이블에서 여러 개의 레코드를 가져오는 로직이 필요하므로, ListView 선택
* Bookmark 테이블에서 한 개의 레코드를 가져오는 로직이 필요하므로, DetailView 선택

bookmark/views.py 파일에 다음 내용을 입력한다.
```python
from django.views.generic import ListView, DetailView
from bookmark.models import Bookmark

class BookmarkLV(ListView):  # 1
    model = Bookmark
    
class BookmarkDV(DetailView):  # 2
    model = Bookmark

```
코드 설명은 다음과 같다.
* **# 1**: BookmarkLV는 Bookmark 테이블의 레코드 리스트를 보여주기 위한 뷰로서, ListView 제네릭 뷰를 상속받는다. ListView를 상속받는 경우는 객체가 들어 있는 리스트를 구성해서 이를 컨텍스트 변수로 템플릿 시스템에 넘겨주면 된다. 만일 이런 리스트를 테이블에 들어 있는 모든 레코드를 가져와 구성하는 경우에는 테이블명, 즉 모델 클래스명만 지정해주면 된다.<br><br>
그리고 명시적으로 지정하지 않아도 장고에서 디폴트로 알아서 지정해주는 속성이 2가지 있다.<br>
첫 번째는 컨텍스트 변수로 **object_list**를 사용하는 것이고, 두 번째는 템플릿 파일명을 **모델명 소문자_list.html** 형식의 이름으로 지정하는 것이다. Bookmark 테이블로부터 모든 레코드를 가져와 object_list라는 컨텍스트 변수를 구성한다. 템플릿 파일명은 디폴트로 bookmark/bookmark_list.html 파일이 된다.

<br>

* **# 2**: BookmarkDV는 Bookmark 테이블의 특정 레코드에 대한 상세 정보를 보여주기 위한 뷰로서, DetailView 제네릭 뷰를 상속받는다. DetailView를 상속받는 경우는 특정 객체 하나를 컨텍스트 변수에 담아서 템플릿 시스템에 넘겨주면 된다. 만일 테이블에서 Primary Key로 조회해서 특정 객체를 가져오는 경우네는 테이블명, 즉 모델 클래스명만 지정해주면 된다. 조회시 사용할 Primary Key 값은 URLconf에서 추출해 뷰로 넘어온 인자를(예, pk=99) 사용한다.<br><br>
그리고 명시적으로 지정하지 않아도 장고에서 디폴트로 알아서 지정해주는 속성이 2가지 있다.<br>
첫 번째는 컨텍스트 변수로 **object**를 사용하는 것이고, 두 번째는 템플릿 파일명을 **모델명 소문자_detail.html** 형식의 이름으로 지정하는 것이다. Bookmark 테이블로부터 특정 레코드를 가져와 object라는 컨텍스트 변수를 구성한다. 템플릿 파일명은 디폴트로 bookmark/bookmark_detail.html 파일이 된다. 테이블 조회 조건에 사용되는 Primary Key 값은 URLconf에서 넘겨받는데, 이에 대한 처리는 DetailView 제네릭 뷰에서 알아서 처리해준다.

<br><br>

## 2.6 개발 코딩하기 - 템플릿

### bookmark_list.html 템플릿 작성하기
다음 코드는 화면에 Bookmark 객체를 표시하고 해당 텍스트를 클릭하는 경우, \<a href\> 태그 기능에 의해 'detail' URL 패턴(/bookmark/1/ 형식)으로 웹 요청을 보낸다는 의미이다. URL 패턴을 만들어 주는 [{% url %}](#--url-) 태그 기능은 자주 사용된다.
```html
<a href="{% url 'detail' bookmark.id %}">{{ bookmark }}</a>
```











<br><br><br>

---

# 개념 정리
## ✅ 제네릭 뷰
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
```python
   def dispatch(self, request, *args, **kwargs):
        # Try to dispatch to the right method; if a method doesn't exist,
        # defer to the error handler. Also defer to the error handler if the
        # request method isn't on the approved list.
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)
```

### 3. http_method_not_allowed(request, \*args, \*\*kwargs)
뷰가 지원하지 않는 HTTP 메서드를 호출한 경우, `http_method_not_allowed()` 메서드가 대신 호출된다.
```python
   def http_method_not_allowed(self, request, *args, **kwargs):
        logger.warning(
            'Method Not Allowed (%s): %s', request.method, request.path,
            extra={'status_code': 405, 'request': request}
        )
        return HttpResponseNotAllowed(self._allowed_methods())
```

### 4. options(request, \*args, \*\*kwargs)
HTTP OPTIONS 요청에 대한 응답을 처리한다.
```python
    def options(self, request, *args, **kwargs):
        """Handle responding to requests for the OPTIONS HTTP verb."""
        response = HttpResponse()
        response['Allow'] = ', '.join(self._allowed_methods())
        response['Content-Length'] = '0'
        return response
```

### references
* https://velog.io/@reowjd/Djangoview-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0

<br>

## ✅ as_view()
```python
# urls.py

from django.urls import path
from bookmark.views import BookmarkLV, BookmarkDV

urlpatterns = [
    path('bookmark/', BookmarkLV.as_view(), name='index'), 
    path('bookmark/<int:pk>/', BookmarkDV.as_view(), name='detail'),
]
```
장고의 URL해석기는 요청과 관련된 파라미터들을 클래스가 아니라 함수에 전달하기 때문에, 클래스형 뷰는 클래스로 진입하기 위한 **as_view()** (진입 메소드) 클래스 메소드를 제공한다.<br>

as_view() 진입 메소드의 역할은 클래스의 인스턴스를 생성하고, 그 인스턴스의 dispatch() 메소드를 호출한다.<br>
dispatch() 메소드는 요청을 검사해서 GET, POST 등의 어떤 HTTP 메소드로 요청되었는지를 알아낸 다음, 인스턴스 내에서 해당 이름을 갖는 메소드로 요청을 중계해준다. 만일 해당 메소드가 정의되어 있지 않으면 HttpResponseNotAllowed Exception을 발생시킨다.

### References
* https://coshin.tistory.com/13

<br>

## ✅ ListView와 DetailView
django를 사용하여 웹페이지를 만들다 보면 아래와 같은 view를 작성하는 경우가 많이 생긴다.<br>
1. 특정 DB table의 모든 record를 가져와서 List로 표시 (예: 게시판 글 목록 전체)
2. 특정 DB table의 특정 record를 가져와서 Detail 내용 표시 (예 : 게시판의 특정 글 상세 내용)

### 1. 글 목록 전체 표시 (List)
* 게시판의 글 목록 전체를 표시하거나, 특정 DB table의 record 전체 (혹은 일부)를 List로 표시할 때 활용할 수 있다.
* 리스트가 테이블의 모든 레코드인 경우 모델 클래스만 지정하면 된다.

* urls.py
```python
from django.urls import path
from bookmark.views import BookmarkLV, BookmarkDV

urlpatterns = [
    path('bookmark/', BookmarkLV.as_view(), name='index'),
]
```

* views.py
```python
from django.views.generic import ListView
from bookmark.models import Bookmark


class BookmarkLV(ListView):
    model = Bookmark
```
이 코드로 모든 작업이 끝이다. Bookmark 모델에 있는 모든 레코드를 가져와서 렌더링하고 템플릿을 호출하고 리스트를 함께 보내는 기능을 한다. 

사용자가 원한다면, context 이름, queryset, template 이름을 직접 지정할 수도 있다. 
```python
class BookmarkLV(ListView):
    model = Bookmark
    context_object_name = 'my_bookmark_list'  # your own name for the list as a template variable
    queryset = Bookmark.objects.filter(title__icontains='war')[:5]  # Get 5 Bookmarks containing the title war
    template_name = 'bookmark/arbitrary_template_name_list.html'  # Specify your own template name/location
```

<br>

오버라이딩 또한 가능하다.

get_queryset() 메소드를 사용해서 반환되는 레코드를 변경할 수 있다. 위에서처럼 `queryset`을 통해 할 수도 있지만, 더 유연한 방법으로 메소드를 이용할 수 있다.
```python
class BookmarkLV(ListView):
    model = Bookmark
    
    def get_queryset(self):
        return Bookmark.objects.filter(title__icontains='war')[:5]  # Get 5 Bookmarks containing the title war
```

<br>

### 2. 특정 글 상세 표시 (Detail)
* 게시판 글 상세내용을 표시하는 것과 같이, 특정 DB table의 특정 record 상세 내용을 표시할 때 활용할 수 있다.
* 조회시 사용할 Primary Key 값은 **URLconf에서 추출하여 뷰로 넘어온 파라미터(PK)** 를 사용한다.

* urls.py
```python
from django.urls import path
from bookmark.views import BookmarkDV


urlpatterns = [
    path('bookmark/<int:pk>/', BookmarkDV.as_view(), name='detail'),
]
```
* views.py
```python
from django.views.generic import DetailView
from bookmark.models import Bookmark

class BookmarkDV(DetailView):
    model = Bookmark
```

<br>
만일 요청 레코드가 존재하지 않는다면 클래스 뷰에서는 Http404 exception이 발생한다. <br>
클래스 뷰가 아닌 function 뷰인 경우, 레코드가 없다면 exception 처리를 해주어야 한다.

```python
from django.shortcuts import get_object_or_404

def bookmark_detail_view(request, primary_key):
    bookmark = get_object_or_404(Book, pk=primary_key)
    return render(request, 'templates/bookmark_detail.html', context={'bookmark': bookmark})
```

<br>

### References
* https://wayhome25.github.io/django/2017/05/02/CBV/
* https://citylock77.tistory.com/66

<br>

## ✅ {% url %}
> {% url %} 템플릿 태그는 URL 패턴에서 URL 스트링을 추출하는 역할을 한다.

```html
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```
위 코드는 **urls** 모듈의 **path()** 함수에서 인자의 이름을 정의했으므로, **{% url %}** template 태그를 사용하여 url 설정에 정의된 특정한 URL 경로들의 의존성을 제거할 수 있다. **urls** 모듈에 서술된 URL의 정의를 탐색하는 식으로 동작한다. 

다음과 같이 'detail' 이라는 이름의 URL이 어떻게 정의되어 있는지 확인할 수 있다.
```python
...
# the 'name' value as called by the {% url %} template tag
path('<int:question_id>/', views.detail, name='detail'),
...
```

### References
* https://docs.djangoproject.com/ko/4.0/intro/tutorial03/

























