# 목차

---

<img src="https://user-images.githubusercontent.com/55045377/148194694-c2b30e4f-c675-46e5-91fa-82fede575c40.png" width=40%>

# chapter 03 실전 프로그램 개발 - Blog 앱
3장에서는 포스트를 등록하고 읽을 수 있는 기능을 먼저 개발할 것이다.

## 3.1 애플리케이션 설계하기
블로그의 글, 즉 포스트에 대한 리스트를 보여주고 포스트를 클릭하면 해당 글을 읽을 수 있는 기능을 개발한다.

## 3.2 개발 코딩하기
### 모델 코딩하기
블로그 앱은 Post 테이블 하나만 필요하다.
* blog/models.py
```python
from django.db import models
from django.urls import reverse  # 1


class Post(models.Model):
    title = models.CharField(verbose_name='TITLE', max_length=50)  # 2
    slug = models.SlugField('SLUG', unique=True, allow_unicode=True, help_text='one word for title alias.')  # 3
    description = models.CharField('DESCRIPTION', max_length=100, blank=True, help_text='simple description text.')  # 4
    content = models.TextField('CONTENT')  # 5
    create_dt = models.DateTimeField('CREATE DATE', auto_now_add=True)  # 6
    modify_dt = models.DateTimeField('MODIFY DATE', auto_now=True)  # 7
    
    class Meta:  # 8
        verbose_name = 'post'  # 9
        verbose_name_plural = 'posts'  # 10
        db_table = 'blog_posts'  # 11
        ordering = ('-modify_dt',)  # 12
        
    def __str__(self):  # 13
        return self.title
    
    def get_absolute_url(self):  # 14
        return reverse("blog:post_detail", args=(self.slug,))
    
    def get_previous(self):  # 15
        return self.get_previous_by_modify_dt()
    
    def get_next(self):  # 16
        return self.get_next_by_modify_dt()
```
코드 설명은 다음과 같다.
* **# 1**: reverse() 함수를 사용하기 위해 임포트한다. **[reverse()](#-reverse)** 함수는 URL 패턴을 만들어주는 장고의 내장 함수이다.
* **# 2**: title 컬럼은 CharField이므로 한 줄로 입력된다. 컬럼에 대한 별칭은 'TITLE'이고 최대 길이는 50글자로 설정했다. 별칭은 폼 화면에서 레이블로 사용되는 문구로, Admin 사이트에서 확인할 수 있다.
* **# 3**: **[slug](#-slug)** 컬럼은 제목의 별칭이라고 할 수 있다. SlugField에 unique 옵션을 추가해 특정 포스트를 검색할 때 기본 키 대신 사용된다. allow_unicode 옵션을 추가하면 한글 처리가 가능하다. help_text는 해당 컬럼을 설명해주는 문구로 폼 화면에 나타난다. Admin 사이트에서 확인할 수 있다.

---

**NOTE_** 슬러그란?<br>
* 슬러그(Slug)는 페이지나 포스트를 설명하는 핵심 단어의 집합이다.
* 웹 개발 분야에서는 콘텐츠의 고유 주소로 사용되어, 콘텐츠의 주소가 어떤 내용인지 쉽게 이해할 수 있도록 한다.
* 보통 슬러그는 페이지나 포스트의 제목에서 조사, 전치사, 쉼표, 마침표 등을 빼고 띄어쓰기는 하이픈(-)으로 대체해서 만들며 URL에 사용된다.
* 슬러그를 URL에 사용함으로써 검색 엔진에서 더 빨리 페이지를 찾아주고 검색 엔진의 정확도를 높여준다.

**NOTE_** SlugField 필드 타입<br>
* 슬러그는 보통 제목의 단어들을 하이픈으로 연결해 생성하며, URL에서 pk 대신 사용되는 경우가 많다. pk를 사용하면 숫자로만 되어 있어 그 내용을 유추하기 어렵지만, 슬러그를 사용하면 보통의 단어들이라서 이해하기 쉽기 때문이다.
* SlugField 필드의 디폴트 길이는 50이며, 해당 필드에는 인덱스가 디폴트로 생성된다.

---

<br>

* **# 4**: description 컬럼은 빈칸(blank)도 가능하다.
* **# 5**: content 컬럼은 TextField를 사용했으므로, 여러 줄 입력이 가능하다.
* **# 6**: create_dt 컬럼은 날짜와 시간을 입력하는 DateTimeField이며, auto_now_add 속성은 객체가 생성될 때의 시각을 자동으로 기록하게 한다.
* **# 7**: modify_dt 컬럼은 날짜와 시간을 입력하는 DateTimeField이며, auto_now 속성은 객체가 데이터베이스에 저장될 때의 시각을 자동으로 기록하게 한다. 즉, 객체가 변경될 때의 시각이 기록되는 것이다.
* **# 8**: 필드 속성 외에 필요한 파라미터가 있으면, **[Meta 내부 클래스](#-django-모델의-meta-클래스)** 로 정의한다.
* **# 9**: 테이블의 별칭은 단수와 복수로 가질 수 있는데, 단수 별칭을 'post'로 한다.
* **# 10**: 테이블의 복수 별칭을 'posts'로 한다.
* **# 11**: 데이터베이스에 저장되는 테이블의 이름을 'blog_posts'로 지정한다. 이 항목을 생략하면 디폴트는 '앱명_모델클래스명'을 테이블명으로 지정한다. 즉, db_table 항목을 지정하지 않았다면 테이블명은 blog_post가 되었을 것이다.
* **# 12**: 모델 객체의 리스트 출력 시 modify_dt 컬럼을 기준으로 내림차순으로 정렬한다.
* **# 13**: 객체의 문자열 표현 메소드인 \_\_str\_\_()은 2장에서 설명한 바 있다. 객체의 문자열을 객체.title 속성으로 표시되도록 한다.
* **# 14**: **[get_absolute_url()](#-get_absolute_url)** 메소드는 이 메소드가 정의된 객체를 지칭하는 URL을 반환한다. 메소드 내에서는 장고의 내장 함수인 reverse()를 호출한다.
* **# 15**: get_previous() 메소드는 메소드 내에서 장고의 내장 함수인 **[get_previous_by_modify_dt()](#-get_previous_by_fookwargs-get_next_by_fookwargs)** 를 호출한다. **# 12**번 설명처럼 최신 포스트를 먼저 보여주고 있으므로, get_previous_by_modify_dt() 함수는 modify_dt 컬럼을 기준으로 최신 포스트를 반환한다.
* **# 16**: get_next() 메소드는 -modify_dt 컬럼을 기준으로 다음 포스트를 반환한다. 메소드 내에서는 장고의 내장 함수인 **[get_next_by_modify_dt()](#-get_previous_by_fookwargs-get_next_by_fookwargs)** 를 호출한다. **# 15**번 설명과 동일하게, get_next_by_modify_dt() 함수는 modify_dt 컬럼을 기준으로 예전 포스트를 반환한다.

위에서 정의한 테이블도 Admin 사이트에 보이도록 admin.py 파일에 다음처럼 등록한다. 또한 Admin 사이트의 모습을 정의하는 PostAdmin 클래스도 정의한다.
* blog/admin.py
```python
from django.contrib import admin
from blog.models import Post

@admin.register(Post)  # 1
class PostAdmin(admin.ModelAdmin):  # 2
    list_display = ('id', 'title', 'modify_dt')  # 3
    list_filter = ('modify_dt',)  # 4
    search_field = ('title', 'content')  # 5
    prepopulated_fields = {'slug': ('title',)}  # 6
```
코드 설명은 다음과 같다.
* **# 1**: admin.site.register() 함수를 사용해도 되지만, 데코레이터를 사용하면 좀 더 간단하다.
* **# 2**:PostAdmin 클래스는 Post 클래스가 Admin 사이트에서 어떤 모습으로 보여줄지를 정의하는 클래스이다.
* **# 3**:Post 객체를 보여줄 때, id와 title, modify_dt를 화면에 출력하라고 지정한다.
* **# 4**:modify_dt 컬럼을 사용하는 필터 사이드바를 보여주도록 지정한다.
* **# 5**:검색박스를 표시하고, 입력된 단어는 title과 content 컬럼에서 검색하도록 한다.
* **# 6**: slug 필드는 title 필드를 사용해 미리 채워지도록 한다. 즉, 이 설정을 통해 title을 입력하면 자동으로 slug 항목도 title 값과 같은 값으로 채워진다.

<br>

### URLconf 코딩하기
2장에서는 URLconf를 projects/urls.py 한곳에만 코딩했지만, 앞으로는 ROOP_URLCONF와 APP_URLCONF, 2개의 파일에 코딩할 것이다. 이번 블로그 앱도 projects/urls.py와 blog/urls.py 2개의 파일에 코딩하고, 2장에서 작성했던 북마크 앱의 URL도 projects/urls.py와 bookmark/urls.py 2개의 파일로 수정한다.

---

**NOTE_** ROOT_URLCONF vs APP_URLCONF
* ROOT_URLCONF와 APP_URLCONF 용어는 장고의 공식 용어가 아니며 이 책에서 설명을 위해 편의상 사용하는 용어이다. 이 경우 상위 계층 URL을 ROOT_URLCONF, 하위 계층 URL을 APP_URLCONF라고 부른다.
* **ROOT_URLCONF**: URL 패턴에서 보통 첫 단어는 애플리케이션을 식별하는 단어가 온다. 첫 단어를 인식하고 해당 애플리케이션의 urls.py 파일을 포함시키기(include)위한 URLconf이다. 프로젝트 디렉터리에 있는 urls.py 파일을 의미한다.
* **APP_URLCONF**: URL 패턴에서 애플리케이션을 식별하는 첫 단어를 제외한 그 이후 단어들을 인식해서 해당 뷰를 매핑하기 위한 URLCONF이다. 각 애플리케이션 디렉터리에 있는 urls.py 파일을 의미한다.

---

먼저 ROOT_URLCONF인 projects/urls.py 파일을 다음과 같이 코딩한다.
* projects/urls.py
```python
from django.contrib import admin
from django.urls import path, include               # include 추가
#from bookmark.views import BookmarkLV, BookmarkDV  # 삭제 ----- # 1


urlpatterns = [
    path('admin/', admin.site.urls),
    path('bookmark/', include('bookmark.urls')),    # 추가 ----- # 2
    path('blog/', include('blog.urls')),            # 추가 ----- # 3
    
    # 기존 2개 라인 삭제
    #path('', BookmarkLV.as_view(), name='index'),  
    #path('<int:pk>/', BookmarkDV.as_view(), name='detail'), 
]
```
코드 설명은 다음과 같다.
* **# 1**: 기존 줄 중 APP_URLCONF로 옮길 줄은 삭제(주석 처리)한다.
* **# 2**: **[include()](#-include)** 함수를 사용하여, 북마크 앱의 APP_CONF로 처리를 위임한다.
* **# 3**: include() 함수를 사용하여, 북마크 앱의 APP_CONF로 처리를 위임한다.






























# 개념 정리
## ✅ reverse()
reverse()를 사용하면 **urls.py**에서 설정한 URL의 **name**이나, viewname을 통해서 다시 URL로 되돌릴 수 있다.
* urls.py
```python
from news import views

path('archive/', views.archive, name='news-archive')
```
* models.py
```python
# name 사용 시
reverse('news-archive')

# viewname 사용 시
from news import views
reverse(views.archive)
```
인수가 있는 URL이라면 다음과 같이 **args**를 포함할 수 있다.
```python
from django.urls import reverse

def myview(request):
    return HttpResponseRedirect(reverse('arch-summary', args=[1945]))
```
**kwargs**로 전달하는 것 또한 가능하다. 하지만 **args**와 **kwargs**를 동시에 전달할 수는 없다.
```python
>>> reverse('admin:app_list', kwargs={'app_label': 'auth'})
'/admin/auth/'
```
일치하는 URL이 없으면 **NoReverseMatch**가 발생한다.

### References
* https://ugaemi.github.io/django/Django-reverse-and-resolve/

<br>

## ✅ slug
slug는 URL의 구성요소로 웹사이트의 특정 페이지를 가리키는 사람이 읽기 쉬운 형식의 식별자이다. 장고에서는 SlugField로 slug를 지원한다.

다음과 같이 모델을 만들어보자.
```python
class Article(models.Model):
   title = models.CharField(max_length=100)
   content = models.TextField(max_length=1000)
   slug = models.SlugField(max_length=40)
```
어떻게 이 객체를 의미있는 이름으로 URL에서 참조할까? `Article.id`와 같은 방법으로 참조할 수 있다.
```
www.example.com/article/23
```
또는 다음과 같이 title로 접근할 수도 있을 것이다.
```
www.example.com/article/The 46 Year Old Virgin
```
공백이 포함되면 유효하지 않은 URL이므로 %20으로 치환될 것이고, 굉장히 복잡해 보인다.
```
www.example.com/article/The%2046%20Year%20Old%20Virgin
```
따라서 다음이 훨씬 나을 것이다.
```
www.example.com/article/the-46-year-old-virgin
```
이게 바로 slug 이다. `the-46-year-old-virgin`. 모든 문자가 소문자화 되어 있고, 공백은 `-`로 교체되어 있다.

### References
* https://itmining.tistory.com/119
* https://django-orm-cookbook-ko.readthedocs.io/en/latest/slugfield.html

<br>

## ✅ Django 모델의 Meta 클래스
메타 데이터는 다른 데이터에 대한 정보를 제공하는 특정 데이터 집합을 나타낸다. Django에서는 Django 모델을 사용하여 데이터베이스의 테이블과 해당 필드를 설계한다. 모델 자체에 대한 데이터를 추가해야하는 경우 **Meta 클래스**를 사용한다. 

**Meta 클래스**는 내부 클래스이다. 즉, 다음과 같이 모델 내부에 정의된다.
```python
from django.db import models

class MyModel(models.Model):
    ...
    class Meta:
        ...
```
**Meta 클래스**는 권한, 데이터베이스 이름, 단/복수 이름, 추상화, 순서 지정 등과 같은 모델에 대한 다양한 사항을 정의하는 데 사용할 수 있다. 
> ※ Django 모델에 Meta 클래스를 추가하는 것은 전적으로 선택 사항이다.

이 클래스에는 구성할 수 있는 많은 옵션도 제공된다. 다음은 일반적으로 사용되는 몇 가지 메타 옵션이다. **[여기](https://docs.djangoproject.com/en/3.0/ref/models/options/)** 에서 모든 메타 옵션을 탐색 할 수 있다.

### 👉 db_table
이 옵션은 데이터베이스 내에서 테이블을 식별하는 데 사용해야하는 이름을 설정하는 데 사용된다. 예를 들어 다음과 같은 작업을 수행하면 데이터베이스에서 모델 이름이 `job`이 된다.
```python
from django.db import models

class JobPosting(models.Model):
    
    class Meta:
        db_table = "job"
```

### 👉 ordering
이 옵션은 모델 객체의 순서를 정의하는 데 사용된다. 이 모델의 객체가 검색되면 이 순서대로 표시된다.
```python
from django.db import models

class JobPosting(models.Model):
    dateTimeOfPosting = models.DateTimeField(auto_now_add = True)
    
    class Meta:
        ordering = ["-dateTimeOfPosting"]
```
위의 예에서 검색된 객체는 `dateTimeOfPosting`필드를 기준으로 내림차순으로 정렬된다. (`-`접두사는 내림차순을 정의하는 데 사용된다)

### 👉 verbose_name
이 옵션은 사람이 읽을 수있는 모델의 단일 이름을 정의하는 데 사용되며 Django의 기본 명명 규칙을 덮어 쓴다. 이 이름은 관리자 패널 (`/admin/`)에도 반영된다.
```python
from django.db import models

class JobPosting(models.Model):
    
    class Meta:
        verbose_name = "Job Posting"
```

### 👉 vervose_name_plural
이 옵션은 모델에 대해 사람이 읽을 수있는 복수형 이름을 정의하는 데 사용되며 Django의 기본 명명 규칙을 덮어 쓴다. 이 이름은 관리자 패널 (`/admin/`)에도 반영된다.
```python
from django.db import models

class JobPosting(models.Model):
    
    class Meta:
        verbose_name_plural = "Job Postings"
```


### References
* https://www.delftstack.com/ko/howto/django/class-meta-in-django/

<br>

## ✅ get_absolute_url()
> 어떠한 모델에 대해서 detail 뷰를 만들게 되면 get_absolute_url() 멤버 함수를 무조건 선언!

get_absolute_url()는 reverse 함수를 통해 모델의 개별 데이터 url을 문자열로 반환한다.

### 👉 예시
아래와 같은 모델이 있다.

방에 대한 모델인데, db에 수많은 방에 대한 레코드들 중에서 특정 방에 대한 접근을 하려면 그 URL을 하드코딩한다는게 상당히 피곤한 일일 것이다.
```
http://127.0.0.1:8000/admin/rooms/room/480/change/
```
481, 482.. 이렇게 방의 PK가 있는걸 유추해 낼 수 있다.

이때 **get_absolute_url()** 메소드는 내가 원하는 모델을 찾을 수 있는 url을 준다.
```python
def get_absolute_url(self):
    return reverse('rooms:detail', kwargs={'pk':self.pk})
```
`rooms`는 namespace 부분이고 `detail`은 name 부분이다.<br>
`kwargs`는 `'pk'`가 url에 입력했던 부분이므로 그대로 스트링으로 입력하고 `self.pk` 인스턴스의 필드인 pk를 입력해주면 끝나게 된다.

이 방법은 어느 한 페이지에만 국한되는 게 아니라 거의 대부분의 페이지에서 이렇게 작동하고, 특히 Detail 페이지로 넘어가기 위해 자주 사용되는 코드이니 꼭 숙지하자!

### 👉 url template tag로 활용 예시
기존 {% url %} template 태그방식은 주석 처리 하고, get_absolute_url로 대체하였다.
```html
<!-- <a class="nav-link" href="{% url 'blog:blog_detail' list.id %}"> -->
<a class="nav-link" href="{{ list.get_absolute_url }}">
```

### References
* https://wayhome25.github.io/django/2017/05/05/django-url-reverse/
* https://velog.io/@hyeseong-dev/django-getabsoluteurl

<br>

## ✅ get_previous_by_FOO(\*\*kwargs), get_next_by_FOO(\*\*kwargs)
필드 타입이 DateField 또는 DateTimeField면서 필드 옵션이 null=True가 아닌 경우에는 이 메소드를 사용할 수 있다. FOO 자리에는 필드명이 들어가면 되고, 필요하면 키워드 인자를 딕셔너리 형식으로 전달할 수 있다. 

### 👉 get_previous_by_FOO(\*\*kwargs)
위에서 예시로 든 `Post` 클래스의 `get_previous`는 다음과 같이 정의되어 있다.
```python
def get_previous(self): 
    return self.get_previous_by_modify_dt()
```
`get_previous()`는 `modify_dt`라는 `DateTimeField`를 기준으로 이전 객체를 반환한다. 이전 객체가 없는 경우에는 `DoesNotExist` 익셉션을 발생시킨다.

### 👉 get_next_by_FOO(\*\*kwargs)
`get_previous_by_FOO(**kwargs)`와 다른 점은 다음 객체를 반환한다는 점이다. 나머지는 모두 동일하다.

### References
* https://sys09270883.github.io/web/56/#get_next_by_fookwargs

<br>

## ✅ include()
django를 쓸 때, 프로젝트의 url을 **include**를 써서 고정적인 url을 쉽게 관리하거나, 앱별로 url을 관리 할 수 있는 강력한 기능이 있다.

### 👉 예시1
`projects/urls.py` 파일에 `blog.urls`를 가져오는 행을 추가해 보자. <br>
`blog.urls`를 가져오려면, `include` 함수가 필요하다.

이제 `projects/urls.py` 파일은 아래처럼 보일 것이다.
* projects/urls.py
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```
장고는 http://127.0.0.1:8000/ 로 들어오는 모든 접속 요청을 `blog.urls`로 전송해 추가 명령을 찾을 것이다.
> ※ 이때 `blog.urls`는 blog 앱 안에 있는 urls 파일을 의미한다.


#### blog.urls
아래 두 줄을 추가하자.
* blog/urls.py
```python
from django.urls import path
from . import views
```
여기서 장고 함수인 `path`와 `blog` 애플리케이션에서 사용할 모든 `views`를 가져왔다.

그 다음, URL 패턴을 추가하자.
* blog/urls.py
```python
urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```
이제 `post_list`라는 `view`가 루트 URL에 할당되었다. 이 URL 패턴은 빈 문자열에 매칭이 되며, 장고 URL 확인자(resolver)는 전체 URL 경로에서 접두어(prefix)에 포함되는 도메인 이름(i.e. http://127.0.0.1:8000/) 을 무시하고 받아들인다. 이 패턴은 장고에게 누군가 웹사이트에 'http://127.0.0.1:8000/' 주소로 들어왔을 때 `views.post_list`를 보여주라고 말해준다.<br>
`name='post_list'`는 URL에 이름을 붙인 것으로 뷰를 식별한다. 

### 👉 예시2
프로젝트 폴더의 urls.py는 다음과 같다.
 
* projects/urls.py
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('service/', include('service.urls')),
    path('review/', include('review.urls')),
]
```

review 앱의 urls.py를 수정해보자.

작성할 URL 경로는 다음 2가지이며,
* 127.0.0.1:8000/review
* 127.0.0.1:8000/review/first

각 URL에서 사용할 함수명은 다음과 같다.
* home
* first

<br>

* review/urls.py
```python
from django.urls import path, include
from review import views

urlpatterns = [
    # 127.0.0.1:8000/review 에 해당
    path('', views.home),  # home 함수 실행
    
    # 127.0.0.1:8000/review/first 에 해당
    path('first/', views.first),  # first 함수 실행
]
```

<br>

* review/views.py
```python
from django.shortcuts import render

def home(request):
    return render(request, "home.html")
    
def first(request):
    return render(request, "first.html")
```

### References
* https://tutorial.djangogirls.org/ko/django_urls/
* https://0ver-grow.tistory.com/906




























