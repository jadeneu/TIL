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

<br>

### 2.1.5 작업/코딩 순서
* 이 책에서의 작업/코딩 순서

<img src="https://user-images.githubusercontent.com/55045377/147384772-e99bbb5b-54ad-46c9-8e58-80096c619138.png" width=70%>


<br><br>

## 2.2 개발 코딩하기 - 뼈대
* 프로젝트에 필요한 디렉토리 및 파일을 구성한다.
* 설정 파일을 세팅한다.
* 기본 테이블을 생성한다.
* 관리자 계정인 슈퍼유저를 생성한다.
* 프로젝트가 만들어지면 그 하위에 애플리케이션 디렉토리 및 파일을 구성한다.

<br>

### 2.2.1 프로젝트 생성

### 2.2.2 프로젝트 설정 파일 변경
프로젝트 설정 파일인 settings.py 파일에 필요한 사항을 지정한다. (Database, INSTALLED_APPS, TIME_ZONE 항목 등)

**첫 번째로** ALLOWED_HOSTS 항목을 적절하게 지정해야 한다. 장고는 DEBUG=True면 개발 모드로, False면 운영 모드로 인식한다. 운영 모드인 경우는 ALLOWED_HOSTS에 반드시 서버의 IP나 도메인을 지정해야 하고, 개발 모드인 경우에는 값을 지정하지 않아도 ['localhost', '127.0.0.1']로 간주한다.

지금은 개발 모드이므로 아래와 같이 지정한다.
```
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
```

<br>

**두 번째로** 프로젝트에 포함되는 애플리케이션들은 모두 설정 파일에 등록되어야 한다. 따라서 startapp 명령으로 애플리케이션을 생성했다면, 설정 파일에 꼭 등록해야 한다.

<br>

**세 번째로** 템플릿 관련 사항도 확인한다. 템플릿 항목을 설정하는 방법은 1.8 버전부터 변경되었다. 자세한 설명은 [15.1 템플릿 설정 항목](#)을 참고하자. 보통 DIRS 항목을 제외한 나머지 항목들은 변경하지 않고 사용한다. DIRS 항목은 프로젝트 템플릿 파일이 위치할 디렉토리를 지정한다. 템플릿 파일을 찾을 때, 프로젝트 디렉토리는 애플리케이션 템플릿 디렉토리보다 먼저 검색한다.
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],   # 
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

정적 파일 처리에 대한 자세한 설명은 [15.7 staticfiles 애플리케이션 기능](#)을 참고하자.
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

지금까지 설정 파일에 등록한 7개 항목 이외에도 특정 앱에서 필요한 항목이나 여러분이 임의로 원하는 항목을 추가, 삭제해도 된다. 한 예로 한국 시간대만 사용하는 프로젝트인 경우 아래와 같이 지정하면, DB에 저장되는 시간도 UTC가 아니라 한국 시간으로 저장되어 편리하다.
```python
# USE_TZ = True
USE_TZ = False
```

<br>

또 다른 예로 장고의 사용 언어를 아래처럼 한글로 지정하면, Admin 사이트 화면의 메뉴 및 설명 등이 한글로 표시된다. 이 설정은 날짜/시간 등의 표현이 달라지므로 주의해야 한다. 예를 들어, [3.2.5절의 post_archive_year.html](#)에 사용된 date:'b'의 의미가 en-us인 경우는 'jan'이 되지만, ko-kr인 경우는 '1월'로 변경된다는 점을 유의하기 바랍니다.
```python
# LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'ko-kr'
```

<br>

사실 settings.py 설정 파일은 프로젝트의 전반적인 사항들을 설정해주는 곳으로, 루트 디렉토리를 포함한 각종 디렉토리의 위치, 로그의 형식, 프로젝트에 포함된 애플리케이션 등이 지정되어 있어서 그 내용에 익숙해질수록 장고의 모습을 이해하는 데 도움이 된다. 그러니 당장은 이햐가 되지 않더라도 어떤 항목이 정의되어 있는지 자주 확인해보자.

<br>

### 2.2.3 기본 테이블 생성
다음은 기본 테이블 생성을 위해 `migrate` 명령을 실행한다. migrate 명령은 데이터베이스에 변경사항이 있을 때 이를 반영해주는 명령이다.

그런데 아직 데이터베이스 테이블을 만들지도 않았는데, 왜 이 명령이 필요할까? 장고는 모든 웹 프로젝트 개발 시 반드시 사용자와 그룹 테이블 등이 필요하다는 가정 하에 설계되었다. 그래서 우리가 테이블을 만들지 않았더라도, 사용자 및 그룹 테이블 등을 만들어주기 위해 프로젝트 개발 시작 시점에 이 명령을 실행하는 것이다. 명령을 실행하면 migrate 명령에 대한 로그가 보이고, 실행 결과로 SQLite3 데이터베이스 파일인 db.sqlite3 파일이 생성된 것을 확인할 수 있다.

<br>

### 2.2.4 슈퍼유저 생성
Admin 사이트에 로그인하기 위한 관리자(슈퍼유저)를 만들어보자. `createsuperuser` 명령을 입력한다.

화면의 지시에 따라 Username/Email/Password/Password(again)을 입력해서 관리자를 생성한다.

<br>

### 2.2.5 애플리케이션 생성
다음으로 북마크 앱을 만드는 명령을 실행한다. bookmark는 원하는 애플리케이션 명칭을 입력하면 된다.
```python
(venv)$ python manage.py startapp bookmark
```
그러면 장고가 북마크 앱 디렉토리와 그 하위에 필요한 파일들을 생성해준다.

그 후에는 각 파일마다 이름에 맞는 내용들을 채워주면 된다. apps.py 파일은 애플리케이션의 설정 클래스를 정의하는 파일로, 장고 1.9 버전부터 사용하기 시작했다. 또한 장고가 자동으로 만들어준 파일 이외에도, 필요하다면 개발자가 임의의 파일을 생성해도 된다.

* **NOTE: 애플리케이션 명칭**<br>
보통은 영문법에 따라 애플리케이션 이름을 복수 단어로 지정하는 경우도 많지만, 이 책에서는 영문법의 단수/복수 구별이 불편하다고 생각되어 주로 단수 단어를 쓴다. 그래서 애플리케이션 이름을 bookmark로 사용했지만, bookmarks라고 해도 무방하다.

<br>

### 2.2.6 애플리케이션 등록
앞의 [2.2.2 프로젝트 설정 파일 변경](#222-프로젝트-설정-파일-변경)에서 설명했듯이, 프로젝트에 포함되는 애플리케이션들은 모두 설정 파일에 등록되어야 한다. 따라서 우리가 개발하고 있는 북마크 앱도 settings.py 파일에 등록해야 한다. 애플리케이션을 등록할 때는 간단하게 애플리케이션의 모듈형인 'bookmark'만 등록해도 되지만, 애플리케이션의 설정 클래스로 등록하는 것이 더 정확한 벙법이다.

bookmark 앱의 설정 클래스는 `startapp bookmark` 명령 시에 자동 생성된 apps.py 파일에 BookmarkConfig라고 정의되어 있다. 그래서 장고가 설정 클래스를 찾을 수 있도록 모듈 경로까지 포함되어 'bookmark.apps.BookmarkConfig'라고 등록한다.
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'bookmark.apps.BookmarkConfig',    # 추가
]
```

<br>

* **NOTE: 애플리케이션 설정 클래스**<br>
애플리케이션의 설정 클래스는 해당 애플리케이션에 대한 메타 정보를 저장하기 위한 클래스로, **django.apps.AppConfig** 클래스를 상속받아 작성한다. 앱 설정 클래스에는 앱 이름(name), 레이블(label), 별칭(verbose_name), 경로(path) 등을 설정할 수 있으며, 이 중 이름(name)은 필수 속성이다.<br>
설정 클래스 개념은 장고 1.7 버전부터 사용했으며, 1.7 및 1.8 버전에서는 장고가 내부에서 설정 클래스를 만들어 사용했으나, 1.9 버전부터는 개발자가 직접 작성하도록 변경되었다. 설정 클래스를 작성하는 위치는 애플리케이션 디렉토리 하위의 apps.py 파일이다.<br>
만일 애플리케이션을 INSTALLED_APPS 항목에 등록 시 설정 클래스 대신에 애플리케이션 디렉토리만 지정하면, \_\_init\_\_.py 파일에서 **default_app_config** 항목으로 지정된 클래스를 설정 클래스로 사용한다. default_app_config 항목도 정의되지 않으면 장고의 기본 AppConfig 클래스를 설정 클래스로 사용한다.

<br><br>

## 2.3 개발 코딩하기 - 모델
> 모델 작업은 데이터베이스에 테이블을 생성하도록 해주는 작업이다.

### 2.3.1 테이블 정의
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

### 2.3.2 Admin 사이트에 테이블 반영
앞서 models.py 파일에서 정의한 테이블을 Admin 사이트에 보이도록 등록하자. 다음처럼 admin.py 파일에 등록해주면 된다.
```python
from django.contrib import admin
from bookmark.models import Bookmark

@admin.register(Bookmark)
class BookmarkAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'url')
```


















<br><br>

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

## references
* https://velog.io/@reowjd/Djangoview-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0








