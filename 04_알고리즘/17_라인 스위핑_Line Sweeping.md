# 라인 스위핑 (Line Sweeping) 

* 라인 알고리즘 또는 스위핑 알고리즘이라고도 합니다.
* 주어진 공간을 한 쪽부터 특정 동작을 수행, 마지막까지 지속하여 답을 구하는 것을 의미합니다.

### Example

문제: https://www.acmicpc.net/problem/2261

```c++
#include <iostream>
#include <cmath>
#include <vector>
#include <set>
#include <algorithm>

using namespace std;
typedef pair<int, int> pii;

const int INF = 0x7f7f7f7f;

int n;			// 점들의 개수
vector<pii> A;	// 점들의 집합
set<pii> S;		// 빠르게 원소를 삭제를 하기 위해 set을 사용합니다.

inline int distance(pii a, pii b) { return pow(a.first - b.first, 2) + pow(a.second - b.second, 2); }


int main() {

	scanf("%d", &n);
	
	A.assign(n, pii());
	
	for (int i = 0; i < n; i++)	
		scanf("%d %d", &A[i].first, &A[i].second);
		
	sort(A.begin(), A.end()); // 입력 받은 좌표를 오름차순으로 정렬합니다.
	
    // 끝에 있는 두 좌표를 집합에 넣습니다. 이 때, y좌표로 정렬되도록 (y, x)로 변경합니다.
    S.insert({ A[0].second, A[0].first }); 
	S.insert({ A[1].second, A[1].first });

	int min_dist = distance(A[0], A[1]);
	int k = 0;

	for (int i = 2; i < n; i++) {
        
        // x좌표 거리가 최소 거리보다 작은 A[k]가 집합에 남도록 합니다.
		while (k < i) {
			if (pow(A[i].first - A[k].first, 2) <= min_dist)
				break;
			else {
				S.erase({ A[k].second, A[k].first }); // x값의 차이가 최소 거리보다 크다면, 후보에서 제외합니다.
				k++;
			}

		}

		// y좌표 거리가 최소 거리보다 작은 범위를 구함
		auto start = S.lower_bound({ A[i].second - sqrt(min_dist), -INF });
		auto end = S.lower_bound({ A[i].second + sqrt(min_dist), INF });

		// 후보군에 있는 점들을 이용해 최소 거리를 갱신
		for (auto itr = start; itr != end; itr++)
			min_dist = min(min_dist, distance({ itr->second, itr->first }, A[i]));
	
		// 현재 기준으로 삼은 점 A[i]를 후보군에 삽입
		S.insert({ A[i].second, A[i].first });
	}
	
	printf("%d\n", min_dist);

	return 0;
}
```

