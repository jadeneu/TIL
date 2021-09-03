# fgets
```c
#include <stdio.h>
char* fgets(char* str, int num, FILE* stream);
```
스트림(stream)에서 문자열을 받는다.

스트림에서 문자열을 받아서 `num-1`개 까지의 문자를 입력받아 C 형식의 문자열로 저장한다.<br>
**개행 문자**는 `fgets` 로 하여금 입력을 끝나게 하지만 이 문자 역시 `str` 에 저장한다. <br>
NULL 문자는 자동적으로 마지막으로 입력받은 문자 뒤에 붙는다.

* **참고**<br>
`fgets` 함수는 `scanf` 함수와는 달리 오직 개행 문자에 의해서만 입력이 끝나기 때문에 띄어쓰기가 있는 문자열도 입력 받을 수 있다. <br>
반면에 `scanf` 함수의 경우 개행 문자 뿐만이 아니라 `' '` 와 `'\t'` 에 의해서도 입력이 끝나기 때문에 띄어쓰기가 있는 문자열은 입력 받을 수 없다.

<br><br>

## 인자
### str
읽어들인 문자열을 저장할 `char` 배열을 가리키는 포인터이다.

### num
마지막 NULL 문자를 포함하여, 읽어들일 최대 문자 수이다. 다시 말해 이 값이 10 이면 컴퓨터는 최대 9 문자를 입력 받는다.

### stream
문자열을 읽어들일 스트림의 FILE 객체를 가리키는 포인터이다. 특히, 표준 입력(stdin) 에서 입력을 받으려면 여기에 `stdin` 을 써주면 된다. 

* **Ex)**<br>
```c
fgets (str, 100, stdin);
```

<br><br>

## 리턴값
성공적으로 읽어들였다면 함수는 `str` 을 리턴한다.

만일 파일 끝에 도달하였는데 아무런 문자도 읽어들이지 않았다면 str 의 내용은 변하지 않고 그 대신 null 포인터가 리턴된다.

또한 오류가 발생해도 null 포인터가 리턴된다.

`ferror` 함수나 `feof`를 사용해서 각각 어떤 오류가 발생하였는지, 파일 끝에 도달하였는지 알 수 있다.

<br><br>

## 예제
### 1.
```c
#include <stdio.h>
#define MAX_STR_SIZE 100

int main() {
  char str_read[MAX_STR_SIZE];
  fgets(str_read, MAX_STR_SIZE, stdin);
  printf("읽어들인 문자열 : %s \n", str_read);
  return 0;
}
```


<br><br>












---
# References
* https://modoocode.com/38

<br><br><br>
