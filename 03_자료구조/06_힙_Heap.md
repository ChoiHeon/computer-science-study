# 힙 (Heap)

힙은 가지고 있는 데이터 중 가장 큰 값 또는 가장 작은 값을 효율적으로 찾을 수 있는 자료구조입니다. 이러한 특징 때문에 `우선순위 큐`를 구현하는데 가장 효율적입니다.

<br>
힙은 완전 이진 트리의 일종으로 완전히 정렬 되어 있는 상태가 아닌, 반정렬 상태를 유지합니다. 즉, 최소 혹은 최대 값을 즉시 찾을 수 있지만, 다른 데이터들은 완벽히 정렬되어 있지 않습니다.

> 완전 이진 트리(Complete Binary Tree)의 특징은 다음과 같습니다.
>
> * 리프 노드들의 레벨이 같습니다.
> * 리프 노드를 제외한 모든 노드들은 자식 노드 2개를 갖습니다.
> * 노드가 제거될 땐 항상 리프노드의 오른쪽부터 제거합니다.

<br>

## 특징

다음은 `최소 힙`의 특징입니다.

* 루트의 키 값은 항상 최소 값을 가져야 합니다.
* 반정렬 상태를 유지합니다.
  * 부모 노드의 키 값은 항상 자식 노드의 키 값보다 작습니다. (최소 힙)

* 완전 이진 트리 구조를 같습니다.
  * 따라서 배열을 통해서 구현하기 유용합니다.
  * 인덱스 i에 대해서 자식의 노드는 다음과 같습니다. (배열의 인덱스가 1부터 시작할 경우)
    * left child = i*2
    * right child = i*2+1
  * 배열의 인덱스가 0 부터 시작한다면 자식 노드는 다음과 같습니다.
    * left child = i * 2 + 1
    * right child = i * 2 + 2
  * 반대로 부모 노드의 인덱스는 다음과 같이 알 수 있습니다.
    * parent = int(i / 2)

<br>

## 원리

힙의 가장 중요한 로직인 삽입과 정렬에 대해 알아보겠습니다. 힙은 최소 힙으로 가정합니다.

* 삽입 (push)
  1. 삽입할 값을 키 값으로 갖는 노드를 생성합니다.
  2. 완전 이진 트리의 구조에 따라 생성한 노드를 부모 노드와 연결합니다.
  3. 부모 노드의 키 값보다 생성한 노드의 키 값이 작을 경우 값을 교환합니다.
  4. 부모 노드의 키 값이 더 작을 때 까지 (3)번 과정을 반복합니다.

<br>

* 제거 (pop)
  1. 루트가 가장 작은 값을 가지고 있으므로 루트의 값을 꺼냅니다.
  2. 트리의 마지막 노드(리프노드 중 가장 오른쪽에 있는 노드를 제거하고 해당 키 값을 루트 노드의 키 값에 저장합니다.
  3. 현재 노드의 키 값이 자식 노드의 키 값보다 크다면 값을 교환합니다. 
     * 만약 두 자식 노드의 키 값이 둘 다 작다면, 더 작은 값과 교환합니다.
  4. 힙 구조에 위배되지 않을 때 까지 (3)번 과정을 반복합니다.



<br>

## 시간복잡도

중요 로직인 삽입과 제거는 트리의 깊이 만큼 반복하므로 시간복잡도는 다음과 같습니다.

> 삽입: logN
>
> 제거: logN

<br>

## Example

다음은 배열을 이용해 최소 힙을 구현한 코드입니다.

```python
class Heap:
    def __init__(self):
        self.__data = []

    def push(self, item):
        self.__data.append(item)  # 가장 마지막에 노드를 삽입
        child = len(self.__data) - 1  # child = 삽입한 노드
        
        while 0 < child:  # 삽입한 노드가 루트 노드라면 반복문 종료
            parent = (child - 1) // 2  # parent = 삽입한 노드의 부모 노드
            
            if self.__data[parent] > self.__data[child]:  # 부모 노드의 키 값이 더 크다면 값을 교환
                self.__data[parent], self.__data[child] = self.__data[child], self.__data[parent]
                child = parent
            else:  # 힙의 구조를 만족한다면 반복문 종료
                break

    def pop(self):
        if not self.__data:  # 배열이 비어있다면 -1을 반환
            return -1
        if len(self.__data) == 1:  # 배열의 길이가 1이라면 맨 앞의 값을 제거 및 반환
            return self.__data.pop()

        ret = self.__data[0]  # ret = 반환할 값
        self.__data[0] = self.__data.pop()  # 가장 마지막 노드의 키 값을 루트의 키 값으로 저장
        parent = 0  # parent = 현재 확인해야 할 노드
        
        while True:
            child = [parent * 2 + 1, parent * 2 + 2]
            target = -1

            # 두 자식 노드 중 더 작은 키 값르 가지는 노드를 탐색
            if child[0] < len(self.__data) and self.__data[child[0]] < self.__data[parent]:
                target = child[0]
            if child[1] < len(self.__data) and self.__data[child[1]] < self.__data[parent]:
                if target == -1:
                    target = child[1]
                elif self.__data[target] > self.__data[child[1]]:
                    target = child[1]

       		# 자식 노드 중 부모 노드보다 작은 키 값을 가진 노드가 없을 경우 반복문 종료, 아니라면 키 값을 교환
            if target != -1:
                self.__data[parent], self.__data[target] = self.__data[target], self.__data[parent]
                parent = target
            else:
                break

        return ret


# Test
heap = Heap()
arr = [10, 20, 3, 5, 1, 2, 9, 5]
sorted_arr = []

for i in arr:
    heap.push(i)

for _ in len(arr):
    sorted_arr.append(heap.pop())

print(sorted_arr) 
```

```
// output
[1, 2, 3, 5, 5, 9, 10, 20]
```

<br>