# SPFA

* SPFA(Short Path Faster Algorithm)은 벨만-포드 알고리즘을 개선한 알고리즘입니다.
* 그래프의 임의의 한 정점에서 모든 정점까지의 최단거리를 구할 수 있습니다.
* 벨만-포드를 개선한 알고리즘이기 때문에 음의 사이클의 존재 여부를 확인할 수 있습니다.



### 시간복잡도

> 시간복잡도 = O(V*E)
> 실제로는 O(V+E)



### Example

```c++
int dist[V];
int cycle[V];
bool inQ[V];
queue<int> q;

// 임의의 한 정점을 1이라 가정
dist[1] = 0;
cycle[1] = 1;
inQ[1] = true;
q.push(1);

while (!q.empty()) {
    int v = q.top();
    q.pop();
    inQ[v] = false;
    
    for (auto e: edges[v]) {
        int u = e.first;
        int c = e.second;
        
        if (dist[u] > dist[v] + c) {
            dist[u] = dist[v] + c;
            
            if (!inQ[u]) {
                cycle[u]++;
                
                if (cycle[u] >= V) {
                    printf("음수 사이클 발견");
                    return;
                }
                
                q.push(u);
                inQ[u] = true;
            }
        }
    }
}
```

