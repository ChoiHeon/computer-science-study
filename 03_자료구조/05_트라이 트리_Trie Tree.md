# 자료구조 - Trie Tree

Trie Tree는 문자열 탐색에 유리한 트리 구조입니다. 특정 문자열의 저장 여부, 특정 문자열로 시작하는 문자열 목록 등을 탐색할 수 있습니다. 



## 노드

Trie Tree의 노드는 하나의 문자(key)와 문자열의 종료 여부(data), 자식 노드들을 리스트 타입의 객체(child를 필드로 가집니다.

필요에 따라 트리의 깊이를 저장하는 depth를 필드를 가질 수 있습니다.



## 초기화

루트 노드는 key 값을 의미없는 값(NULL or None)으로 가집니다. 



## 삽입

삽입하고자 하는 문자열(str)에 대해 각 문자를 순서대로 노드에 저장합니다. 

1. 루트 노드의 자식 노드들의  key와 str의 첫 번째 문자가 일치하는지 탐색합니다.
2. 일치한다면 현재 노드를 해당 자식 노드로 변경합니다.
3. 일치하지 않는다면 첫 번째 문자를 key로 가지는 노드를 생성한 후, 현재 노드를 해당 자식 노드로 변경합니다.
4. 다음 문자에 대해 1, 2, 3번 과정을 진행합니다.
5. 다음 문자가 없을 경우 현재 노드의 data 값을 True로 저장한뒤 종료합니다.



## 시간복잡도

탐색하고자 하는 문자열의 길이가 N일 경우, 시간복잡도는 O(N) 입니다.



## 예시

``` python
class Node:
    def __init__(self, key, data=None):
        self.key = key
        self.data = data
        self.children = {}      	# key : value = character : Node


class Trie:
    def __init__(self):
        self.head = Node(key=None)  # key = None, data = None

    def insert_string(self, string):
        cursor = self.head
        for c in string:
            if c not in cursor.children.keys():
                cursor.children[c] = Node(key=c)
            cursor = cursor.children[c]
        cursor.data = string

    def search(self, string):
        cursor = self.head
        for c in string:
            if c not in cursor.children.keys():
                return False
            cursor = cursor.children[c]
        return cursor.data is not None

    def start_with(self, prefix):
        cursor = self.head
        for c in prefix:
            if c in cursor.children.keys():
                cursor = cursor.children[c]
            else:
                return []

        ret = []
        sub_trie = [cursor]
        while sub_trie:
            node = sub_trie.pop()
            if node.data is not None:
                ret.append(node.data)
            sub_trie += node.children.values()
        return ret


# Test Code
trie = Trie()
trie.insert_string("hello")
trie.insert_string("hi")

print(trie.start_with("h"))
print(trie.start_with("he"))
```

```
# 실행결과
['hi', 'hello']
['hello']
```

