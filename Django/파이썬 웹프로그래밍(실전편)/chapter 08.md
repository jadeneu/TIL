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
* **# 7**: 템플릿에 넘겨줄 컨텍스트 변수 context를 사전 형식으로 정의한다.
* **# 8**: form 객체, 즉 PostSearchForm 객체를 컨텍스트 변수 form에 지정한다.
* **# 9**: 검색용 단어 searchWord를 컨텍스트 변수 search_term에 지정한다.
* **# 10**: 검색 결과 리스트인 post_list를 컨텍스트 변수 object_list에 지정한다.
* **# 11**: 단축 함수 **[render()](#-render)** 는 템플릿 파일과 컨텍스트 변수를 처리해, 최종적으로 HttpResponse 객체를 반환한다. form_valid() 메소드는 보통 리다이렉트 처리를 위해 HttpResponseRedirect 객체를 반환한다. 여기서는 form_valid() 메소드를 재정의하여 render() 함수를 사용함으로써, HttpResponseRedirect가 아니라 HttpResponse 객체를 반환한다. 즉 리다이렉트 처리가 되지 않는다.



























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
```sql
SELECT * FROM Account
WHERE user_account='abc';
```
#### Python Django ORM
```python
Account.objects.filter(user_account='abc')
```
* **filter() 메소드와 같이 쓰였던 exists()**<br>
  해당 메소드는 DB에서 filter를 통해 원하는 조건의 데이터가 유무에 따라 True, False를 반환하는 메소드이다. queryset은 아니다. (`type()`함수 등을 통해 확인 가능) 어떤 특정 조건에 대해서 이벤트나 로직을 처리할 때 많이 쓰임.
* **get() 메소드**<br>
  해당 메소드는 queryset이 아닌 모델 객체를 반환하는 함수이다. 특정 column 조건에 해당하는 결과를 객체로 반환하는 함수로 찾는 결과가 없다면 DoesNotExist 예외를 발생시킨다. 따라서 해당 메소드를 이용하여 원하는 결과를 가져오고자 할 때에는

  `try-except` 구문등으로 예외처리를 해주는 것이 좋다.
  ```python
  # try-except 사용예제
  try:
      if Account.objects.filter(user_account=account_data['user_account']).exists():
          account = Account.objects.get(user_account=account_data['user_account'])

          if account.password == account_data['password']:
              return JsonResponse({'message':'Welcome back!'}, status=200)
          return HttpResponse(status=401)

       return HttpResponse(status=400)

    except KeyError:
        return HttpResponse(status=400)
  ```
  
<br>

### 👉 UPDATE
아래 메소드의 결과들은 모두다 같은 효과를 지니게 된다.
```python
>>> Account.objects.create(user_account='ghi', password=5678)
```
```python
>>> Account(
		user_account='ghi',
		password=5678,
).save()
```
둘다 모델 객체를 생성하여 실제 DB에 UPDATE를 진행하는 방법으로 알고 있으면 된다.

<br>

### 👉 ETC
기타 다른 queryset이나 그에 해당하는 SQL 구문과 관련된 내용은 [django queryset 공식문서](https://docs.djangoproject.com/en/4.0/ref/models/querysets/)를 참고하자.

### References
* https://velog.io/@ybear90/Django-Django-ORM-queryset-%EC%A0%95%EB%A6%ACmodel-filter-all-get-filter-exists-create-save

<br>

## ✅ render()
#### render
```python
render(request, template_name, context=None, content_type=None, status=None, using=None)
```
`render` 는 다음과 같은 파라미터들을 가진다. 이 중에서 `request` 와 `template_name` 은 필수적으로 필요하다. request 는 위와 동일하게 써주게 되고, template_name 은 불러오고 싶은 템플릿을 기재해준다. 쉽게 생각해서 화면에 html 파일을 띄운다고 생각하면 된다. 이 때 `context` 로 원하는 인자를, 즉 view 에서 사용하던 파이썬 변수를 html 템플릿으로 넘길 수 있다. context 는 딕셔너리형으로 사용하며 key 값이 템플릿에서 사용할 변수이름, value 값이 파이썬 변수가 된다.
```python
# views.py

from django.shortcuts import render

def my_view(request):
    name = "minsung"
    return render(request, 'myapp/index.html', {
        'name': name,
    }
```

### References
* https://ssungkang.tistory.com/entry/Django-render-%EC%99%80-redirect-%EC%9D%98-%EC%B0%A8%EC%9D%B4




















