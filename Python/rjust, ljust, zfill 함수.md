# 목차
* [rjust 함수](#rjust-함수)
* [ljust 함수](#ljust-함수)
* [zfill 함수](#zfill-함수)
<br><br><br>

## rjust 함수
rjust 함수는 문자열을 오른쪽 정렬하도록 도와준다.

형태는 다음과 같다.
### *str.rjust(width, [fillchar])*
<br>

공백을 공백 그대로 두려면 아무것도 입력하지 않으면 된다.<br>
예시는 아래와 같다.
```python
names = ["Jane", "JungHyun", "DH", "Leonardo"]
for i in names:
  print (i.rjust(10,'.'))

for i in names:
  print(i.rjust(10))
```
<br>

출력 결과
```
......Jane
..JungHyun
........DH
..Leonardo
      Jane
  JungHyun
        DH
  Leonardo
```
<br><br>

## ljust 함수
ljust 함수는 문자열을 왼쪽 정렬하도록 도와준다.

형태는 다음과 같다.
### *str.ljust(width, [fillchar])*
<br>

예시는 아래와 같다.
```python
val = "222".ljust(3, "0")
print(val) 

val = "222".ljust(15, "a")
print(val)
```
<br>

출력 결과
```
222
222aaaaaaaaaaaa
```
<br><br>

## zfill 함수
zfill 함수는 문자열 앞에 0을 채워준다.

형태는 다음과 같다.
### *str.zfill(width)*
<br>

예시는 아래와 같다.
```python
str = '1234'

str_zero = str.zfill(8)
print(str_zero)
# 00001234

# 지정한 자리수보다 대상 문자열 길이가 긴 경우
print(str.zfill(3))
# 1234

# 문자열 앞에 +나 - 기호가 있는 경우
s = '-1234'
print(s.zfill(8))
# -0001234

s = '+1234'
print(s.zfill(8))
# +0001234
```
<br><br>



















---
# 출처
* https://www.crocus.co.kr/1660
* https://ponyozzang.tistory.com/536
* https://lovelydiary.tistory.com/294
<br><br>




