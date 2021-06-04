# 선택 정렬 (Selection)

선택 정렬은 현재 위치에 들어갈 값을 찾아 정렬하는 알고리즘입니다. 값을 찾을 때 비교하는 방법에 따라 최소 선택 정렬과 최대 선택 정렬로 구분할 수 있습니다.

<br>

## 기본 로직

1. 맨 앞(인덱스=0)부터 마지막까지 원소 중 가장 작은 값을 가진 원소를 탐색합니다.
2. 탐색한 인덱스가 기리키는 원소와 맨 앞 원소를 교환합니다.
3. 다음 인덱스가 마지막 인덱스일 때 까지 (2) 를 반복합니다. 

<br>

## 시간복잡도

각 인덱스에 마다 현재 인덱스부터 마지막 인덱스까지 값을 비교하므로 시간복잡도는 다음과 같습니다.

> O(N<sup>2</sup>)

<br>

## 장단점

* 장점
  * 구현이 간단합니다.
  * 비교 횟수에 비해 값을 교환하는 수는 많지 않기 때문에 많은 교환이 일어나야 하는 자료상태에서 효율적으로 사용될 수 있습니다.
  * 버블정렬과 Big-O는 같지만, 조금 더  빠를 수 있습니다.
    * 선택 정렬은 값의 교환 횟수가 정해져 있습니다.
* 단점
  * 항상 O(N<sup>2</sup>)이 걸리기 때문에 다른 정렬들에 비해 느립니다.

<br>

## Example

**[Python]**

``` python
arr = [8, 3, 5, 1, 9, 0, 4]
n = len(arr)

for i in range(n-1):
    target = i
    for j in range(i+1, n):
        if arr[target] > arr[j]:
            target = j
    arr[i], arr[target] = arr[target], arr[i]

print(arr)
```

```
// output
[0, 1, 3, 4, 5, 8, 9]
```

<br>

**[C++]**

```c++
void sort(int arr[], int n) {
	for (int i = 0; i < n - 1; i++) {
		int target = i;

		for (int j = i + 1; j < n; j++)
			if (arr[j] < arr[target])
				target = j;

		swap(arr[target], arr[i]);
	}
}


int main() {
	int arr_count = 10;
	int arr[10] = { 10, 30, 40, 50, 20, 1, 0, 4, 11, 3 };

	sort(arr, arr_count);

	for (int i = 0; i < arr_count; i++)
		cout << arr[i] << ' ';
	cout << endl;

	return 0;
}
```



