# 목차
* [document.querySelector()](#1-documentqueryselector)
* [document.createElement()](#2-documentcreateelement)
* [document.body.appendChild()](#3-documentbodyappendchild)
* [.value](#4-value)
* [.select()](5-select)
* [document.execCommand()](#6-documentexeccommand)
* [document.body.removeChild()](#7-documentbodyremovechild)
* [.addEventListener()](#8-addeventlistener)
* [출처](#출처)

<br><br>


인프런에서 진행중인 'MBIT' 사이트를 만들어 보며 장고 사용법을 익히고 있다. <br>
MBIT 사이트를 따라 만들면서 모르는 문법들을 정리해보자.

---

<img src="https://user-images.githubusercontent.com/55045377/114498887-f0c1f000-9c5f-11eb-9a5e-724dda34b66e.png" width=60% height=60%>

## 1. document.querySelector()
.querySelector()는 CSS 선택자로 요소를 선택하게 해준다. 주의할 점은 선택자에 해당하는 첫번째 요소만 선택한다는 것이다.

<img src="https://user-images.githubusercontent.com/55045377/114499245-a68d3e80-9c60-11eb-8bde-8d8f4efc4466.png" width=70% height=70%>

<img src="https://user-images.githubusercontent.com/55045377/114499241-a55c1180-9c60-11eb-9bfe-41f18823d4b1.png" width=70% height=70%>
<br><br>

## 2. document.createElement()
html 요소를 추가할 수 있다.

<img src="https://user-images.githubusercontent.com/55045377/114499634-772b0180-9c61-11eb-8263-3de5c00caf19.png" width=50% height=50%>

<img src="https://user-images.githubusercontent.com/55045377/114499638-77c39800-9c61-11eb-9f70-c9e1ce1faa53.png" width=70% height=70%>
<br><br>

## 3. document.body.appendChild()
.appendChild()는 선택한 요소 안에 자식 요소를 추가한다.

<img src="https://user-images.githubusercontent.com/55045377/114500165-7c3c8080-9c62-11eb-974f-50b11839d1d6.png" width=80% height=80%>

<img src="https://user-images.githubusercontent.com/55045377/114500166-7cd51700-9c62-11eb-8600-be4d89883eb0.png" width=90% height=90%>

> '노드 추가!' 클릭 전

<img src="https://user-images.githubusercontent.com/55045377/114500163-7b0b5380-9c62-11eb-900f-0f8570537298.png" width=90% height=90%>

> '노드 추가!' 클릭 후

<br><br>

## 4. .value
구글에 검색결과가 얼마 나오지 않아서 찾지는 못했는데, <br>
내 느낌상 위의 코드내용에 맞춰서 해석해보면<br>
tmp, 즉 input 요소의 value를 url 로 두라는 의미같다.
<br><br>

## 5. .select()
.select() 는 input text 나 textarea 요소를 드래그한 듯이 선택하기 위해 사용하는 함수다.

<img src="https://user-images.githubusercontent.com/55045377/114500646-6b403f00-9c63-11eb-8d1c-e1f0096a957a.png" width=70% height=70%>

Select text 버튼을 클릭하면

<img src="https://user-images.githubusercontent.com/55045377/114500649-6bd8d580-9c63-11eb-887e-d7fa696f8462.png" width=30% height=30%>

이렇게 드래그가 된 것을 알 수 있다.
<br><br>

## 6. document.execCommand()

<img src="https://user-images.githubusercontent.com/55045377/114501110-4ac4b480-9c64-11eb-8591-8f4a5f026713.png" width=70% height=70%>

<img src="https://user-images.githubusercontent.com/55045377/114501114-4b5d4b00-9c64-11eb-83e7-7f1ab4062bcc.png" width=65% height=65%>

<br><br>

## 7. document.body.removeChild()

형식 -> "**부모노드.removeChild(delNode)**"

<img src="https://user-images.githubusercontent.com/55045377/114501581-3af9a000-9c65-11eb-87d8-0003fc1202fd.png" width=70% height=70%>

<br><br>

## 8. .addEventListener()
addEventListener은 이벤트를 등록하는 가장 권장되는 방식이다. 

<img src="https://user-images.githubusercontent.com/55045377/114501821-ae031680-9c65-11eb-98cc-b0f352faa30f.png" width=70% height=70%>

<img src="https://user-images.githubusercontent.com/55045377/114501818-ad6a8000-9c65-11eb-9e16-330ffd0bf294.png" width=70% height=70%>

<br><br>




---

## 출처
* 1.document.querySelector()<br>
https://www.codingfactory.net/10410
* 2.document.createElement()<br>
https://www.codingfactory.net/10436
* 3.document.body.appendChild()<br>
http://www.tcpschool.com/javascript/js_dom_nodeManage
* 5..select()<br>
https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/select
* 6.document.execCommand()<br>
https://dororongju.tistory.com/30<br>
https://jamesdreaming.tistory.com/220
* 7.document.body.removeChild()<br>
https://m.blog.naver.com/PostView.nhn?blogId=5340579&logNo=220908240262&proxyReferer=https:%2F%2Fwww.google.com%2F
* 8..addEventListener()<br>
https://m.blog.naver.com/PostView.nhn?blogId=qbxlvnf11&logNo=220877806711&proxyReferer=https:%2F%2Fwww.google.com%2F








