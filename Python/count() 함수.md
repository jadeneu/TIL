# 목차
* [count() 함수](#count-함수)
* [count()의 특징](#count의-특징)
* [count() 예시](#count-예시)
---
* [출처](#출처)
<br><br><br>

## count() 함수
count() 함수는 문자열 안에서 찾고 싶은 문자의 개수를 찾을 수 있다.<br>
count() 함수는 튜플, 리스트, 집합과 같은 반복 가능한 iterable 자료형에서도 사용 가능하다.

함수 모양을 한번 보자.

***.count(self, x, __start, __end)***

self는 일단 무시하고 살펴보자.<br>
**x는 찾을 문자열, 찾을 문자를 넣으면 된다.**<br>
__ start, __ end 는 iterable 자료형의 어디부터 어디까지 내부에서 찾아달라는 뜻이다.
<br><br>

## count()의 특징
* 대소문자를 구분한다.
* 찾을 x 에 문자 한개를 넣어도 가능하고 문자열을 넣어도 가능하다.
* __ start, __ end에 아무것도 넣지 않으면 iterable 처음부터 끝까지 탐색한다.
* 찾을 x의 범위는 __ start <= x < __ end이다. __ start는 포함, __ end는 포함하지 않는다.
<br><br>

## count() 예시
```python
# 문자열 'BlockDMask' 선언 
a = 'BlockDMask' 

# 문자열에서 'k'가 몇개 있는지 ?
print('#1 a.count("k")') 
print(a.count('k')) 

# 문자열에서 'DM'가 몇개 있는지 ? 
print('#2 a.count("DM")') 
print(a.count('DM')) 

# 문자열에서 특정 범위 내부에 'k' 가 몇개 있는지? 
# B l o c k D M a s k 에서 index를 표기해보면 
# 0 1 2 3 4 5 6 7 8 9 이다. 

print("#3 a[2] + ' ~ ' + a[4]") 
print(a[2] + ' ~ ' + a[4]) 

print("#4 a.count('k', 2, 3)") 
print(a.count('k', 2, 3)) 

print("#5 a.count('k', 2, 4)") 
print(a.count('k', 2, 4)) 

print("#6 a.count('k', 2, 5)") 
print(a.count('k', 2, 5))
```
<br>

### 예제 결과
<img src="https://user-images.githubusercontent.com/55045377/121183302-e2c6cf00-c89e-11eb-8193-f3c8aaf8fce2.png" width=30% height=30%>

<br><br>

---
# 출처
* https://blockdmask.tistory.com/410 [개발자 지망생]
* https://ooyoung.tistory.com/76
