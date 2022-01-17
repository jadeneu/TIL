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
* **# 1**: URL /blog/search/ 요청을 처리할 뷰 클래스를 SearchFormView로 지정한다. URL 패턴의 이름은 이름공간을 포함해 'blog:search'가 된다. SearchFormView 클래스형 뷰는 폼을 보여주고 폼에 들어 있는 데이터를처리하기 위한 뷰이므로, [FormView](#-formview)를 상속받아 정의한다.



























# 개념 정리
## ✅ FormView
FormView는 
* Get, Post 요청을 모두 알아서 처리한다.
* form을 보여주고 template을 rendering을 해준다.

### References
* https://ggodong.tistory.com/209

<br>






















