## get
```python
>>> a = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}
>>> a.get('name')
'pey'
>>> a.get('phone')
'0119993323'
```
**get(x) 함수는 x라는 Key에 대응되는 Value를 돌려준다.**<br>
a.get('name')은 a['name']을 사용했을 때와 동일한 결과값을 돌려받는다.

다만 다음 예제에서 볼 수 있듯이 a['nokey']처럼 존재하지 않는 키(nokey)로 값을 가져오려고 할 경우<br>
a['nokey']는 Key 오류를 발생시키고 a.get('nokey')는 None을 돌려준다는 차이가 있다. 
```python
>>> a = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}
>>> print(a.get('nokey'))
None
>>> print(a['nokey'])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'nokey'
```
딕셔너리 안에 찾으려는 Key 값이 없을 경우 미리 정해 둔 디폴트 값을 대신 가져오게 하고 싶을 때에는 **get(x, '디폴트 값')** 을 사용하면 편리하다.
```python
>>> a.get('foo', 'bar')
'bar'
```
a 딕셔너리에는 'foo'에 해당하는 값이 없다. 따라서 디폴트 값인 'bar'를 돌려준다.
<br><br>

---

* **출처: https://wikidocs.net/16#key-valueget**






















