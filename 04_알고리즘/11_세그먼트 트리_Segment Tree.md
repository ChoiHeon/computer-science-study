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
/*
node = 세그먼트 트리의 노드
left, right = 현재 노드가 가리키는 배열의 범위
*/
int init(int node, int left, int right) {
	if (left == right)
		return tree[node] = arr[left];

	int mid = (left + right) / 2;
	
	return tree[node] = init(node * 2, left, mid) + init(node * 2 + 1, mid + 1, right);
}
```

<br>

### 갱신

* 루트 노드부터 시작해 목표 인덱스까지 변경합니다.
* 기존의 값을 대체할 값을 인자로 넣는 것이 아닌, 두 값의 차이를 인자로 넣습니다.
* **기존의 값을 알아야 하므로 갱신시 배열의 값도 변경해야 합니다.**

```c++
/*
node = 세그먼트 트리의 노드
left, right = 현재 노드가 가리키는 배열의 범위
index = 배열의 값을 변경할 인덱스
diff = 변경할 값 - 기존의 겂
*/
void update(int node, int left, int right, int index, int diff) {
	if (index < left || right < index)
		return;

	tree[node] += diff;

	if (left != right) {
		int mid = (left + right) / 2;
		
		update(node * 2, left, mid, index, diff);
		update(node * 2 + 1, mid + 1, right, index, diff);
	}
}
```

<br>

### 부분합

* 세그먼트 트리를 구현한 이유입니다.
* 합을 구하는 과정은 4가지로 나눌 수 있습니다. 
* 현재 노드가 가리키는 범위를 [left, right], 구하고자 하는 합의 범위를 [stasrt, end]라고 가정합니다.
  1. [start, end]와 [left, right]가 겹쳐지지 않는 경우
  2. [start, end]가 [left, right]를 포함하는 경우
  3. [left, start]가 [start, end]를 포함하는 경우
  4. (1) (2) (3)을 제외한, [left, right]와 [start, end]가 일부 겹쳐져 있는 경우

```c++
/*
node = 세그먼트 트리의 노드
left, right = 현재 노드가 가리키는 배열의 범위
start, end = 합을 구하려는 배열의 범위
*/
int sum(int node, int left, int right, int start, int end) {
    // (1)
	if (end < left || right < start)
		return 0LL;

    // (2)
	if (start <= left && right <= end)
		return tree[node];

	int mid = (left + right) / 2;
	
    // (3) (4)
	return sum(node * 2, left, mid, start, end) + sum(node * 2 + 1, mid + 1, right, start, end);
}
```

<br>

### Example

문제: https://www.acmicpc.net/problem/2042

```c++
#include <iostream>

using namespace std;
typedef long long ll;

const int MAX_N = 1000000;
const int TREE_SIZE = (1 << (20 + 1)) + 1; // 20 = ceil(log2(MAX_N))

int n, m, k;
ll arr[MAX_N];		// 시작 인덱스 = 0
ll tree[TREE_SIZE];	// 시작 인덱스 = 1


/*
node = 세그먼트 트리의 노드
left, right = 현재 노드가 가리키는 배열의 범위
*/
ll init(int node, int left, int right) {
	if (left == right)
		return tree[node] = arr[left];

	int mid = (left + right) / 2;
	
	return tree[node] = init(node * 2, left, mid) + init(node * 2 + 1, mid + 1, right);
}


/*
node = 세그먼트 트리의 노드
left, right = 현재 노드가 가리키는 배열의 범위
index = 배열의 값을 변경할 인덱스
diff = 변경할 값 - 기존의 값
*/
void update(int node, int left, int right, int index, ll diff) {
	if (index < left || right < index)
		return;

	tree[node] += diff;

	if (left != right) {
		int mid = (left + right) / 2;
		
		update(node * 2, left, mid, index, diff);
		update(node * 2 + 1, mid + 1, right, index, diff);
	}
}


/*
node = 세그먼트 트리의 노드
left, right = 현재 노드가 가리키는 배열의 범위
start, end = 합을 구하려는 배열의 범위
*/
ll sum(int node, int left, int right, int start, int end) {
	if (end < left || right < start)
		return 0LL;

	if (start <= left && right <= end)
		return tree[node];

	int mid = (left + right) / 2;

	return sum(node * 2, left, mid, start, end) + sum(node * 2 + 1, mid + 1, right, start, end);
}


int main() {
	scanf("%d %d %d", &n, &m, &k);

	for (int i = 0; i < n; i++) 
		scanf("%lld", &arr[i]);

	init(1, 0, n - 1);

	int t = m + k;
	int a, b;
	ll c;

	while (t--) {
		scanf("%d %d %lld", &a, &b, &c);

		if (a == 1) {
			update(1, 0, n - 1, b - 1, c - arr[b - 1]);
			arr[b - 1] = c;
		}
		else
			printf("%lld\n", sum(1, 0, n - 1, b - 1, c - 1));
	}

	return 0;
}
```

