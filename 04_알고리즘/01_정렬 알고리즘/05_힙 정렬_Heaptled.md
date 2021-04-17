# 힙 정렬

힙 정렬은 최대 힙 트리나 최소 힙 트리를 구성해 정렬하는 방법입니다. 힙의 특성상 최대 혹은 최소 값을 찾는데 특화되어 있기 때문에 정렬하는데에 유용합니다.

힙에 관한 자세한 내용은 [여기](../../03_자료구조/06_힙_Heap.md)에서 확인할 수 있습니다.

<br>

## 기본 로직

다음은 배열을 오름차순으로 정렬하는 방법입니다.

1. 최소 힙을 생성합니다.
2. 배열의 값을 차례차례 최소 힙에 삽입합니다.
3. 정렬된 배열을 저장할 공간을 생성합니다.
4. 생성한 공간에 최소 힙의 값을 제거해 삽입합니다.
5. (4)번 과정을 최소 힙이 빌 때까지 반복합니다.

<br>

## 시간복잡도

배열의 길이만큼 삽입(push)과 제거(delete)를 반복하게 됩니다. 삽입과 제거의 시간복잡도가 `logN`이므로 힙 정렬의 시간복잡도는 다음과 같습니다.

> NlogN

<br>

## Example

다음은 최소 힙 정렬을 이용해 오름차순으로 배열을 정렬하는 과정입니다.

``` python
class Heap:
    def __init__(self):
        self.__data = []

    def push(self, item):
        self.__data.append(item)

        child = len(self.__data) - 1
        while 0 < child:
            parent = (child-1) // 2
            if self.__data[parent] > self.__data[child]:
                self.__data[parent], self.__data[child] = \
                    self.__data[child], self.__data[parent]
                child = parent
            else:
                break

    def pop(self):
        if not self.__data:
            return -1
        if len(self.__data) == 1:
            return self.__data.pop()

        ret = self.__data[0]
        self.__data[0] = self.__data.pop()
        parent = 0

        while True:
            child = [parent * 2 + 1, parent * 2 + 2]
            target = -1

            if child[0] < len(self.__data) and self.__data[child[0]] < self.__data[parent]:
                target = child[0]
            if child[1] < len(self.__data) and self.__data[child[1]] < self.__data[parent]:
                if target == -1:
                    target = child[1]
                elif self.__data[target] > self.__data[child[1]]:
                    target = child[1]

            if target != -1:
                self.__data[parent], self.__data[target] = self.__data[target], self.__data[parent]
                parent = target
            else:
                break

        return ret


# Test
heap = Heap()
input_data = [10, 20, 3, 5, 1, 2, 9, 5]
output_data = []

for i in [10, 20, 3, 5, 1, 2, 9, 5]:
    heap.push(i)

for _ in range(len(input_data)):
    output_data.append(heap.pop())

print(output_data)  # expected output: [1, 2, 3, 5, 5, 9, 10, 20]
```

