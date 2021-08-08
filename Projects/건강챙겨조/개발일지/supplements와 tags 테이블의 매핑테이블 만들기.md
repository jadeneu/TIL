## 구현하게 된 과정
영양제 정보를 담는 `supplements` 테이블과 증상별 태그 이름을 담는 `tags` 테이블을 연결짓는 `supplement_tags` 테이블에 row를 insert 해야 했다. `supplements` 테이블은 크롤링을 하면서 내용을 모두 채운 상태였고, `tags`의 태그 이름과 `supplement_tags`의 row들을 insert 해서 기능(증상) 분류를 했다.

<br><br>

## 구현한 방법
### 1. `tags` 테이블 insert
태그 이름은 아래 사이트에서 착안해서 정할 수 있었다.<br>
http://www.foodsafetykorea.go.kr/portal/healthyfoodlife/functionality.do?menu_grp=MENU_NEW01&menu_no=2657

태그 이름을 정해서 `tags` 테이블에 insert 해주었다.

* sql 스크립트
```sql
LOCK TABLES `TAGS` WRITE;
/*!40000 ALTER TABLE `TAGS` DISABLE KEYS */;
INSERT INTO `TAGS` VALUES
('장 건강'),
('혈당조절'),
('콜레스테롤 개선'),
('면역기능'),
('항산화'),
('피부건강'),
('혈압조절'),
('혈행 개선'),
('기억력 개선'),
('간 건강'),
('눈 건강'),
('전립선 건강'),
('칼슘 흡수'),
('치아 형성'),
('피로개선'),
('갱년기 남성'),
('갱년기 여성'),
('위 건강'),
('정자 운동성'),
('어린이'),
('수면'),
('근력');
/*!40000 ALTER TABLE `TAGS` ENABLE KEYS */;
UNLOCK TABLES;
```
<br>

테이블에 잘 insert 되었다.

<img src="https://user-images.githubusercontent.com/55045377/128626047-cd3c68aa-c38e-443a-b2ab-2f34a109a8a5.png" width=40% height=40%>  

<br><br>

### 2. `supplement_tags` 테이블 insert
`supplement_tags` 테이블의 컬럼은 아래와 같다.

<img src="https://user-images.githubusercontent.com/55045377/128626270-6c2d1da4-b267-4102-a3cd-8e1798618cab.png" width=90% height=90%>

두 컬럼 모두 `supplement_tags` 테이블의 pk이다.

또한, 각각 `tags`, `supplements` 테이블의 fk이었기 때문에 `tag_name`과 `supplement_id`는 `tags`와 `supplements` 테이블로부터 row 값을 가져와야 했다.



























