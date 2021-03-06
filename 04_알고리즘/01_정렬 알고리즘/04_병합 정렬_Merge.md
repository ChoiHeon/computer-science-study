# 병합 정렬

병합 정렬은 배열을 계속 분할하여 최소 크기로 나눈 다음 정렬한 뒤, 병합하는 과정에서 정렬상태를 유지하는 정렬 알고리즘입니다. 분할 정복 알고리즘(Divide & Conquer)을 활용한 방법으로 안정 정렬(Stable Sort)에 속합니다.

> **분할 정복 알고리즘**은 주어진 문제를 나눌 수 없을 때까지 나눈 다음(divide), 병합하면서 문제를 해결(conquer)해나가는 알고리즘입니다. 문제를 나누는 방법에 따라 효율이 달라지며 해당 과정에서 재귀를 사용하면 쉽게 구현이 가능하지마느 효율성이 떨어질 수 있습니다.

> **안정 정렬** 은 정렬되지 않은 상태에서 같은 키값을 가진 원소의 순서가 정렬 후에도 유지되는 정렬입니다. 예를 들어 배열의 두 원소의 값이 각각 A와 B 이고 키는 같을 때, 키를 기준으로 안정 정렬한 결과 A는 여전히 B의 앞에 있게 됩니다.

<br>

## 기본 로직

다음은 배열을 병합 정렬을 이용해 오름차순으로 정렬하는 과정입니다.

1. 배열을 절반으로 분할합니다.
2. 각 배열의 크기가 1이 될 때 까지 (1)번 과정을 반복합니다.
3. 같은 크기의 배열의 병합을 반복합니다. 병합하는 과정에서 다음과 같은 방법으로 정렬합니다.
   1. 배열 A와 B의 값을 저장할 배열 C를 생성합니다.
   2. 배열 A와 B의 앞에 있는 원소의 값을 비교합니다.
   3. 둘 중 작은 값을 꺼내 배열 C에 삽입합니다.
   4.  (2)(3)번 과정을 반복합니다. 만약 둘 중 하나의 배열이 비게되면, 나머지 배열을 C에 삽입합니다.

<br>

## 시간복잡도

배열의 길이가 N일 때, 병합하는 횟수가 *logN*이며, 병합 할 때 크기가 같은 모든 부분 배열의 병합과정에서 N번의 비교가 발생하므로 시간복잡도는 다음과 같습니다.

> O(lNogN)

<br>

## 장단점

* 장점
  * 안정적인 정렬 방법입니다.
  * 입력 데이터의 길이에 따라 정렬되는 시간은 항상 동일합니다.
* 단점
  * 배열(Array)를 정렬하려면 병합할 떄마다 임시 배열이 필요합니다. 공간의 낭비가 발생합니다.
    * 연결 리스트(Linked-List)를 사용하면 별도로 저장 공간을 생성하지 않아도 됩니다. (in-place-sort가 가능합니다.)
  * 배열의 크기가 길 경우, 데이터를 이동하는 횟수가 많이 발생하므로 시간적 손해가 생길 수 있습니다.

<br>

## Example

```python
def merge(s, t):
    ret = []
    i, j = 0, 0

    while i < len(s) and j < len(t):
        if s[i] < t[j]:
            ret.append(s[i])
            i += 1
        else:
            ret.append(t[j])
            j += 1

    if i < len(s):
        ret += s[i:]
    else:
        ret += t[j:]

    return ret


def merge_sort(arr):
    if len(arr) == 1:
        return arr

    m = len(arr) // 2
    s = merge_sort(arr[:m])
    t = merge_sort(arr[m:])

    return merge(s, t)


arr = [21, 10, 12, 20, 25, 13, 15, 22]
sorted_arr = merge_sort(arr)
print(sorted_arr)  # output: [10, 12, 13, 15, 20, 21, 22, 25]
```

* 재귀를 이용한 병합 정렬입니다.
* 배열의 길이가 1이 될 때까지 분할합니다.
* 분할한 배열을 병합해 반환하는 과정을 재귀적으로 반복합니다.

<br>

```c++
void sort(int arr[], int left, int right) {
	if (left == right)
		return;

	int mid = (left + right) / 2;

    // 분할
	sort(arr, left, mid);
	sort(arr, mid + 1, right);

    // 정복
	int i = left;
	int j = mid + 1;
	int k = 0;
	int* temp = new int[right - left + 1];

	while (i <= mid && j <= right) {
		if (arr[i] < arr[j]) {
			temp[k] = arr[i];
			i++;
		}
		else {
			temp[k] = arr[j];
			j++;
		}

		k++;
	}

	if (mid < i) {
		while (j <= right) {
			temp[k] = arr[j];
			j++;
			k++;
		}
	}
	else if (right < j) {
		while (i <= mid) {
			temp[k] = arr[i];
			i++;
			k++;
		}
	}

	for (int t = 0; t < k; t++)
		arr[left + t] = temp[t];

	delete[] temp;
}


int main() {
	int arr_count = 10;
	int arr[10] = { 10, 30, 40, 50, 20, 1, 0, 4, 11, 3 };

	sort(arr, 0, arr_count - 1);

	for (int i = 0; i < arr_count; i++)
		cout << arr[i] << ' ';
	cout << endl;

	return 0;
}
```

* 임시로 동적 할당 배열(temp)를 생성한 뒤, 정렬한 값을 저장합니다.
* 함수 종료 전에 temp의 값을 복사한 다음, 메모리를 해제합니다.
* 정렬할 배열과 같은 크기의 배열을 전역 변수로 선언한다면, 동적으로 임시 배열을 생성할 필요없이 정렬할 수 있습니다.