# 목차
* [chapter 08 Blog 앱 확장 - 검색 기능](#chapter-08-blog-앱-확장---검색-기능)

<br><br>

---

<img src="https://user-images.githubusercontent.com/55045377/149664393-77e068f5-a467-45dc-8fd3-a3f30bae0bcc.png" width=40%>

<br><br>

# chapter 08 Blog 앱 확장 - 검색 기능
## 8.1 애플리케이션 설계하기
8장의 검색 기능은 블로그 앱 내에서의 검색 기능을 구현하는 것으로, 이 정도의 기능은 장고 자체의 Q-객체를 이용하면 어렵지 않게 구현할 수 있다. 장고의 Q-객체는 테이블에 대한 복잡한 쿼리를 처리하기 위한 객체이다.

검색 기능을 위해서, 검색 단어를 입력받는 폼 기능 및 Q-객체를 사용해 검색 단어가 들어 있는 블로그를 찾고 그 결과를 보여주는 기능을 개발한다.

## 8.2 개발 코딩하기
### URLconf 코딩하기
* blog/urls.py
```python
    # Example: /blog/tag/tagname/
    path('tag/<str:tag>/', views.TaggedObjectLV.as_view(), name='tagged_object_list'),
    # 상단 내용 동일

    # Example: /blog/search/
    path('search/', views.SearchFormView.as_view(), name='search'),  # 1
]

```
코드 설명은 다음과 같다.
* **# 1**: URL /blog/search/ 요청을 처리할 뷰 클래스를 SearchFormView로 지정한다. URL 패턴의 이름은 이름공간을 포함해 'blog:search'가 된다. SearchFormView 클래스형 뷰는 폼을 보여주고 폼에 들어 있는 데이터를처리하기 위한 뷰이므로, **[FormView](#-formview)** 를 상속받아 정의한다.

<br>

### 뷰 코딩하기
* blog/forms.py
```python
from django import forms  # 1


class PostSearchForm(forms.Form):  # 2
    search_word = forms.CharField(label='Search Word')  # 3
```
코드 설명은 다음과 같다.
* **# 1**: 장고는 폼을 클래스로 표현할 수 있도록 하는 기능을 django.forms 모듈에서 제공합니다. 이 모듈을 임포트한다.
* **# 2**: 폼을 정의하기 위해서는 django.forms 모듈의 Form 클래스를 상속받아 클래스를 정의하면 된다.
* **# 3**: 폼을 정의하는 방법은 테이블의 모델 클래스를 정의하는 방법과 매우 유사하다. CharField 필드는 TextInput 위젯으로 표현되며, label 인자인 Search Word는 폼 위젯 앞에 출력되는 레이블이 되고, 변수 search_word는 input 태그에 대한 name 속성이 되어 사용자가 입력한 값을 저장하는 데 사용된다. 결국 HTML 요소 \<input type="text"\> 하나를 만든 것이다.

<br>

* blog/views.py
```python
from django.views.generic import ListView, DetailView, TemplateView
from django.views.generic import ArchiveIndexView, YearArchiveView, MonthArchiveView
from django.views.generic import DayArchiveView, TodayArchiveView
from django.conf import settings

from blog.models import Post

from django.views.generic import FormView  # 추가
from blog.forms import PostSearchForm      # 추가
from django.db.models import Q             # 추가
from django.shortcuts import render        # 추가

(중략)

# 상단 내용 동일
# 파일 끝에 아래 클래스형 뷰 추가


#--- FormView
class SearchFormView(FormView):  # 1
    form_class = PostSearchForm   # 2
    template_name = 'blog/post_search.html'  # 3

    def form_valid(self, form):  # 4
        searchWord = form.cleaned_data['search_word']  # 5
        post_list = Post.objects.filter(Q(title__icontains=searchWord) |  Q(description__icontains=searchWord) | Q(content__icontains=searchWord)).distinct()  # 6

        context = {}  # 7
        context['form'] = form  # 8
        context['search_term'] = searchWord  # 9
        context['object_list'] = post_list  # 10

        return render(self.request, self.template_name, context)   # No Redirection  # 11
```
코드 설명은 아래와 같다.
* **# 1**: FormView 제네릭 뷰를 상속받아 SearchFormView 클래스형 뷰를 정의한다. FormView 제네릭 뷰는 GET 요청인 경우 폼을 화면에 보여주고 사용자의 입력을 기다린다. 사용자가 폼에 데이터를 입력한 후 제출하면 이는 POST 요청으로 접수되며 FormView 클래스는 데이터에 대한 유효성 검사를 한다. 데이터가 유효하면 form_valid() 함수를 실행한 후에 적절한 URL로 리다이렉트시키는 기능을 갖고 있다.
* **# 2**: 폼으로 사용될 클래스를 PostSearchForm으로 지정한다.
* **# 3**: 템플릿 파일을 지정한다. 템플릿 파일은 다음 절에서 코딩할 예정이다.
* **# 4**: POST 요청으로 들어온 데이터의 유효성 검사를 실시해 에러가 없으면 form_valid() 메소드를 실행한다.
* **# 5**: 유효성 검사를 통과하면, 사용자가 입력한 데이터들은 cleaned_data 사전에 존재한다. 이 사전에서 search_word 값을 추출해 searchWord 변수에 지정한다. search_word 키는 PostSearchForm 클래스에서 정의한 필드 이름이다.
* **# 6**: Q 객체는 filter() 메소드의 매칭 조건을 다양하게 줄 수 있도록 한다. 예제에서는 3개의 조건을 OR 문장으로 연결하고 있다. 각 조건의 icontains 연산자는 대소문자를 구분하지 않고 단어가 포함되어 있는지 검사한다. distinct() 메소드는 중복된 객체를 제외한다. 즉 이 줄의 의미는 Post 테이블의 모든 레코드에 대해 title, description, content 컬럼에 searchWord가 포함된 레코드를 대소문자 구별 없이 검색하고 서로 다른 레코드들만 리스트로 만들어서 post_list 변수에 지정한다.



























# 개념 정리
## ✅ FormView
FormView는 
* Get, Post 요청을 모두 알아서 처리한다.
* form을 보여주고 template을 rendering을 해준다.

### References
* https://ggodong.tistory.com/209

<br>

## ✅ Django ORM queryset 정리
### 👉 CREATE TABLE
#### SQL
```sql
CREATE TABLE "user_account" (
  "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
  "user_account" varchar(50) NOT NULL,
  "password" varchar(300) NOT NULL,
  "created_at" datetime NOT NULL,
  "updated_at" datetime NOT NULL
);
```
#### Python Django ORM
```python
class Account(models.Model):
    user_account    = models.CharField(max_length=50)
    password        = models.CharField(max_length=300)
    created_at      = models.DateTimeField(auto_now_add=True)
    updated_at      = models.DateTimeField(auto_now=True)

    class Meta:
        db_table = 'user_account'
```

<br>

### 👉 SELECT
#### Select all rows
#### SQL
```sql
SELECT * FROM user_account;
```
#### Python Django ORM
```python
accounts = Account.objects.all()

for account in accounts:
  	print(account.user_account)
    print(account.password)
    print(account.created_at)
    print(account.updated_at)
```
#### WHERE Clause
#### SQL





















