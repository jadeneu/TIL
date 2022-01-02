### 코드
```python
def solution(s):
    s = s.lower()
    p_cnt = s.count('p')
    y_cnt = s.count('y')
    
    if p_cnt == y_cnt:
        return True
    else:
        return False
```

### 코드 설명
* 문자열 `s`를 `lower()`로 소문자로 변경하고 `count()`를 사용하여 p와 y의 개수를 구한다.
* `p_cnt` 값과 `y_cnt` 값이 같으면 True, 아니면 False를 return 한다.
