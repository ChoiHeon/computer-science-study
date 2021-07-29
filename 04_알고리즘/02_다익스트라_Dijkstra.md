# Dijkstra

Dijkstra 알고리즘은 음의 가중치가 없는 그래프에서 임의의 한 정점에서 모든 정점까지의 최단거리를 구할 수 있는 알고리즘입니다. 이 때 그래프 방향의 유무는 상관이 없습니다.



## 기본 로직

1. 임의의 정점 v를 선택합니다.
2. 길이가 정점의 개수인 리스트 costs를 생성합니다. 
3. costs의 원소를 최대값으로 초기화합니다. costs[u]는 v에서 u까지 최단거리를 의미합니다.
4. costs[v]의 값을 0으로 변경합니다.
5. v에서 이동할 수 있는 정점들 u에 대해 costs[v] + graph\[v][u] < costs[u]가 성립한다면, costs[u]의 값을 costs[v] + graph\[v][u]으로 갱신합니다.
6. v에서 갈 수 있는 정점들 중 거리가 가장 짧은 정점에 대해 3~5번 과정을 반복합니다. 이 때 이미 탐색을 끝낸 정점은 제외합니다.



### 시간복잡도

일반적인  Dijkstra 알고리즘은 모든 정점에 대해서 순서에 상관없이 최소 거리를 탐색하므로 **O(V<sup>2</sup>)**이었으나 ,우선순위 큐나 힙에 의해 임의의 정점에서 가장 최단거리로 이동할 수 있는 정점을 빠르게 찾을 수 있게 되어 최소한의 반복으로 거리를 갱신할 수 있게 되었습니다.

따라서 <u>우선순위 큐</u> 또는 <u>힙</u>을 이용한 시간복잡도는 힙의 최대 크기를 알아야할 필요가 있습니다. 모든 간선이 검사될 떄마다 위의 자료구조에 데이터가 추가디고 횟수는  **O(logE)**라고 할 수 있습니다. 또한 삽입 및 삭제에 O(logE)가 걸리므로 총 시간복잡도는 O(ElogE)가 됩니다. 다만 E는 **V**<sup>**2**</sup>보다 작으므로 **O(logE)**는 **O(logV)**로 볼 수 있습니다. 따라서 시간복잡도는 다음과 같습니다. 

> *O(E log V)*

##  Example

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/12978

<br>

해당 문제는 1번 정점에서 모든 정점까지의 최단거리를 알아낸 다음, 주어진 값 이하의 최단거리의 개수를 찾는 문제입니다. 벨만-포드 알고리즘도 가능하지만, <u>시간복잡도에서 이득을 얻을 수 있기 때문에</u> Dijkstra 알고리즘의 사용이 가장 적절합니다.

```python
import heapq

def solution(n, road, k):
    graph = [[float('inf')]*n for _ in range(n)]

    for v, u, c in road:
        graph[v-1][u-1] = min(graph[v-1][u-1], c)
        graph[u-1][v-1] = min(graph[u-1][v-1], c)

    # ------ Dijkstra -----------------------
    costs = [float('inf')] * n
    costs[0] = 0
    heap = [[costs[0], 0]]

    while heap:
        c, v = heapq.heappop(heap)
        if costs[v] < c:
            continue
        for u in range(n):
            if graph[v][u] == float('inf'):
                continue
            if c + graph[v][u] < costs[u]:
                costs[u] = c + graph[v][u]
                heapq.heappush(heap, [costs[u], u]) 
    # ----------------------------------------

    return len([*filter(lambda e: e <= k, heap)])
```

