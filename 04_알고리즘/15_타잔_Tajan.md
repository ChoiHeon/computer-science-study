# 타잔 (Tajan)

* 단방향 그래프의 SCC를 탐색하는 알고리즘입니다.
  * SCC (Strongly Connected Component)는 단방향 그래프 내의 사이클을 의미합니다.
  * 즉, 그래프 내 임의의 두 노드 A와 B 사이를 오갈 수 있는 경로가 있어야 합니다.
* DFS를 이용한 트리를 생성하여 그래프의 간선들을 분류합니다.
  * 트리 간선 : 트리에 포함되는 간선
  * 순방향 간선 : 트리의 선조에서 자손으로 이어지는 간선
  * 역방향 간선 : 트리의 자손에서 선조로 이어지는 간선
  * 교차 간선: 트리에서 자손 관계가 성립하지 않는 노드들을 잇는 간선
* SCC 탐색에는 트리 간선과 역방향 간선을 이용합니다.



### 기본 로직

* num[v] = DFS 스패닝 트리 도중 v를 방문한 순서
* low[v] = v가 루트인 서브 트리에서 하나의 간선을 이용해 이동할 수 있는 가장 작은 low
  * 만약 v에서 u로 이어지는 간선이
    * 트리 간선인 경우 : low[v] = min(low[v], low[u])
    * 역방향 간선인 경우 : low[v] = min(low[v], num[u])

* 만약 num[v]와 low[v]가 같을 경우 **SSC**의 탐색에 성공했음을 알 수 있습니다. 



### Example

문제: www.acmicpc.net/problem/2150

```c++
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <stack>

using namespace std;

int n, m;
int order;
int num[10001];
int low[10001];
bool visited[10001];
stack<int> s;
vector<vector<int>> edge;
vector<vector<int>> answer;


void dfs_tree(int v) {
	if (visited[v]) return;
	
	visited[v] = true;
	num[v] = ++order;
	low[v] = order;
	s.push(v);					// 방문한 순서대로 스택에 저장

	for (int u : edge[v]) {
		if (!num[u]) {			// 아직 방문하지 않은 노드, 따라서 edge(v->u)는 트리 간선
			dfs_tree(u);
			low[v] = min(low[v], low[u]);
		}
		else if (visited[u])	// num[u]가 0이 아님과 u를 이미 방문했음을 통해서
            					// u를 루트로 하는 서브트리 내에 v가 있음을 알 수 있음
            					// 따라서 edge(v->u)는 역방향 간선
			low[v] = min(low[v], num[u]);
	}

	if (low[v] == num[v]) {		// v의 자손들 중 v로 오는 간선이 존재함
        						// => SCC를 발견
		vector<int> scc;

		while (!s.empty()) {
			int poped = s.top();
			s.pop();
			visited[poped] = false;
			scc.push_back(poped);

			if (poped == v) break;
		}

		sort(scc.begin(), scc.end());
		answer.push_back(scc);
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	cin >> n >> m;

	memset(num, 0, sizeof(num));
	memset(low, 0, sizeof(num));
	memset(visited, false, sizeof(visited));
	edge.assign(n + 1, vector<int>());

	for (int i = 0; i < m; i++) {
		int v, u;
		cin >> v >> u;
		edge[v].push_back(u);
	}

	order = 0;

	for (int v = 1; v <= n; v++) {
		if (!num[v])	
			dfs_tree(v);
	}

	sort(answer.begin(), answer.end(),
		[](auto& a, auto& b)->bool { return a.front() < b.front(); });

	cout << answer.size() << "\n";
	
	for (auto scc : answer) {
		for (int v : scc)
			cout << v << " ";
		cout << "-1\n";
	}

	return 0;
}

```



