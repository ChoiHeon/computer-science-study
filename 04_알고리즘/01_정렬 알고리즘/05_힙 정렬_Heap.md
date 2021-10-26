# 힙 정렬

힙 정렬은 최대 힙 트리나 최소 힙 트리를 구성해 정렬하는 방법입니다. 힙의 특성상 최대 혹은 최소 값을 찾는데 특화되어 있기 때문에 정렬하는데에 유용합니다.

힙에 관한 자세한 내용은 [여기](../../03_자료구조/06_힙_Heap.md)에서 확인할 수 있습니다.

<br>

## 특징

* 불안정한 정렬입니다.
  * 키의 값이 같은 요소의 순서가 정렬 후와 일치하는 것을 보장하지 않습니다.



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
arr = [10, 20, 3, 5, 1, 2, 9, 5]
sorted_arr = []

for i in arr:
    heap.push(i)

for _ in range(len(arr)):
    sorted_arr.append(heap.pop())

print(sorted_arr)
```

```
# output
[1, 2, 3, 5, 5, 9, 10, 20]
```

<br>

```c++
void sort(int arr[], int n) {
	int* heap = new int[n];
	
	// 힙 삽입
	for (int i = 0; i < n; i++) {
		heap[i] = arr[i];

		int child = i;
		while (child != 0) {
			int parent = child / 2;

			if (heap[parent] < heap[child])
				break;

			swap(heap[parent], heap[child]);
			child = parent;
		}
	}

	

	// 힙 제거
	for (int i = 0; i < n; i++) {
		arr[i] = heap[0];

		int parent = 0;
		int heap_size = n - i - 1;
		heap[0] = heap[heap_size];

		while (true) {
			int child_1 = parent * 2 + 1;
			int child_2 = parent * 2 + 2;
			int target = 0;

			if (heap_size <= child_1)
				break;

			if (child_2 < heap_size && heap[child_1] > heap[child_2])
				target = child_2;
			else
				target = child_1;

			if (heap[parent] > heap[target]) {
				swap(heap[parent], heap[target]);
				parent = target;
			}
			else {
				break;
			}
		}
	}
    
    delete[] heap;
}


int main() {
	int arr_count = 10;
	int arr[10] = { 10, 30, 40, 50, 20, 1, 0, 4, 11, 3 };

	sort(arr, arr_count);

	for (int i = 0; i < arr_count; i++)
		cout << arr[i] << ' ';
	cout << endl;

	return 0;
}
```

```
// output
0 1 3 4 10 11 20 30 40 50
```

* 정렬 시작시 임시로 힙 배열을 생성합니다.
* 함수 종료시 힙 배열 메모리를 해제합니다.

**[임시 배열을 사용하지 않은 힙 정렬]**

```c++
void heapSort(vector<int>& a) { // 오름차순 정렬
	int n = a.size();

	// create map heap tree
	for (int i = 1; i < n; i++) {
		int c = i;
		int p = (i - 1) >> 1;

		while (c && a[c] > a[p]) {
			swap(a[c], a[p]);

			c = p;
			p = (c - 1) >> 1;
		}
	}

	// erase max heap 
	for (int i = n - 1; i > 0; --i) {
		swap(a[0], a[i]);

		int p = 0;
		int c1 = p * 2 + 1;
		int c2 = p * 2 + 2;

		while (c1 < i) {
			if (c2 < i && a[c1] < a[c2])
				c1 = c2;  // a[c1] > a[c2]가 되도록 값을 변경

			if (a[p] >= a[c1])
				break;  // 힙의 구조를 만족하면 반복문 종료

			swap(a[p], a[c1]);
			p = c1;
			c1 = p * 2 + 1;
			c2 = p * 2 + 2;
		}
	}
}
```

