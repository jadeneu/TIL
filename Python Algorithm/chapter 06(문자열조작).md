# 목차
* [chapter 6. 문자열 조작](#6장-문자열-조작)
* [References](#references)

<br><br>

<img src="https://user-images.githubusercontent.com/55045377/116873723-1a64aa80-ac53-11eb-8d03-5df57948b42d.jpg">

<br>

---

<br>

# 6장 문자열 조작
> 문자열 조작(String Manipulation)이란 문자열을 변경하거나 분리하는 등의 여러 과정을 말한다.

<br>

파이썬에서 문자열을 조작하는 메서드는 여러 가지가 있다.<br>
* **문자열 바꾸기**
  * **[replace](#replace)**
  * **[translate](#translate)**
* **문자열 분리하기**
  * **split**
* **구분자 문자열과 문자열 리스트 연결하기**
  * **join**
* **소문자를 대문자로 바꾸기**
  * **upper**
* **대문자를 소문자로 바꾸기**
  * **lower**
* **왼쪽 공백 삭제하기/왼쪽의 특정 문자 삭제하기**
  * **lstrip**
* **오른쪽 공백 삭제하기/오른쪽의 특정 문자 삭제하기**
  * **rstrip**
* **양쪽 공백 삭제하기/양쪽의 특정 문자 삭제하기**
  * **strip**
* **문자열을 왼쪽 정렬하기**
  * **ljust**
* **문자열을 오른쪽 정렬하기**
  * **rjust**
* **문자열을 가운데 정렬하기**
  * **center**
* **메서드 체이닝**
* **문자열 왼쪽에 0 채우기**
  * **zfill**
* **문자열 위치 찾기**
  * **find**
* **오른쪽에서부터 문자열 위치 찾기**
  * **rfind**
* **문자열 위치 찾기**
  * **index**
* **오른쪽에서부터 문자열 위치 찾기**
  * **rindex**
* **문자열 개수 세기**
  * **count**
  
<br><br>


## replace
replace('바꿀문자열', '새문자열')은 문자열 안의 문자열을 다른 문자열로 바꾼다.<br>
```python
s = 'Hello, world!'
print(s.replace('world!', 'Python'))
```
```
Hello, Python
```
문자열 자체는 변경하지 않으며 바뀐 결과를 반환한다.
```python
s = 'Hello, world!'
s.replace('world!', 'Python')
print(s)
```
```
Hello, world!
```

<br>

## translate
translate는 문자열 안의 문자를 다른 문자로 바꾼다.<br>
먼저 str.maketrans('바꿀문자', '새문자')로 변환 테이블을 만든다. 그다음에 translate(테이블)을 사용하면 문자를 바꾼 뒤 결과를 반환한다. 
```python
table = str.maketrans('aeiou', '12345')
'apple'.translate(table)
```
```
'1ppl2'
```




---

# References
* https://dojang.io/mod/page/view.php?id=2299

<br><br>
