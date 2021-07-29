# Union Find

유니온 파인드는 상호 베타적 집합 (Disjoint Set, 서로 중복되지 않는 부분 집합들로 나뉘어진 원소들에 대한 정보를 저장하고 조작하는 자료구조)을 생성하는 알고리즘입니다. 주로 어떤 원소가 특정 집합에 속해 있는지, 집합과 집합간의 병합 등을 수행하며, 유니온 파인드 알고리즘으로 인해 생성되는 집합들은 교집합을 가지지 않습니다.

<br>

## 알고리즘 구조 

유니온 파인드는 배열과 같은 형태를 가지고 있습니다. 다음은 유니온 파인드 배열의 초기 상태입니다.

| 인덱스 | 1    | 2    | 3    | ...  | n    |
| ------ | ---- | ---- | ---- | ---- | ---- |
| 값     | 1    | 2    | 3    | ...  | n    |

각 인덱스는 원소의 이름을 의미하며 값은 원소가 속해있는 집합의 번호 (또는 ID)를 의미합니다. 따라서 초기엔 각 원소는 자기 자신만을 원소로 가지고 있는 집합에 속해있습니다. 일반적으로 집합의 번호는 집합 내 가장 작은 인덱스입니다.

따라서 임의의 원소가 특정 집합에 속해있는지 확인하려면 해당 원소의 값이 특정 집합의 번호과 일치하는지를 확인하면 되며, 서로 다른 집합을 하나의 집합으로 병합하기 위해선 하나의 집합에 속한 원소의 값을 나머지 집합의 번호로 변경하면 됩니다.

이를 바탕으로 집합의 실질적인 구조는 트리 형태를 띠고 있음을 추측할 수 있습니다. 집합 번호가 A인 집합은 A라는 인덱스를 가진 원소가 루트인 트리 형태이기 때문입니다. 또한 집합 내 원소의 값은 루트의 인덱스(또는 값)으로 통일되어 있습니다.

<br>

## 기본 로직 1

유니온 파인드 알고리즘은 기본적으로 2가지 동작을 구현해야 합니다. 하나는 임의의 원소가 속한 집합의 번호를 탐색하는 Find, 나머진 임의의 두 집합을 하나의 집합으로 병합하는 Union입니다.

임의의 원소 A의 Find 동작 과정은 다음과 같습니다. A는 인덱스와 같습니다.

1. 인덱스와 값이 일치한다면 인덱스를 반환합니다.
2. 일치하지 않는다면 인덱스가 A의 값인 원소 B에 대해 1번 과정을 수행합니다.

<br>

위의 과정을 코드로 작성하면 다음과 같습니다.

``` python
def find(x):
   	if parents[x] == x:
        return s
    parents[x] = find(parents[x])
    return parents[x]
```

* 결과적으로 트리(집합) 의 루트를 반환하는 것과 동일합니다.
* 재귀를 통해 찾은 집합의 번호를 탐색을 마친 후 전체적으로 갱신합니다.

<br>

## 기본 로직 2

임의의 원소 A와 B가 속한 집합들의 Union 동작 과정은 다음과 같습니다.

1. Find를 통해 A와 B가 속한 집합의 루트들을 반환합니다.
2. 루트들의 값이 같다면 동작을 종료합니다.
3. 다르다면 둘 중 하나의 값을 나머지 루트의 값으로 변경합니다.

<br>

위의 과정을 코드로 작성하면 다음과 같습니다.

```python
def union(x, y):
    x = find(x)
    y = find(y)
    
    if x == y:  # 생략 가능
        return
    
    if x < y:
        parents[y] = x
 	else:
        parents[x] = y
```

* find를 통해 반환한 값이 같아도 결과는 달라지지 않으므로 위의 2번 과정은 생략이 가능합니다.
* 위의 코드에선 더 작은 번호로 변경했습니다.

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

위의 문제는 유니온 파인드를 이용한 크루스칼 알고리즘을 사용했습니다.