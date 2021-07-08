# 목차

# 16장 트라이
> 트라이(Trie)는 검색 트리의 일종으로 일반적으로 키가 문자열인, 동적 배열 또는 연관 배열을 저장하는 데 사용되는 정렬된 트리 자료구조다.

트라이(Trie)는 실무에 매우 유용하게 쓰이는 자료구조로서, 특히 자연어 처리(NLP) 분야에서 문자열 탐색을 위한 자료구조로 널리 쓰인다.<br>
트라이는 1959년에 처음 공개됐으며, 검색을 뜻하는 'retrieval'의 중간 음절에서 용어를 따왔다.<br>
'retrieval(리트리벌이라고 읽는다)'에서 따온 단어이기에 초창기에는 '트리'로 발음했으나, 기존의 트리(Tree)와 구분하기 위해 오늘날에는 '트라이'로 불린다.<br>
**트라이는** 트리와 유사하지만, 지금까지 우리가 주로 살펴본 이진 트리의 모습이 아닌 **전형적인 다진 트리(m-ary Tree)의 형태를 띤다.**

트라이는 각각의 문자 단위로 색인을 구축한 것과 유사한데, 예를 들어 그림 16-1의 트라이에서 단어 apple을 찾는다고 가정해보자.<br>
수백 개의 문자가 있다고 할 때 이 경우 트라이 탐색을 하면 단 다섯 번 만에 apple 문자열의 존재 여부를 파악할 수 있다.<br>
문자열의 길이만큼만 탐색하면 되기 때문이다.<br>
자연어 처리 분야에서는 형태소 분석기에서 분석 패턴을 트라이로 만들어두고 자연어 문장에 대해 패턴을 찾아 처리하는 등으로 활용하고있다.

* ***색인 : 책 속의 낱말이나 구절, 또 이에 관련한 지시자를 찾아보기 쉽도록 일정한 순서로 나열한 목록을 가리킨다. 인덱스(index)라고도 한다.***<br>

<img src="https://user-images.githubusercontent.com/55045377/124859754-66352680-dfeb-11eb-9e82-907134a6ea33.png" width=50% height=50%>

이 그림은 apple, appear, appeal 등으로 트라이를 구성한 것이다.<br>
여기서 만약 apple을찾는다면, a → p → p 순으로 문자별 일치하는 노드를 찾아 내려가면 된다.<br>
apple이므로 그다음은 l 노드를 찾아 내려가면 될 것이고, 만약 appear를 찾는다면 e 노드를 찾아 내려가면 된다.<br>
이런 식으로 루트부터 a → p → p → l → e까지 내려가면 단어 apple을 찾을 수 있다.<br>
이처럼 트라이에서는 각 문자열의 길이만큼만 탐색하면 원하는 결과를 찾을 수 있다.

트라이는 문자열을 위한 트리의 형태이기 때문에 사실상 문자 개수만큼 자식이 있어 그림 16-1과 같이 나타내보면 상당히 많은 자식 노드를 갖고 있는 트리임을 확인할 수 있다.<br>
이제 문제 풀이를 통해 트라이를 직접 한번 구현해보자.
<br><br>

## 리트코드
### 문제 56 트라이 구현
* 트라이의 insert, search, startsWith 메소드를 구현하라.

  ```c++
  Trie trie = new Trie();
  
  trie.insert("apple");
  trie.search("apple");   // returns true
  trie.search("app");     // returns false
  trie.startsWith("app"); // returns true
  trie.insert("app");
  trie.search("app");     // returns true
  ```
  
### 문제 56 트라이 구현 풀이
#### 풀이1. 딕셔너리를 이용해 간결한 트라이 구현
트라이를 직접 구현해보는 문제다.<br>
여기서는 딕셔너리를 이용해 가급적 간결한 형태로 풀이해본다.<br>
먼저, 트라이를 저장할 노드는 다음과 같이 별도 클래스로 선언한다.

```python
class TrieNode:
    def __init__(self):
        self.word = False
        self.children = {}
```
메소드를 포함해 같은 클래스로 묶을 수도 있지만, 이 경우 구현상 복잡도가 늘어나기 때문에 가능하면 간결하게 구현하기 위해 별도로 구분해 선언했다. <br>
다음으로, 트라이 연산을 구현할 별도 클래스를 선언하고 삽입 메소드를 구현해보자.

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.word = True
```
처음 Trie 클래스를 생성하게 되면 루트 노드로 별도 선언한 TrieNode 클래스를 갖게 되고, 삽입 시 루트부터 자식 노드가 점점 깊어지면서 문자 단위의 다진 트리(m-ary Tree) 형태가 된다.<br>
만약 입력값이 apple인 경우, 삽입 코드는 다음과 같다.

```python
t = Trie()
t.insert('apple')
```
이 경우 트라이는 그림 16-2 같은 형태가 된다.

<img src="https://user-images.githubusercontent.com/55045377/124861343-5c60f280-dfee-11eb-9ecf-f5938a717d04.png" width=40% height=40%>

이 그림에서 트라이는 다음 문자를 키로 하는 자식 노드 형태로 점점 깊어지면서, 각각의 노드는 word 값을 갖는다.<br> 
이 값은 단어가 모두 완성되었을 때만 True가 된다.<br>
즉 apple의 경우 단어가 모두 완성되는 e에 이르러서야 True로 셋팅된다.<br>
만약 apple 외에 appear, appeal 같은 문자가 추가로 삽입된다면 코드는 다음과 같다.

```python
t.insert('appear')
t.insert('appeal')
```
마찬가지로 트라이를 구성하는 다진 트리는 다음 그림 16-3과 같은 형태가 될 것이다.


























