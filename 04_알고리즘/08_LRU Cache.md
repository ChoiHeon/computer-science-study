# 알고리즘 - LRU Cache

LRU(Least Recently Used) 알고리즘은 캐쉬에서 메모리를 다루기 위한 알고리즘 중 하나로 가장 최근에 사용한 적이 없는 알고리즘을 캐쉬에서 제거하는 알고리즘입니다.

<br>

## 기본 로직

1. 캐쉬가 가득찰 때까지 호출한 데이터를 삽입합니다.
2. 캐쉬가 가득찬 상태에서 데이터를 호출합니다.
   * 호출한 데이터가 이미 있다면 해당 데이터를 캐쉬의 맨 앞에 위치시킵니다.
     * 캐쉬의 앞에 있을수록 최근에 사용한 데이터임을 의미합니다.
   * 캐쉬에 없다면, 캐쉬 내 데이터 중 사용하지 않은지 가장 오래된 데이터를 삭제한 후 삽입합니다.
     * 이 때, 보통 캐쉬의 맨 뒤에 있는 데이터를 삭제합니다.

<br>

# Example

```python
from collections import deque

size = 4
cache = deque()
dct = dict()


def lru(data):
    if len(cache) != size:
        cache.append(data)
        dct[data] = len(cache)-1
    else:
        if data not in dct:
            del dct[cache.popleft()]
            for key in dct:
                dct[key] -= 1
            cache.append(data)
            dct[data] = size-1
        else:
            idx = dct[data]
            for key in dct:
                if dct[key] > idx:
                    dct[key] -= 1
            del cache[idx]
            cache.append(data)
            dct[data] = size-1


for data in [1, 2, 3, 4, 2, 3, 5, 2, 3, 4, 5]:
    lru(data)
    print("input data: {}, cache: {}".format(data, [*cache]))
```

```
# expected output

input data: 1, cache: [1]
input data: 2, cache: [1, 2]
input data: 3, cache: [1, 2, 3]
input data: 4, cache: [1, 2, 3, 4]
input data: 2, cache: [1, 3, 4, 2]
input data: 3, cache: [1, 4, 2, 3]
input data: 5, cache: [4, 2, 3, 5]
input data: 2, cache: [4, 3, 5, 2]
input data: 3, cache: [4, 5, 2, 3]
input data: 4, cache: [5, 2, 3, 4]
input data: 5, cache: [2, 3, 4, 5]
```

