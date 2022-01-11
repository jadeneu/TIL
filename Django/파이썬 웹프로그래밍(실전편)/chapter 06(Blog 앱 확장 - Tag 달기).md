# 목차

# chapter 06 Blog 앱 확장 - Tag 달기
6장에서는 각 포스트마다 태그를 달 수 있는 기능을 개발한다. 태그를 달고 태그별로 포스트의 리스트를 보여주며 태그 클라우드를 만드는 방법도 소개한다.

블로그 앱에서 태그 기능은 거의 필수라고 여겨지고 있어, 태그 기능을 제공하는 오픈 소스도 다양하다. 이 중 가장 많이 사용하는 django-taggit 패키지를 선택할 것이며, 여기에 템플릿 태그 기능 및 태그 클라우드 기능이 추가된 django-taggit-templatetags2 패키지도 같이 사용할 예정이다.

이번 6장을 통해 오픈 소스를 활용하는 방법을 익혀두자!

## 6.1 애플리케이션 설계하기
이미 만들어 둔 블로그 앱에 태그 기능을 추가해보자. 블로그의 각 포스트마다 태그를 보여주고 해당 태그를 클릭하는 경우, 그 태그를 가진 모든 포스트의 리스트를 보여준다. 그리고 태그만 모아서 보여주는 태그 클라우드 기능도 개발한다.

## 6.2 개발 코딩하기
오픈 소스로부터 설치한 taggit 앱을 등록하고, taggit 앱에서 제공하는 기능을 사용하기 위해 관련 파일들을 수정한다.

### 모델 코딩하기
기존 Post 테이블에 tags 필드만 추가하면 된다.
* blog/models.py
```python
from django.db import models
from django.urls import reverse
from taggit.managers import TaggableManager

# 중간 내용 동일
    create_dt = models.DateTimeField('CREATE DATE', auto_now_add=True)
    modify_dt = models.DateTimeField('MODIFY DATE', auto_now=True)
    tags = TaggableManager(blank=True)
# 하단 내용 동일
```

<br>

포스트별로 태그 이름이 어드민 화면에 나타나도록 admin.py 파일을 변경한다.
* blog/admin.py - tags 추가
```python
from django.contrib import admin
from blog.models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'modify_dt', 'tag_list')  # 1
    list_filter = ('modify_dt',)
    search_field = ('title', 'content')
    prepopulated_fields = {'slug': ('title',)}
    
    # 두 메소드 추가
    def get_queryset(self, request):  # 2
        return super().get_queryset(request).prefetch_related('tags')
    
    def tag_list(self, obj):  # 3
        return ', '.join(o.name for o in obj.tags.all())
```
태그 관련 코드가 추가되었다. 코드 설명은 아래와 같다.
* **# 1**: tags 필드에 정의된 TaggableManager 클래스는 list_display 항목에 직접 등록할 수 없으므로, 별도로 tag_list 항목을 메소드로 정의해서 등록한다.
* **# 2**: Post 레코드 리스트를 가져오는 **[get_queryset()](#-get_queryset)** 메소드를 오버라이딩한다. 그 이유는 Post 테이블과 Tag 테이블이 ManyToMany 관계이므로, Tag 테이블의 관련 레코드를 한 번의 쿼리로 미리 가져오기 위함이다. 이렇게 N:N 관계에서 쿼리 횟수를 줄여 성능을 높이고자 할 때 **[prefetch_related()](#-prefetch_related)** 메소드를 사용한다는 점에 유의하자.
* **# 3**: tag_list 항목에 보여줄 내용을 정의하고 있다. 각 태그에는 name 필드가 있는데, name 필드의 값들을 ', '로 연결하여 보여주도록 한다.

<br>

taggit 패키지에는 자체 테이블이 정의되어 있어서 makemigrations/migrate 명령을 실행하면 tags 컬럼만 추가되는 것이 아니라 새로운 2개의 테이블, Tag과 TaggedItem이 데이터베이스에 추가된다.


























# 개념 정리
## ✅ super()
**super()** 는 자식 클래스에서 부모 클래스의 내용을 사용하고 싶을 경우에 사용한다.

### 👉 예시
```python
class father():  # 부모 클래스
    def handsome(self):
        print("잘생겼다")
 
 
class brother(father):  # 자식클래스(부모클래스) 아빠매소드를 상속받겠다
    '''아들'''
 
 
class sister(father):  # 자식클래스(부모클래스) 아빠매소드를 상속받겠다
    def pretty(self):
        print("예쁘다")
 
    def handsome(self):
        '''물려받았어요'''
 
 
brother = brother()
brother.handsome()
 
girl = sister()
girl.handsome()
girl.pretty()
```
결과
```
잘생겼다
예쁘다
```
원래대로라면 세 줄이 출력되어야 한다. 위 결과는 오버라이딩 때문에 두 줄만 출력이 된 것이다.

이제 sister에서도 '잘생겼다'가 출력되게 해보자.
```python
class father():  # 부모 클래스
    def handsome(self):
        print("잘생겼다")
 
 
class brother(father):  # 자식클래스(부모클래스) 아빠매소드를 상속받겠다
    '''아들'''
 
 
class sister(father):  # 자식클래스(부모클래스) 아빠매소드를 상속받겠다
    def pretty(self):
        print("예쁘다")
 
    def handsome(self):
       super().handsome()
 
 
brother = brother()
brother.handsome()
 
girl = sister()
girl.handsome()
girl.pretty()
```
결과
```
잘생겼다
잘생겼다
예쁘다
```
잘 출력되는 걸 볼 수 있다. super를 사용해서 상속받은 handsome에 있는 잘생겼다를 출력했다.

### References
* https://rednooby.tistory.com/56

<br>

## ✅ get_queryset()
**get_queryset()** 은 request가 발생할 때마다 동작합니다.

* 예시
```python
class ListView(ListAPIView):
    def get_queryset(self):
        return order.objects.filter(created_date=date.today())  # request 마다 date.today() 실행 됨
```

### References
* https://coblin.xyz/20

<br>

## ✅ prefetch_related()
**prefetch_related**는 하나의 QuerySet을 가져올 때, 미리 related objects들까지 다 불러와주는 함수이다.<br>

* query를 복잡하게 만들긴 하지만, 그렇게 불러온 data들은 모두 cache에 남아있게 되므로 DB에 다시 접근해야 하는 수고를 덜어줄 수 있다. 이렇게 DB에 접근하는 수를 줄여, performance를 향상시켜준다.
* prefetch_related는 foreign-key, one-to-one 뿐만 아니라 many-to-many, many-to-one 등 모든 relationships에서 사용 가능하다.

### 👉 예시1
아래와 같은 모델이 있다고 가정해보자.
```python
from django.db import models
 
 
class Country(models.Model):
 
    name = models.CharField(
        max_length=10,
    )
 
    def __str__(self):
        return self.name
 
 
class Person(models.Model):
 
    city = models.ForeignKey(City)
     
    name = models.CharField(
        max_length=10,
    )
 
    def __str__(self):
        return self.name
         
         
class Pet(models.Model):
 
    person = models.ForeignKey(Person)
     
    name = models.CharField(
        max_length=10,
    )
 
    def __str__(self):
        return self.name
```

<br>

다음과 같이 Person 모델과 N:M 관계를 가지는 Language라는 모델을 하나 추가했다.
```python
class Language(models.Model):
 
    person_set = models.ManyToManyField(Person)
 
    name = models.CharField(
        max_length=10,
    )
 
    def __str__(self):
        return self.name
```
여기서 Person의 모든 instance들과 그 instance들의 language_set들을 아래와 같이 모두 출력해야 한다고 가정하자.

**Tom: Python Ruby<br>
Peter: Python Node.js Java<br>
John: Java C++ php<br>**

두 가지 방법을 사용했다.<br>
1.
```python
# No use prefetch_related()
 
people = Person.objects.all()
for person in people:
    print(person.name+" : ", end="")
    for language in person.language_set.all():
        print(language.name+" ", end="")
    print("")
```
2.
```python
# Use prefetch_related()
 
people = Person.objects.all().prefetch_related('language_set')
for person in people:
    print(person.name+" : ", end="")
    for language in person.language_set.all():
        print(language.name+" ", end="")
    print("")
```

* 첫번째 경우에서는 `Person.objects.all()` 안에 있는 person마다 `person.language_set.all()` 이라는 query가 실행이 된다. 즉, person이 100개 있다면 `person.language_set.all()` 이라는 query가 DB로 100번 날라간다는 의미이다.
* 하지만 두번째 경우에서처럼 **prefetch_related**를 쓰게 된다면, 이 부분을 엄청나게 효율적으로 개선할 수 있다. Person.objects.all()라는 query가 동일하게 실행됨과 동시에 self.language_set.all() 이라는 query가 별도로 실행돼 받아온 data들이 cache에 저장되게 된다. 그래서 person의 수 만큼 person.language_set.all()이 실행되더라도 DB에 접근하지 않고 cache에서 찾아서 쓰게 된다.

### 👉 예시2
https://wave1994.tistory.com/70 참고.




### References
* https://jupiny.tistory.com/entry/selectrelated%EC%99%80-prefetchrelated
* https://wave1994.tistory.com/70

























