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

<img src="https://user-images.githubusercontent.com/55045377/124861626-ea3cdd80-dfee-11eb-8026-5361a7246dfb.png" width=55% height=55%>

이 그림에서 트라이는, 같은 문자가 같은 자식을 타고 내려가다가, 달라지는 문자부터 서로 다른 노드로 분기된다.<br>
마지막에는 appeal과 appear가 완성되는 l과 r 노드가 각각 True로 셋팅된다.<br>
여기까지가 삽입의 기본 원리다.

그렇다면 이제 이 값이 존재하는지 확인하려면 어떻게 해야 할까?<br>
문제에 제시된 메소드인 search()와 startsWith() 메소드가 그런 역할을 한다. <br>
search()는 단어가 존재하는지 여부를 판별하는 것이고 startsWith()는 해당 문자열로 시작하는 단어가 존재하는지 여부를 판별한다.<br>
즉 둘 다 동일하게 문자 단위로 계속 깊이 탐색을 하고 search()의 경우에만 마지막에 word가 True인지 확인하면 될 것이다.<br>
먼저 search()의 코드는 다음과 같다.

```python
def search(self, word: str) -> bool:
    node = self.root
    for char in word:
        if char not in node.children:
            return False
        node = node.children[char]
        
    return node.word
```
문자열에서 문자를 하나씩 for 반복문으로 순회하면서 자식 노드로 계속 타고 내려간다.<br>
그리고 마지막에 node.word 여부를 리턴한다.<br>
만약 단어가 완성된 트라이라면 True로 되어 있을 것이고, 이때 True가 결과로 리턴된다.<br>
startsWith()의 코드는 다음과 같다.

```python
def startsWith(self, prefix: str) -> bool:
    node = self.root
    for char in prefix:
        if char not in node.children:
            return False
        node = node.children[char]
        
    return True
```
search()와 거의 동일하다.<br>
차이점은 node.word를 확인하지 않고, 자식 노드가 존재하는지 여부만 판별한다는 점이다.<br>
자식 노드가 존재한다면 for 반복문을 끝까지 빠져나올 것이고, True를 리턴한다. 

이제 모든 메소드를 정리하면 전체 코드를 만들어 낼 수 있다.

그전에 먼저 코드를 좀 더 줄여보자.<br>
self.children을 defaultdict()로 선언한다면 insert() 삽입 메소드에서 매번 if로 체크하는 구문은 없앨 수 있을 것 같다.<br>
이를 반영한 전체 코드는 다음과 같다. 

```python
# 트라이의 노드
class TrieNode:
    def __init__(self):
        self.word = False
        self.children = collections.defaultdict(TrieNode)
        
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    # 단어 삽입
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            node = node.children[char]
        node.word = True
    
    # 단어 존재 여부 판별
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]

        return node.word
    
    # 문자열로 시작 단어 존재 여부 판별
    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]

        return True
```
<br><br>

### 문제 57 팰린드롬 페어
* 단어 리스트에서 words[i] + words[j]가 팰린드롬이 되는 모든 인덱스 조합(i, j)를 구하라(팰린드롬에 대한정의는 138페이지를 참조).

* 예제1
  * 입력
    ```
    ["abcd","dcba","lls","s","sssll"]
    ```
  * 출력
   ```
   [[0,1],[1,0],[3,2],[2,4]]
   ```
  * 설명<br>
    ["dcbaabcd", "abcddcba", "slls", "llssssll"]이 팰린드롬이다.
    
* 예제2
  * 입력
    ```
    ["bat","tab","cat"]
    ```
  * 출력
    ```
    [[0,1], [1,0]]
    ```
  * 설명<br>
    ["battab", "tabbat" ]이 팰린드롬이다.

<br><br>

### 문제 57 팰린드롬 페어 풀이
#### 풀이1. 팰린드롬을 브루트 포스로 계산
먼저 브루트 포스 풀이부터 진행해보자.<br>
각각의 모든 조합을 구성해보고 이 구성이 팰린드롬인지 여부를 판별하면, O(n^2) 시간 복잡도로 브루트 포스 풀이가 가능할 것 같다.<br>
먼저, 팰린드롬 여부를 체크하는 함수는 6장의 1번 '유효한 팰린드롬' 문제에서 풀어본 풀이 중 가장 간단한 코드를 택해보자.<br>
슬라이싱 풀이인데, 당시 풀이를 보면 알겠지만 가장 간단할 뿐만 아니라 성능 또한 매우 좋았다.
```python
def is_palindrome(word):
    return word == word[::-1]
```
이제 n^2번 반복하면서 모든 조합을 구성하고, 매번 팰린드롬 여부인지 체크한다.<br>
전체 코드는 다음과 같다.
```python
def palindromePairs(self, words: List[str]) -> List[List[int]]:
    def is_palindrome(word):
        return word == word[::-1]
        
    output = []
    for i, word1 in enumerate(words):
        for j, word2 in enumerate(words):
            if i == j:
                continue
            if is_palindrome(word1 + word2):
                output.append([i, j])
    return output
```
간단하게 구현했고 실행도 잘 된다.<br>
그러나 리트코드에서는 타임아웃이 발생하며 테스트 케이스를 통과할 수 없다.<br>
이런 형태의 브루트 포스로는 시간 초과로 풀리지 않는다.<br>
좀 더 효율적인 풀이가 필요하다.
<br><br>

#### 풀이2. 트라이 구현
O(n^2)을 O(n)으로 풀이할 수 있는 방법이 있을까? 앞서 문제에서 구현해본 트라이로 풀이를 진행해보자.<br>
먼저 이전 문제에서 풀이했던 트라이 구현을 그대로 가져와본다.
```python
# 트라이를 저장할 노드
class TrieNode:
    def __init__(self):
        self.word = False
        self.children = collections.defaultdict(TrieNode)
        
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    # 단어 삽입
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            node = node.children[char]
        node.word = True
    
    # 단어 존재 여부 판별
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]

        return node.word
```
시작 단어 존재 여부 판별 메소드를 제외한 모든 구현을 가져왔다.<br>
물론 여기서도 메소드 내용 대부분은 변경할 예정이지만 일단 기본적인 뼈대는 동일하다.<br>
이 트라이 구현을 그대로 활용할 것이다.

그렇다면 어떻게 풀어야 할까?<br>
O(n)으로 풀기 위해서는, 모든 입력 값을 트라이로 만들어두고 딱 한 번씩만 탐색하는 문제로 변형할 것이다.<br>
팰린드롬을 판별해야 하기 때문에, 뒤집어서 트라이로 구성하면 해법을 찾을 수 있을 것 같다.

먼저, 이 예제 입력값은 너무 쉬우므로, ['d', 'cbbcd', 'dcbb', 'dcbc', 'cbbc', 'bbcd'] 정도로 좀 복잡한 값을 입력값으로 해서 뒤집어서 그림 16-4와 같이 트라이로 구현해보자.

<img src="https://user-images.githubusercontent.com/55045377/124883779-a527a400-e00c-11eb-9260-dd99439e99f6.png" width=50% height=50%>

입력값을 뒤집으면 ['d', 'dcbbc', 'bbcd', 'cbcd', 'cbbc', 'dcbb']가 되고 이 값의 트라이를 구축한 결과는 위 그림과 같다.<br>
이제 트라이로 삽입하는 코드를 작성해보자.
```python
def insert(self, index: int, word: str) -> None:
    node = self.root
    for i, char in enumerate(reversed(word)):
        ...
        node = node.children[char]
    node.word_id = index
```

























