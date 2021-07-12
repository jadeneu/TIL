# 목차
* [chapter 16. 트라이](#16장-트라이)
* [리트코드](#리트코드)
  + [문제 56 트라이 구현](#문제-56-트라이-구현)
  + [문제 56 트라이 구현 풀이](#문제-56-트라이-구현-풀이)
    - [풀이1. 딕셔너리를 이용해 간결한 트라이 구현](#풀이1-딕셔너리를-이용해-간결한-트라이-구현)
  + [문제 57 팰린드롬 페어](#문제-57-팰린드롬-페어)
  + [문제 57 팰린드롬 페어 풀이](#문제-57-팰린드롬-페어-풀이)
    - [풀이1. 팰린드롬을 브루트 포스로 계산](#풀이1-팰린드롬을-브루트-포스로-계산)
    - [풀이2. 트라이 구현](#풀이2-트라이-구현)
    - [문법. @staticmethod 데코레이터](#문법-staticmethod-데코레이터)
<br><br><br>

# 16장 트라이
> 트라이(Trie)는 검색 트리의 일종으로 일반적으로 키가 문자열인, 동적 배열 또는 연관 배열을 저장하는 데 사용되는 정렬된 트리 자료구조다.

트라이(Trie)는 실무에 매우 유용하게 쓰이는 자료구조로서, 특히 자연어 처리(NLP) 분야에서 문자열 탐색을 위한 자료구조로 널리 쓰인다.<br>
트라이는 1959년에 처음 공개됐으며, 검색을 뜻하는 'retrieval'의 중간 음절에서 용어를 따왔다.<br>
'retrieval(리트리벌이라고 읽는다)'에서 따온 단어이기에 초창기에는 '트리'로 발음했으나, 기존의 트리(Tree)와 구분하기 위해 오늘날에는 '트라이'로 불린다.<br>
**트라이는** 트리와 유사하지만, 지금까지 우리가 주로 살펴본 이진 트리의 모습이 아닌 **전형적인 다진 트리(m-ary Tree)의 형태를 띤다.**

트라이는 각각의 문자 단위로 **색인**을 구축한 것과 유사한데, 예를 들어 그림 16-1의 트라이에서 단어 apple을 찾는다고 가정해보자.<br>
수백 개의 문자가 있다고 할 때 이 경우 트라이 탐색을 하면 단 다섯 번 만에 apple 문자열의 존재 여부를 파악할 수 있다.<br>
문자열의 길이만큼만 탐색하면 되기 때문이다.<br>
자연어 처리 분야에서는 형태소 분석기에서 분석 패턴을 트라이로 만들어두고 자연어 문장에 대해 패턴을 찾아 처리하는 등으로 활용하고있다.

* ***색인 : 책 속의 낱말이나 구절, 또 이에 관련한 지시자를 찾아보기 쉽도록 일정한 순서로 나열한 목록을 가리킨다. 인덱스(index)라고도 한다.***<br>

<img src="https://user-images.githubusercontent.com/55045377/124859754-66352680-dfeb-11eb-9e82-907134a6ea33.png" width=50% height=50%>

이 그림은 apple, appear, appeal 등으로 트라이를 구성한 것이다.<br>
여기서 만약 apple을 찾는다면, a → p → p 순으로 문자별 일치하는 노드를 찾아 내려가면 된다.<br>
apple이므로 그다음은 l 노드를 찾아 내려가면 될 것이고, 만약 appear를 찾는다면 e 노드를 찾아 내려가면 된다.<br>
이런 식으로 루트부터 a → p → p → l → e까지 내려가면 단어 apple을 찾을 수 있다.<br>
이처럼 트라이에서는 각 문자열의 길이만큼만 탐색하면 원하는 결과를 찾을 수 있다.

트라이는 문자열을 위한 트리의 형태이기 때문에 사실상 문자 개수만큼 자식이 있어 그림 16-1과 같이 나타내보면 상당히 많은 자식 노드를 갖고 있는 트리임을 확인할 수 있다.<br>
이제 문제 풀이를 통해 트라이를 직접 한번 구현해보자.
<br><br>

## 리트코드
### 문제 56 트라이 구현
> 461p
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
> 466p
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
뒤집은 다음, 문자 단위로 계속 내려가면서 트라이를 구현하고, 각각의 단어가 끝나는 지점에는 단어 인덱스를 word_id로 부여했다.<br>
코드에서는 word_id이고, 그림 16-4에서는 w로 줄여서 표시했다.<br>
이전 문제에서는 True, False 여부만 표기했지만, 여기서는 해당 단어의 인덱스를 찾아야 하기 때문에 word_id로 부여했다.<br>
이제 단어 존재 여부를 찾는 함수의 핵심 코드는 다음과 같다.
```python
results = []
...
while word:
    if node.word_id >= 0:
        ...
        result.append([index, node.word_id])
```
단어를 뒤집어서 구축한 트라이이기 때문에 입력값은 순서대로 탐색하다가, 끝나는 지점의 word_id 값이 -1이 아니라면, 현재 인덱스 index와 해당 word_id는 팰린드롬으로 판단할 수 있다.<br>
예를 들어 그림 16-4에서 입력값 bbcd의 트라이 탐색이 끝나는 지점에는 word_id = 2가 셋팅되어 있고, bbcd 의 인덱스는 5이기 때문에, [5, 2]인 bbcd + dcbb는 팰린드롬이며, 이는 정답 중 하나가 된다.<br>
이 방법이 첫 번째 판별 로직이다.

두 번째 판별 로직은 트라이 삽입 중에 남아 있는 단어가 팰린드롬이라면 미리 팰린드롬 여부를 세팅해 두는 방법이다.<br>
즉 입력값 ['d', 'dcbbc', 'bbcd', 'cbcd', 'cbbc', 'dcbb']에서 cbbc는 단어 자체가 팰린드롬이므로 루트에 바로 입력값의 인덱스인 p = 4를 셋팅하고, word[0:len(word) - i] 형태로 단어에서 문자 수를 계속 줄여 나가며 팰린드롬 여부를 체크한다.<br>
문자가 하나만 남게 될 때는 항상 팰린드롬이므로 마찬가지로 p = 4를 마지막에 셋팅한다.<br>
당연히 이 마지막 값은 항상 w의 바로 앞 노드가 된다.<br>
그림 16-4에 이 알고리즘을 추가로 구현하여 트라이에 표현해보자.<br>
그림 16-5는 이와 같이 팰린드롬 여부인 p를 추가한 트라이다.

<img src="https://user-images.githubusercontent.com/55045377/124906098-ae6f3b80-e021-11eb-9e4d-11ae028db300.png" width=50% height=50%>

이제 이 알고리즘을 추가하여 삽입 함수를 다음과 같이 개선해보자.
```python
def insert(self, index: int, word: str) -> None:
    node = self.root
    for i, char in enumerate(reversed(word)):
        if self.is_palindrome(word[0:len(word) - i]):
            node.palindrome_word_ids.append(index)
        node = node.children[char]
    node.word id = index
```
참고로 그림 16-5에서 p로 표현한 것을 코드에서는 palindrome_word_ids로 풀어서 표현했다.<br>
또한 코드에서는 속성의 이름을 복수형으로 정했는데, 그 이유는 이 그림의 루트 경우처럼 p 값이 여러 개가 될 수 있기 때문이다.<br>
따라서 코드에서 palindrome_word_ids는 리스트이며 복수형이다. <br>
이제 이 로직들을 반영한 각 트라이 노드가 저장될 TrieNode 클래스를 다음과 같이 수정해보자.
```python
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.word_id = -1
        self.palindrome_word_ids = []
```
word_id 외에도 palindrome_word_ids를 트라이 노드의 속성으로 추가하고 TrieNode 클래스로 구현했다.

이제 남아 있는 단어가 팰린드롬인 경우를 좀 더 살펴보자.<br>
그림 16-5에서 w는 단어의 끝이고 p는 w 이전 노드에 반드시 셋팅이 된다.<br>
문자가 하나만 남았을 때는 항상 팰린드롬이기 때문이라고 언급한 바 있다.<br>
다시 한번 입력값 ['d', 'cbbcd', 'dcbb', 'dcbc', 'cbbc', 'bbcd']을 보면 dcbb의 인덱스는 2이고, 트라이에서는 d->c->b->b의 마지막 노드가 p = 1 이다. <br>
그렇다면 [2, 1]은 정답 중 하나가 된다.<br>
실제로 인덱스 1의 입력값은 cbbcd이므로 dcbb + cbbcd는 팰린드롬으로, 정답 중 하나다.

또 다른 경우로, 인덱스 0의 d를 살펴보자.<br>
그림 16-5에서 d 노드는 p = 1이다. <br>
즉 [0, 1]도 정답이 된다.<br>
d + cbbcd이며 마찬가지로 팰린드롬이다.<br>
이 부분을 판별하는 코드는 다음과 같다.
```python
for palindrome_word_id in node.palindrome_word_ids:
    result.append([index, palindrome_word_id])
```

마지막인 세 번째 판별 로직은 입력값을 문자 단위로 확인해 나가다가 해당 노드의 word_id가 -1이 아닐 때, 나머지 문자가 팰린드롬이라면 팰린드롬으로 판별하는 경우다.<br>
dcbc + d가 이에 해당하는데, 입력값 dcbc는 먼저 d부터 탐색하다가 d의 word_id가 -1이 아니기 때문에 나머지 문자 cbc에 대한 팰린드롬 여부를 검사한다.<br>
여기서는 dcbc + d를 팰린드롬으로 판별한다.<br>
그리고 다음과 같은 판별 로직을 코드에 추가한다.
```python
while word:
    if node.word_id >= 0:
        if self.is_palindrome(word):
            result.append([index, node.word_id])
        ...
    node = node.children[word[0]]
    word = word[1:]
```
다소 복잡해 보이지만 3가지 판별 로직을 다시 정리하면 다음과 같다.

1. 끝까지 탐색했을 때 word_id가 있는 경우(그림 16-5에서는 w, 전체 풀이 코드에서는 #1)
2. 끝까지 탐색했을 때 palindrome_word_ids가 있는 경우(그림 16-5에서는 p, 전체 풀이 코드에서는 #2)
3. 탐색 중간에 word_id가 있고(그림 16-5 에서는 w) 나머지 문자가 팰린드롬인 경우(전체 풀이 코드에서는 #3)

이렇게 3가지 경우를 팰린드롬으로 판별할 수 있으며, 입력값을 각각 한 번씩만 대입하면 되기 때문에 O(n)으로 풀이할 수 있다.<br>
좀 더 정확히는 단어의 최대 길이를 k로 했을때 O(k^2(n)) 이며, 앞서 브루트 포스 풀이의 경우는 O(k(n^2))이다.

이 문제에서는 k가 훨씬 더 작기 때문에 트라이 풀이가 더 효율적이며, 이 때문에 앞서 브루트 포스가 타임아웃인 데 반해 트라이 풀이는 시간 내에 잘 실행이 된다.<br>
이제 전체 코드는 다음과 같다.
```python
# 트라이를 저장할 노드
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.word_id = -1
        self.palindrome_word_ids = []
        
class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    @staticmethod
    def is_palindrome(word: str) -> bool:
        return word[::] == word[::-1]
        
    # 단어 삽입
    def insert(self, index: int, word: str) -> None:
        node = self.root
        for i, char in enumerate(reversed(word)):
            if self.is_palindrome(word[0:len(word) - i]):
                node.palindrome_word_ids.append(index)
            node = node.children[char]
            node.val = char
        node.word_id = index
        
    def search(self, index, word) -> List[List[int]]:
        result = []
        node = self.root
        
        while word:
            # 판별 로직  # 3
            if node.word_id >= 0:
                if self.is_palindrome(word):
                    result.append([index, node.word_id])
            if not word[0] in node.children:
                return result
            node = node.children[word[0]]
            word = word[1:]
            
        # 판별 로직  # 1
        if node.word_id >= 0 and node.word_id != index:
            result.append([index, node.word_id])
            
        # 판별 로직  # 2
        for palindrome_word_id in node.palindrome_word_ids:
            result.append([index, palindrome_word_id])
            
        return result
        
class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        trie = Trie()
        
        for i, word in enumerate(words):
            trie.insert(i, word)
            
        results = []
        for i, word in enumerate(words):
            results.extend(trie.search(i, word))
            
        return results
```
이 문제는 트라이와 같은 효율적인 자료구조로 접근해야 풀 수 있는 상당히 어려운 문제인 반면, 트라이의 장점을 십분 발휘할 수 있는 매우 좋은 문제이기도 하다.
<br><br>

* 실제 돌릴 수 있는 코드
```python
from typing import List
import collections

# 트라이를 저장할 노드
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.word_id = -1
        self.palindrome_word_ids = []
        
class Trie:
    def __init__(self):
        self.root = TrieNode()
        
    @staticmethod
    def is_palindrome(word: str) -> bool:
        return word[::] == word[::-1]
        
    # 단어 삽입
    def insert(self, index: int, word: str) -> None:
        node = self.root
        for i, char in enumerate(reversed(word)):
            if self.is_palindrome(word[0:len(word) - i]):
                node.palindrome_word_ids.append(index)
            node = node.children[char]
            node.val = char
        node.word_id = index
        
    def search(self, index, word) -> List[List[int]]:
        result = []
        node = self.root
        
        while word:
            # 판별 로직  # 3
            if node.word_id >= 0:
                if self.is_palindrome(word):
                    result.append([index, node.word_id])
            if not word[0] in node.children:
                return result
            node = node.children[word[0]]
            word = word[1:]
            
        # 판별 로직  # 1
        if node.word_id >= 0 and node.word_id != index:
            result.append([index, node.word_id])
            
        # 판별 로직  # 2
        for palindrome_word_id in node.palindrome_word_ids:
            result.append([index, palindrome_word_id])
            
        return result
        
class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        trie = Trie()
        
        for i, word in enumerate(words):
            trie.insert(i, word)
            
        results = []
        for i, word in enumerate(words):
            results.extend(trie.search(i, word))
            
        return results

solution = Solution()
words = ["abcd","dcba","lls","s","sssll"]
print(solution.palindromePairs(words))
```
<br><br>

### 문법. @staticmethod 데코레이터
앞서 풀이4의 전체 코드에서 처음 보는 부분이 눈에 띨 것이다.<br>
메소드 위에 @staticmethod 라는 표기가 그것인데, 자바에서는 애노테이션(Annotation)이라 부르는 동일한 위치에 있다.<br>
파이썬에서는 이 부분을 **데코레이터(Decorator)** 라 부른다.<br>
그중에서 @staticmethod는 자바의 메소드 static 선언과도 비슷한데, **이렇게 정의한 메소드는 클래스와 독립적으로 함수로서의 의미를 강하게 갖는다.**<br>
실제로 파라미터에도 클래스 메소드에는 항상 따라붙는 self가 빠져 있고, 함수 자체가 별도의 자료형으로 선언되어 있다.

다음과 같이 실험해보자.
```python
class CLASS:
    def a(self):
        pass
        
    @staticmethod
    def b():
        pass
```
이 같은 클래스가 선언되어 있을 때, 함수 a()와 b()의 타입을 출력해보자.
```python
>>> type(CLASS.a), type(CLASS.b)
(<class 'function'>, <class 'function'>)
```
클래스를 생성하지 않고 바깥에서 직접 호출했을 때 타입은 이처럼 둘 다 함수(Function)가 된다.

```python
>>> cls = CLASS()
>>> type(cls.a), type(cls.b)
(<class 'method'>, <class 'function'>)
```
그러나 클래스를 생성한 후에 함수에 대한 타입을 확인해보면, 클래스 내 함수는 이제 메소드(Method)가 된다.<br>
그러나 @staticmethod로 선언한 함수는 여전히 함수임을 확인할 수 있다.<br>
클래스의 메소드가 아니라 여전히 독립된 함수의 의미를 갖는 것이다.<br>
사실상 클래스 바깥에 함수를 별도로 선언한 것과 같은 의미를 지닌다.<br>
이렇게 하면 클래스 메소드처럼 자유롭게 클래스 인스턴스에 접근하는 것이 제한된다.<br>
따라서 **클래스 인스턴스에 접근을 제한하고 분명하게 독립적인 함수로 선언하고자 할 경우 종종 사용된다.**
<br><br>



















