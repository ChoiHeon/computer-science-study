# 퀵 정렬

퀵 정렬은 분할 정복 알고리즘을 사용해서 주어진 배열을 정렬합니다. 임의의 데이터를 **pivot(기준, 피벗)**으로 잡고, pivot의 좌우가 pivot보다 작고 크도록 값을 이동시킵니다. pivot으로 인해 발생한 좌우의 부분배열에 대해 앞의 과정을 반복합니다. 또한 `비균등 정렬`입니다.

<br>

## 기본 로직

다음은 배열을 오름차순으로 정렬하는 과정입니다.

1. 배열의 중간에 위치한 데이터를 pivot으로 지정합니다.
2. 배열의 각 왼쪽, 오른쪽 끝을 left, right로 지정합니다.
3. left가 가리키는 데이터가 pivot보다 클 때까지 left를 오른쪽을 이동시킵니다.
   * 이 때 left의 위치는 pivot보다 오른쪽에 있지 않습니다.
4. right가 가리키는 데이터가 pivot보다 작을 때까지 right를 왼쪽으로 이동시킵니다.
   * 이 때 right의 위치는 pivot보다 왼쪽에 있지 않습니다.
5. left와 right의 데이터를 교환합니다.
6. (3)~(5)번 과정을 left가 right보다 크기 전까지 반복합니다.
7. pivot을 기준으로 나뉜 부분배열에 대해 (1)~(6)번 과정을 수행합니다.
8. 부분배열의 크기가 1이 될 때까지 (7)번을 반복합니다.

<br>

## 시간복잡도

만약  pivot를 지정할 때마다 부분배열의 최대값 혹은 최소값으로 지정한다면, pivot을 제외한 부분배열에 대해서 다시 퀵 정렬을 해야 하므로 시간복잡도는 다음과 같습니다.

> O(N<sup>2</sup>)

하지만 이러한 경우는 정렬되어 있는 배열에서 pivot을 맨 앞이나 맨 뒤로 잡는 경우, 즉 최대 또는 최소값을 pivot으로 지정하는 경우에 해당하며 이외에는 다음의 시간복잡도를 갖습니다.

> O(NlogN)

<br>

## 장단점

* 장점
  * 불필요한 데이터의 이동이 적습니다.
  * 한 번 결정된 pivot은 정렬의 대상에서 제외되기 때문에 같은 시간복잡도 *O(NlogN)*을 가지는 정렬 알고리즘 중에서도 가장 빠릅니다.
    * 평균적으로 가장 빠른 정렬 알고리즘입니다.
  * 배열 내에서 교환이 이루어지기 때문에 별도의 공간을 필요로하지 않습니다.
  * 일부 언어(ex.Java)에서는 하나의 배열에 두 개의 pivot을 가지고 정렬하는 Dual Pivot Quick Sort를 사용할 정도로 효율적입니다.
* 단점
  * 불안정 정렬입니다.
  * 최악의 경우 *O(N<sup>2</sup>)* 시간복잡도를 갖습니다.

<br>

# Example

```python
def quick_sort(arr, left, right):
    L = left
    R = right
    pivot = arr[(left + right) // 2]

    while L <= R:
        while arr[L] < pivot:
            L += 1
        while arr[R] > pivot:
            R -= 1

        if L <= R:
            if L != R:
                arr[L], arr[R] = arr[R], arr[L]
            L += 1
            R -= 1

	print(arr)
            
    if left < R:
        quick_sort(arr, left, R)
    if L < right:
        quick_sort(arr, L, right)


arr = [10, 2, 9, 30, 4, 1, 50]
quick_sort(arr, 0, len(arr) - 1)
print(arr)  # expected output: [1, 2, 4, 9, 10, 30, 50]
```

