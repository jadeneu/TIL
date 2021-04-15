# 목차
* [1장 코딩 인터뷰](#1장-코딩-인터뷰)
* [2장 프로그래밍 언어 선택](#2장-프로그래밍-언어-선택)
  + [1. 제네릭 프로그래밍](#1-제네릭-프로그래밍)
  + [2. 구조체](#2-구조체)
  + [3. 클래스](#3-클래스)
* [출처](#출처)
<br><br>



<img src="https://user-images.githubusercontent.com/55045377/114827387-9497d100-9e03-11eb-931f-fe0a3cb8e5d2.jpg">
스터디에서 쓰고 있는 책이다.<br>
그럼 1~2장 리뷰 시작!

---

# 1장 코딩 인터뷰
코딩 인터뷰, 코딩테스트에 대한 전반적인 설명 및 팁에 관한 내용이었다.
<br><br>

# 2장 프로그래밍 언어 선택
### 1. 제네릭 프로그래밍
* **정의**<br>
제네릭(generic)이란 파라미터의 타입이 나중에 지정되게 해서 재활용성을 높일 수 있는 프로그래밍 스타일이다.<br>
(포괄적으로는 어느 상황, 어느 자료형에서도 수행할 수 있는 기능, 로직을 작성하는 기법)

* **예시**<br>
C에서는 void* 를 사용해서 제네릭 프로그래밍을 할 수 있다.<br>
void* 는 어느 자료형이든 간에 형변환 없이 자료형의 주소를 담을 수 있다.<br>
  + 기본 자료형일 때
  ```c
  void* h = CreateHead(10);
  int i;
  for(i=0; i<10; i++)
    AppendNode(h, 10*i+20);
  for(i=0;;)
  {
    int data = (int)GetData(h, i);
    if(NULL == data)
      break;
    
    printf("%d\n", data);
    i++;
  }
  ```
  + 사용자 정의 자료형일때
  ```c
  DATA headData = {10, 20};
  void* h = CreateHead(&headData);
  int i;

  DATA data1 = {30, 40};
  DATA data2 = {40, 50};
  DATA data3 = {60, 70};
  DATA data4 = {80, 90};

  AppendNode(h, &data1);
  AppendNode(h, &data2);
  AppendNode(h, &data3);
  AppendNode(h, &data4);

  for(i=0;;)
  {
    PDATA data = (PDATA)GetData(h, i);
    if(NULL == data)
      break;

    printf("%d %d\n", data->a, data->b);
    i++;
  }
  ```
  
* **추가**<br>
파이썬은 원래 동적 타이핑 언어이기 때문에 제네릭이 필요 없다. <br>
하지만 동적 타이핑의 장점이자 단점은 얼핏 사용하기엔 매우 편하지만 코드의 복잡도가 높아질수록 혼란을 가중시킨다는 점이다. 타입을 아예 명시하지 않으면 가독성을 낮추고 버그 발생 확률이 높아진다.<br><br>
(파이썬으로 타입 명시하는 예시 → 책 58p)
<br><br>

### 2. 구조체
* **정의**<br>
구조체는 메모리의 어느 영역에나 어떤 크기로든 할당할 수 있는 복합 자료형이다.

* **설명**<br>
파이썬에는 구조체가 없다. 그리고 클래스 또한 데이터 타입을 지정할 수 없다.<br>
따라서 구조체와 같은 형태를 취하려면 네임드 튜플(Named Tuple)을 사용해야 했다.<br><br>
하지만 파이썬 3.7부터 @dataclass 데코레이션을 사용하여 class를 이용해 구조체 형태를 취할 수 있게 되었다.<br>
(그 예시는 → 책 62p)

* **추가**
  + **튜플**<br>
  튜플(tuple)이란 자료 구조로, 배열처럼 여러 개의 데이터를 열거하여 담아두는 데에 사용한다.<br><br>
    - **Q. 튜플과 배열의 차이점은?**<br>
    -배열 : 여러 개의 데이터들을 모은 집합. 추가와 삭제가 가능하다. [ ]로 사용한다.<br><br>
    -튜플 : 리스트와 동일하게 여러 객체를 모아서 담는다. 숫자, 문자, 객체, 배열, 튜플 안의 튜플 전부 가능하다.<br>
    하지만 튜플 내의 값은 수정이 불가하다. 추가도, 삭제도 안 된다. 한번 만들어지면 끝까지 가지고 가야 된다.<br>
    ( )로 사용하고, ( )가 없어도 동일하게 사용 가능하다.<br><br>
    → 읽을 수만 있게 하고 싶을 때, 수정하면 안 되는 자료를 쓸 땐  Tuple을 사용한다.<br>
    ※ 수정과 삭제가 안 된다고 해서 덮어 씌우기까지 안 되는 건 아니고, 다른 것으로 재할당이 가능하다. 문자열과 비슷하다고 보면 됨.
    
  + **네임드 튜플**<br>
  (교재 예시 → 62p)<br>
  <img src="https://user-images.githubusercontent.com/55045377/114832723-c3b14100-9e09-11eb-8f6e-210193a1ac35.png" width=70% height=70%>
  <img src="https://user-images.githubusercontent.com/55045377/114832742-c7dd5e80-9e09-11eb-92a7-3afd0b8b1134.png" width=70% height=70%>
  <img src="https://user-images.githubusercontent.com/55045377/114832747-c9a72200-9e09-11eb-8a75-44e3bbba497d.png" width=70% height=70%>
  
### 3. 클래스
* **설명**<br>
<img src="https://user-images.githubusercontent.com/55045377/114833332-6d90cd80-9e0a-11eb-87f1-ba5e852bce73.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114833334-6ec1fa80-9e0a-11eb-8cbb-f28e6e478a10.png" width=80% height=80%>

* **예시**<br>
<img src="https://user-images.githubusercontent.com/55045377/114834293-61f1d680-9e0b-11eb-8cae-e3bebeed5aad.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834299-63bb9a00-9e0b-11eb-994d-1783c75f8ebf.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834303-64543080-9e0b-11eb-86ed-0b45323ce0d4.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834308-64543080-9e0b-11eb-884e-cffcf867f9e9.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834311-65855d80-9e0b-11eb-8580-fc3d5b768ffc.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834315-661df400-9e0b-11eb-8cdb-16c7e73ec8c9.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834318-66b68a80-9e0b-11eb-905e-470185a38e0b.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834322-674f2100-9e0b-11eb-99e6-5c4cf61ef221.png" width=80% height=80%>
<img src="https://user-images.githubusercontent.com/55045377/114834326-67e7b780-9e0b-11eb-996f-66de5636662b.png" width=80% height=80%>

* **추가**<br>
<img src="https://user-images.githubusercontent.com/55045377/114835037-21468d00-9e0c-11eb-9799-ce0405cf8b40.png" width=80% height=80%>
<br><br>



  
  
  
  
  
## 출처
### 2장 프로그래밍 언어 선택 
#### 1. 제네릭 프로그래밍
* https://blog.naver.com/chhh92/70168891255<br>
[출처] [C] C 제네릭 프로그래밍(C Generic Programming)|작성자 후추나

#### 2. 구조체
* https://velog.io/@riceintheramen/%ED%8A%9C%ED%94%8C%EC%9D%B4-%EB%AD%90%EC%98%88%EC%9A%94<br>
[출처] 튜플이 뭐예요?|작성자 riceintheramen
* https://velog.io/@hyeseong-dev/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EB%84%A4%EC%9E%84%EB%93%9C%ED%8A%9C%ED%94%8C-tutorial
  
#### 3. 클래스
* https://wikidocs.net/28
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
