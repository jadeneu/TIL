Question 모델의 컬럼들에 값을 저장하고 `save()`를 했더니 아래와 같은 에러가 발생했다.
```
django.db.utils.OperationalError: table blog_question has no column named subject
```
코드들을 봐도 이상한 점을 찾을 수 없다면 초기화하는 방법을 써보자!

1. 프로젝트 내 app 디렉토리 내 migrations 폴더 안에 0001과 같이 migrate 관련 파일만 싹 다 지워준다. (__ init__.py 제외)
2. app이름/migrations/__ pycache__ 로 들어가서 위에서 지운 파일과 비슷한 이름들은 다 지워준다. 여기에서도 init이 들어간 파일은 제외한다.
3. 프로젝트 내 db.sqlite3 파일도 지워준다.
4. 터미널에 프로젝트 디렉토리로 들어간 후에 `python manage.py showmigrations` 입력했을때 목록 앞에 [ ] 이부분이 다 비워져있으면 초기화가 된 것이고 다시 처음부터 `makemigrations`, `migrate` 해주면 된다.

