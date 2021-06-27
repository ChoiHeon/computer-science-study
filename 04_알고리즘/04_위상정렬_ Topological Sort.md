# 알고리즘 - Topological Sort

위상정렬은 어떠한 작업의 선후가 주어질 때, 작업의 순서를 정해주는 알고리즘입니다. 일반적으로 그래프에 사용하며 그래프는 DAG(Directed Acycle Graph, 사이클이 없는 방향 그래프)를 만족해야 합니다.

<br>

## 기본 로직

1. 그래프 내 모든 정점의 진입차수(해당 정점으로 향하는 간선의 개수)를 저장합니다.
2. 진입차수가 0인 정점을 큐 (또는 스택 등)에 삽입합니다.
3. 다음 과정을 반복합니다.
   1. 큐에서 하나의 정점 V 를 pop 합니다.
   2. V와 연결된 간선을 모두 제거합니다.
   3. 간선을 제거했으므로 정점들의 진입차수를 갱신합니다.
   4. 진입차수가 0 인 정점들을 큐에 삽입합니다.
4. 3번 과정에서 pop 된 정점들을 순서대로 반환합니다.

<br>

## 시간복잡도

위상정렬은 각 정점마다 연결된 모든 간선을 제거하기 때문에 시간복잡도는 다음과 같습니다.

> O(V + E)

V와 E는 각각 정점과 간선의 개수입니다.

<br>

## Example

문제 출처: https://www.acmicpc.net/problem/2252

<br>

```python
from collections import deque


n, m = map(int, input().split())
next = {i: set() for i in range(1, n+1)}
prev = {i: set() for i in range(1, n+1)}
for _ in range(m):
    a,b = map(int, input().split())
    next[a].add(b)
    prev[b].add(a)

answer = []
queue = deque(filter(lambda e: not prev[e], range(1, n+1)))

while queue:
    stud = queue.popleft()
    answer.append(stud)
    for i in next[stud]:
        prev[i].remove(stud)
        if not prev[i]:
            queue.append(i)
    del next[stud]

print(' '.join(map(str, answer)))
```

