## 내가 짠 코드
```python
def solution(array, commands):
    answer = []
    
    for fac in commands:
         # i,j,k에 값 할당
        i = fac[0]
        j = fac[1]
        k = fac[2]
        
        answer.append(sorted(array[i-1:j])[k-1])
        
    return answer

arr = [1, 5, 2, 6, 3, 7, 4]
com = [[2, 5, 3], [4, 4, 1], [1, 7, 3]]
print(solution(arr,com))
```
<br><br>

## 다른 풀이
### 1.
```python
def solution(array, commands):
    return list(map(lambda x:sorted(array[x[0]-1:x[1]])[x[2]-1], commands))
```
<br><br>
