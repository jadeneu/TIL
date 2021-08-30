## for...else... 구문
for문을 사용하다보면 루프 중간에 break 문으로 빠져나오는 경우들이 있는데, 이게 break문이 걸려서 빠져나가는지 아닌지를 판단이 필요한 경우가 있다.

보통은 flag 등을 둬서 처리하는게 기존의 방식이라면, for문과 같은 레벨에 else를 둬서 break없이 빠져나온 경우를 처리하는 방법이다.
```python
for x in range(4):
  if x == 2:
    print ('loop out')
    break
else:
  print ('loop end')
```
위 예제의 경우는 x =2 에서 루프를 빠져나오기때문에, else문이 실행이 되지 않고 'loop out' 이 출력이 되고,
```python
for x in range(4):
  pass
else:
  print ('loop end')
```
위와 같은 경우는 for loop가 break없이 빠져나왔으므로 'loop end' 가 출력이 된다.
<br><br>

* **출처 : http://pyengine.blogspot.com/2019/12/for-else.html**
<br><br>
