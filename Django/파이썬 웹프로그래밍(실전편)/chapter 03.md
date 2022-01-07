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

* **# 4**: 




























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


















