# 세그먼트 트리 (Segment Tree)

* 배열의 부분합을 효율적으로 계산하는 알고리즘입니다.
* **배열을 이진 트리구조로 만들어야 합니다.**
* **트리의 리프노드는 배열의 값이 저장됩니다.**
* **부모 노드는 양 쪽 자식 노드의 값의 합이 저장됩니다.**
* 노드 클래스를 정의해서 구현할 수 있지만, 일반적으로 배열로 구현합니다.
  * 배열로 구현시 메모리를 효율적으로 사용할 수 있습니다.
  * 아래의 내용은 배열로 구현한 세그먼트 트리에 대한 내용입니다.

<br>

### 세그먼트 트리 크기

* 트리의 높이는 다음과 같습니다.

  > 세그먼트 트리의 높이(H) = ceil(log2(N))

* 배열의 크기가 N일 때, 구현하고자 하는 트리의 크기(배열의 길이)는 다음과 같습니다.

	> 세그먼트 트리의 크기 = N보다 큰 2<sup>K</sup> 중 최소 값 = 2<sup>H + 1</sup>

<br>

### 세그먼트 트리 생성

* 다음은 배열을 이용해 세그먼트 트리를 생성하는 방법입니다.
  * 균형 이진 트리를 배열로 구현하는 것과 유사합니다.
  * 균형 이진 트리와는 달리 사용되지 않는 인덱스가 존재합니다.

* 세그먼트 트리 생성은 재귀를 사용한 분할 & 정복 방식을 통해서 쉽게 구현할 수 있습니다.

* 트리의 부모노드의 인덱스 (index)에 대해 자식 노드의 인덱스는  다음과 같습니다.

  > 루트 노드의 인덱스가 1일 때,
  >
  > 자식 노드1 = index * 2
  >
  > 자식 노드2 = index * 2 + 1

```c++
int init(int* arr, int* tree, int index, int start, int end) {
	if (start == end)
		return tree[index] = arr[start];

	int mid = (start + end) / 2;
	return tree[index] = init(arr, tree, index * 2, start, mid) +
						 init(arr, tree, index * 2 + 1, mid + 1, end);
}
```

<br>

### 갱신

* 루트 노드부터 시작해 목표 인덱스까지 변경합니다.
* 기존의 값을 대체할 값을 인자로 넣는 것이 아닌, 두 값의 차이를 인자로 넣습니다.

```c++
// node: 현재 노드의 번호. 처음 호출할 때 1을 인자로 넣습니다.
// index: 목표 인덱스 (바꾸고자 하는 인덱스)
void update(int* tree, int node, int start, int end, int index, int diff) {
	if (!(start <= index && index <= end))
		return;

	tree[node] += diff;

	if (left != right) {
		int mid = (start + end) / 2;
		update(tree, node * 2, start, mid, index, diff);
		update(tree, node * 2 + 1, mid + 1, end, index, diff);
	}
}
```

<br>

### 부분합

* 세그먼트 트리를 구현한 이유입니다.
* 합을 구하는 과정은 4가지로 나눌 수 있습니다. 
* 현재 노드가 가리키는 범위를 [start, end], 구하고자 하는 합의 범위를 [left, right]라고 가정합니다.
  1. [left, right]와 [start, end]가 겹쳐지지 않는 경우
  2. [left, right]가 [start, end]를 포함하는 경우
  3. [start, end]가 [left, right]를 포함하는 경우
  4. (1) (2) (3)을 제외한, [left, right]와 [start, end]가 일부 겹쳐져 있는 경우

```c++
int sum(int* t, int node, int start, int end, int left, int right) {
    // 1. [left, right]와 [start, end]가 겹쳐지지 않는 경우
    if (left > end || right < start)
        return 0;
    
    // 2. [left, right]가 [start, end]를 포함하는 경우
    if (left <= start && end <= right)
        return tree[node];
    
    int mid = (start + end) / 2;
    return sum(tree, node * 2, start, mid, left, right) + 
           sum(tree, node * 2 + 1, mid + 1, end, left, right);
}
```

<br>

### Example

```c++
#include <iostream>

using namespace std;

int init(int* arr, int* tree, int index, int start, int end) {
	if (start == end)
		return tree[index] = arr[start];

	int mid = (start + end) / 2;
	return tree[index] = init(arr, tree, index * 2, start, mid) +
						init(arr, tree, index * 2 + 1, mid + 1, end);
}

void update(int* tree, int node, int start, int end, int index, int diff) {
	if (!(start <= index && index <= end))
		return;

	tree[node] += diff;

	if (left != right) {
		int mid = (start + end) / 2;
		update(tree, node * 2, start, mid, index, diff);
		update(tree, node * 2 + 1, mid + 1, end, index, diff);
	}
}

int sum(int* tree, int node, int start, int end, int left, int right) {
	// 1. [left, right]와 [start, end]가 겹쳐지지 않는 경우
	if (left > end || right < start)
		return 0;

	// 2. [left, right]가 [start, end]를 포함하는 경우
	if (left <= start && end <= right)
		return tree[node];

	int mid = (start + end) / 2;
	return sum(tree, node * 2, start, mid, left, right) +
		sum(tree, node * 2 + 1, mid + 1, end, left, right);
}

int main() {
	int arr[20];
	
	for (int i = 0; i < 20; i++)
		arr[i] = i;

	int h = (int)ceil(log2(20));
	int size = 1 << (h + 1);
	int* tree = new int[size + 1];

	init(arr, tree, 1, 0, 19);
	cout << sum(tree, 1, 0, 19, 0, 19) << endl;

	delete[] tree;


	return 0;
}
```

