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

그래서 처음 생각한 로직이 다음과 같다.
```
태그를 선택하면 불러오는 영양제들

1. effect 에 해당 태그 단어가 있는 경우
2. supplement_name에 해당 태그 단어가 있는 경우

but 가장 우선순위인 제품은 1,2번 모두 해당하는 경우임
```
여기서 effect는 영양제의 효능을 나타내는 `supplements` 테이블 컬럼 effect를 말한다.

<br>

로직은 정했고, row 값을 채우는 방법을 고안해내야 했다.<br>
**(1)처음에는 `IF`를 사용할까 생각했다.**
* `IF` 사용 로직
```
if tag_name이 effect에 포함된다면 or 제목에 포함된다면 
그 태그 이름과 영양제 id 보여주기
```
그러나 `IF`보다는 더 광범위한 조건을 포함할 수 있는 조건문이 필요했다.

그래서 생각해낸 것이 **(2)`WHERE`문에 `LIKE`를 사용하는 것이었다.**<br>
```
1. tag 이름 전체를 supplements.effect에서 찾아내서 select

SELECT id, supplement_name, effect, name, count(*) FROM supplements, tags
WHERE effect LIKE CONCAT('%',name,'%')
GROUP BY name
```
`supplements`와 `tags` 테이블을 불러와서 컬럼들을 열거시키고 그 중에서 effect가 `tags`의 name 컬럼 row 값을 포함시키는 경우만 select하게 했다. 또한, name으로 그루핑을 하여 name을 기준으로 row들을 묶었다.

이렇게 해서 생기는 특징은 다음과 같았다.
```
-> 검색결과가 제한적이기 때문에 무분별한 정보가 없음. but 우리가 갖고있는 정보량에
비해 적은 영양제가 추출돼서 아쉬운 부분이 있음. 태그 개수가 18개임.
```
만약 `tags`의 name 컬럼의 row 값이 `치아 건강`이라면 저 단어가 포함된 effect 값만 불러오고, `치아`가 포함된 내용은 불러오질 못해 생각보다 적은 영양제가 추출되었다.<br>
그래서 또 다른 방법을 생각해 냈다.
```
2. tag 이름 중 띄어쓰기 앞 단어를 supplements.effect에서 찾아내서 select

SELECT id, supplement_name, effect, name, count(*) FROM supplements, tags
WHERE effect LIKE CONCAT('%',(SELECT SUBSTRING_INDEX(name, ' ', 1)),'%')
GROUP BY name
```
`tags` 테이블의 name 컬럼 row 값들의 앞단어만 추출하여 `supplements` 테이블의 effect 컬럼 row를 검색하는 것이었다.<br>
이렇게 해서 생기는 특징은 다음과 같았다.
```
-> 다양한 태그 검색 가능, but '위 건강/소화기능' 이나 '장 건강' 같은 경우는
'위', '장' 을 포함하는 effect를 뽑아내기 때문에 실제 위, 장에 도움이 되지 않는 영양제
도 포함됨 and 태그 개수가 24개임
```
위와 같은 문제점이 있었지만 1번 방법보다 나은 점도 있었기 때문에 어떤 방법을 쓸지 고민이 되었다.
* 2번 방법이 1번 방법보다 나은 점
```
-> 1번 방법이 2번 방법과 비교해서 빠지는 태그이름: 치아 건강, 위 건강/소화기능, 운동수행 능력
개선, 배뇨 기능, 어린이 성장, "질내 유익균 증식, 유해균 억제"
and '면역 기능'은 있긴 하지만 1번 방법에서는 결과개수가 5103개인데 비해 2번 방법
에서는 2개 뿐임.
```

<br>

이 두 방법을 절충하기 위해 2가지 방법을 더 생각해봤다.
```
3. tag 이름 전체를 supplements.effect에서 찾거나, supplement_name에서 찾아서
select

SELECT id, supplement_name, effect, name, count(*) FROM supplements, tags
WHERE effect LIKE CONCAT('%',name,'%') OR
	supplement_name LIKE CONCAT('%', name, '%')
GROUP BY name
ORDER BY name

4. tag 이름 전체를 supplements.effect에서 찾거나 tag 이름 중 띄어쓰기 앞 단어를 
supplements.supplement_name에서 찾아서 select

SELECT id, supplement_name, effect, name, count(*) FROM supplements, tags
WHERE effect LIKE CONCAT('%',name,'%') OR
	supplement_name LIKE CONCAT('%',(SELECT SUBSTRING_INDEX(name, ' ', 1)),'%')
GROUP BY name
ORDER BY name
```
3번 방법으로 select를 했을 때는 앞서 1번 방법과 큰 차이가 없었고,<br>
4번 방법은 `tag_name`과 관련이 없는 영양제까지 검색되어 연결되는 문제점이 있었다.<br>
Ex) `tag_name`은 '갱년기 남성'인데 앞 단어 '갱년기'를 기준으로 effect를 검색하니 '갱년기여성...'과 같은 제품도 검색되어 적합하지 않은 제품까지 연결되었다.

이러한 문제점들 때문에 앞 글자를 추출해서 supplement_name을 검색하는 row 값을 제한하기로 결정했다.<br>
앞 단어로 supplement_name을 검색해야 했던 태그 이름들 중 가장 필요했던 단어는 '칼슘'이었고, 최종적으로 다음과 같이 sql 문을 작성하여 `supplement_tags` 테이블의 row를 insert 할 수 있었다.
```
SELECT id, supplement_name, effect, name, count(*) FROM supplements, tags
WHERE effect LIKE CONCAT('%',name,'%') OR
	supplement_name LIKE CONCAT('%', name, '%') OR
    (name = "칼슘 흡수" AND effect LIKE CONCAT("%칼슘%"))
GROUP BY name
ORDER BY name
```

### 3. 오류 해결
`supplement_tags` insert를 할 방법을 결정한 뒤 sql 스크립트를 작성하는데 오류가 생겼다.

* sql 스크립트

<img src="https://user-images.githubusercontent.com/55045377/128627375-30c2834f-72c5-4ccb-95e6-f7b80afc6e02.png" width=60% height=60%>

* 오류 내용
```
ERROR 1100 (HY000) at line 158: Table 'SUPPLEMENTS' was not locked with LOCK TABLES
```
알고 보니 `supplement_tags`에 테이블 LOCK을 걸고 `supplements` 테이블을 사용하려고 해서 생긴 문제였다.<br>
그래서 맨 윗줄의 `LOCK TABLES ...`만 지워주면 해결되는 거였다.

<br>

* 오류 해결 sql 스크립트

<img src="https://user-images.githubusercontent.com/55045377/128627469-3c78945c-37ee-4eda-83dc-1a043e5859ab.png" width=60% height=60%>

-------

`supplement_tags` 테이블에 insert가 잘 된 것을 확인할 수 있다.

<img src="https://user-images.githubusercontent.com/55045377/128627579-4abc012c-4237-44a6-8ad4-f6af60ad2449.png" width=35% height=35%>

<br><br><br>













