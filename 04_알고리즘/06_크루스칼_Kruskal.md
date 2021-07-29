# Kruskal

크루스칼 알고리즘은 그리디(Greedy)한 방법으로 간선에 가중치가 부여된 그래프의 모든 정점을 최소 비용으로 연결하는 방법을 탐색하는 알고리즘입니다. 여기서 그리디한 방법이란 매 순간 가장 좋다고 생각되는 선택을 함으로써 문제의 해를 찾는 방법입니다.

최소 비용으로 정점들을 연결하는 을 최소 스패닝 트리(MST)라고 하며 각 단계에서 사이클을 생성하지 않는 최소 비용 간선을 선택해야 합니다.

<br>

## 기본 로직

1. 그래프의 간선들을 오름차순으로 정렬합니다.
2. 정렬된 간선 리스트에서 가중치가 가장 낮은, 사이클을 형성하지 않는 간선을 선택합니다.
3. 모든 정점이 연결될 때 까지 2번 과정을 반복합니다.

<br>

위의 과정에서 중요한 부분은 2번의 "사이클을 형상하지 않는" 간선을 선택하는 것입니다. 따라서 간선을 선택할 때 마다 간선에 연결된 정점 V와 U가 이미 하나의 트리에 속하는지 판단해야 합니다. 이를 간단히 할 수 있는 방법은 **유니온 파인드(Union Find)**를 사용하는 것입니다.

<br>

## Example

문제 출처: https://www.acmicpc.net/problem/1197

<br>

``` python
import sys
input = sys.stdin.readline

V, E = map(int, input().split())
parents = [*range(V+1)]
edges = []

for _ in range(E):
    edges.append(list(map(int, input().split())))
edges.sort(key=lambda e: e[2])


def find(x):
    if parents[x] == x:
        return x
    parents[x] = find(parents[x])
    return parents[x]


def union(x, y):
    x = find(x)
    y = find(y)
    parents[x] = y


answer = 0
for edge in edges:
    v, u, w = edge
    if find(v) != find(u):
        answer += w
        union(v, u)

print(answer)
```

* 유니온 파인드 알고리즘과 크루스칼 알고리즘이 동시에 적용된 코드입니다.