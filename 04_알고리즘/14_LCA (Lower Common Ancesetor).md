# LCA (Lower Common Ancestor)

* 하나의 트리에 속한 임의의 두 노드가 주어졌을 때, 두 노드의 조상 노드 중 깊이가 가장 큰 노드를 구하는 알고리즘입니다.

<br>

### 기본 로직

1. 두 노드 `V`와 `U`가 주어집니다.
2. 이 때 `V`와 `U`의 깊이가 다를 경우, `U`의 깊이가 더 깊다고 가정합니다.
   * 실제 코드에서 `V`가 더 깊을 경우, `V`와 `U`를 스왑(swap)합니다.
3. `U`가 `V`의 깊이와 같을 때까지 `U`를 조상 노드 방향으로 변경합니다.
4. `U`와 `V`를 동시에 조상 노드 방향으로 같을 때까지 변경합니다.

<br>

* 위의 로직을 직관적으로 구현하면 최대 `O(N)`의 시간이 걸리게 됩니다.
* 하지만 (3), (4)번의 과정에서 DP, 세그먼트 트리를 이용해서 해당 시간을 `log` 단위로 수행할 수 있습니다.
* 둘 중에서 DP를 이용한 방법을 알아보고자 합니다.

<br>

### DP 활용

* 이차원 배열의 parent를 선언합니다.

* parent의 점화식은 다음과 같습니다.

  > parent\[u][k] = 노드 u의 2^k만큼 위에 있는 조상 노드
  >
  > parent\[u][k+1] = parent\[parent\[u]\[k]][k]

* 이후 기본 로직의 (3)번 과정에서 다음과 같이 활용할 수 있습니다.

  1. `V`와 `U`의 깊이 차이가 11이라면,
  2. 11을 이진수로 변경했을 때 1011이므로, 아래의 코드를 실행하면 `V`와 같은 깊이의 조상 노드로 변경할 수 있습니다.

  ```c++
  int diff = depth[u] - depth[v];
  
  for (int k = 0; diff; k++) {
  	if (diff % 2)
          u = parent[u][k];
      diff /= 2;
  }
  ```

* (4)번 과정은 다음처럼 변경할 수 있습니다.

  1. 만약 `V`와 `U`가 다르다면,
  2. 가장 위의 노드부터 시작해서 `V`와 `U`의 조상 노드가 다를 때까지 탐색합니다.

  ```c++
  if (u != v) {							
      for (k = MAX_DEPTH; k >= 0; k--)	
          if (parent[u][k] != -1 && parent[u][k] != parent[v][k]) {
              u = parent[u][k];
              v = parent[v][k];
          }
  
      u = parent[u][0]; //  parent[u][0] == parent[v][0]
  }
  ```

<br>

### 시간복잡도

* 2의 진수 단위로 탐색하므로 시간복잡도는 `O(logN)`입니다.

<br>

### Example

문제: https://www.acmicpc.net/problem/11437

```c++
#include <iostream>
#include <cstring>
#include <vector>

using namespace std;

const int MAX_NODE = 50001;
const int MAX_DEPTH = 16;

int n, m;
int depth[MAX_NODE];
int parent[MAX_NODE][MAX_DEPTH];
vector<vector<int>> tree;

void init1(int prev, int current) {
	depth[current] = depth[prev] + 1;

	for (int child : tree[current]) {
		if (child == prev)
			continue;

		parent[child][0] = current;
		init1(current, child);
	}
}

void init2() {

	for (int k = 0; k < MAX_DEPTH - 1; k++)
		for (int v = 1; v <= n; v++)
			if (parent[v][k] != -1)
				parent[v][k + 1] = parent[parent[v][k]][k];
}

int LCA(int v, int u) {
	if (depth[v] > depth[u])
		swap(v, u);

	int diff = depth[u] - depth[v];

	for (int k = 0; diff; k++) {
		if (diff % 2)
			u = parent[u][k];
		diff /= 2;
	}

	if (v != u) {
		for (int k = MAX_DEPTH - 1; k >= 0; k--) {
			if (parent[v][k] != -1 && parent[v][k] != parent[u][k]) {
				v = parent[v][k];
				u = parent[u][k];
			}
		}

		v = parent[v][0];
	}

	return v;
}

int main() {
	
	int v, u;

	scanf("%d", &n);

	tree.assign(n + 1, vector<int>());

	for (int i = 0; i < n - 1; i++) {
		scanf("%d %d", &v, &u);
		tree[v].push_back(u);
		tree[u].push_back(v);
	}

	depth[0] = -1;
	memset(parent, -1, sizeof(parent));
	init1(0, 1);
	init2();

	scanf("%d", &m);
	
	while (m--) {
		scanf("%d %d", &v, &u);
		printf("%d\n", LCA(v, u));
	}


	return 0;
}
```



