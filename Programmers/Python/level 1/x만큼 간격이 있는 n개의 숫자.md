## 내가 짠 코드
```python
def solution(x, n):
    answer = []
    num = 0
    for _ in range(n):
        answer.append(num + x)
        num += x
    return answer
```
`x`만큼 수가 늘어나기 때문에 `x`의 값은 변하면 안 된다. 그래서 append하는 변수 `num`을 따로 선언해서 `num`에 `x`값을 더해서 append하는 방법으로 문제를 풀었다.

<br><br>

## 다른 풀이
### 1.
```python
def number_generator(x, n):
    return [i * x + x for i in range(n)]
    
print(number_generator(2, 5))
```

<br><br>











