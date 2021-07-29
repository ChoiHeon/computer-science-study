# 플로이드-와샬 (Floyd-Warshall)

* 그래프에서 '모든' 정점에서 '모든' 정점까지의 최단 거리를 구하는 알고리즘입니다.
* 사이클이 없는 경우, 음수 가중치의 처리가 가능합니다.
  * 최단거리 알고리즘 중 하나인 다익스트라는 음수 가중치의 처리가 어렵습니다.

<br>

### 기본 로직

1. 주어진 그래프의 각 간선을 2차원 배열로 표현합니다.
   * 이 때, 간선이 존재하지 않는 정점 사이의 거리는 '무한대'로 표현합니다.
2. 임의의 정점 A와 임의의 두 정점 B, C에 대해 다음 코드를 수행합니다.
   * 현재 까지의 B->C의 거리와 B->A->C의 거리를 비교해 작은 값으로 갱신합니다.
3. 모든 정점에 대해 (2)번 과정을 수행합니다.

<br>

### 시간 복잡도

* 3중 반복문을 사용하기 때문에 시간복잡도는 다음과 같습니다.

> 정점의 개수가 V개일 때, 시간복잡도는
> O(V<sup>3</sup>)  

<br>

### Example

문제 출처: https://www.acmicpc.net/problem/11404

```python
import sys
input = sys.stdin.readline

n = int(input())
m = int(input())
w = [[0 if i == j else float('inf') for j in range(n)] for i in range(n)]

for _ in range(m):
    a, b, c = map(int, input().split())
    w[a-1][b-1] = min(w[a-1][b-1], c)

for k in range(n):
    for i in range(n):
        for j in range(n):
            w[i][j] = min(w[i][j], w[i][k] + w[k][j])

for p in w:
    print(*[0 if e == float('inf') else e for e in p])
```

<br>

### 다익스트라 vs 플로이드-와샬

* 공간복잡도
  * 다익스트라 
    * 인접행렬 : V<sup>2</sup>
    * 인접리스트: V+E
    * 힙, 우선순위 큐: V ~ E
  * 플로이드-와샬
    * V<sup>2</sup>
* 시간복잡도
  * 다익스트라
    * O(V<sup>2</sup>) or O(ElogV)
  * 플로이드-와샬
    * O(V<sup>3</sup>)
* 플로이드-와샬 선택 기준
  * 소스가 간결해야 할 경우
  * 간선의 수가 적을 경우 (다익스트라와 구체적으로 비교)
  * 탐색 과정이 전체 시간에 큰 영향이 없을 경우 (고려)
  * 음의 가중치가 존재할 경우 (음의 사이클은 X)

