# 알고리즘 - 비트마스크

* 현대의 모든 CPU는 이진수를 이용해 모든 자료를 표현합니다. 
* 따라서 내부적으로 이진법 관련 연산을 아주 빠르게 할 수 있습니다.
* 이런 특성을 이용해서 정수의 이진수 표현을 자료구조로 쓰는 기법을 비트마스크라 합니다.



### 장점

* 더 빠른 수행 시간
* 더 간결한 코드
* 더 작은 메모리 사용량
* 연관 배열을 배열로 대체



### 사용 방법

```c++
// 공집합 = 상수 0
int A = 0;

// 꽉 찬 집합 = (1 << N) - 1
int F = (1 << 20) - 1;

// 원소 추가
A = A | (1 << p);
    
// 원소 삭제
A = A & ~(1 << p);

// 원소 포함 확인
if (A & (1 << p))	cout << "contain" << endl;

// 원소 토글
A ^= (1 << p);

// 최소 원소
int e = A & -A;

// 최소 원소 지우기
A = A & (A - 1);
```



```c++
// 합집합
int C = A | B;

// 교집합
int D = A & B;

// 차집합 = A - B
int E = A & ~B;

// 합집합 - 교집합
int G = A ^ B;
```



```c++
// 집합 크기
int bitCount(int A) {
    if (A == 0)
        return 0;
    return (A % 2) + bitCount(A / 2);
}
```



```c++
// 집합 순회
for (int S = A; S; S = (S - 1) & A) {
	// S = A의 부분 집합
}
```



### 활용 방법

* 메모이제이션

* 에라토스테네스의 체 (임의의 정수 이하의 소수 탐색)

* 퍼즐 상태 (0~15가 들어있는 4x4 퍼즐 상태 = 64 비트 정수)

* 우선순위 큐 (실제 리눅스 커널의 프로세스 관리에 사용됨)

  

  

### Example

문제: https://www.algospot.com/judge/problem/read/GRADUATION

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int INF = 987654321;
int T, N, K, M, L;
int lecture[12];
int semester[10];
int cache[10][1 << 12];

int bitCount(int n) {
	int ret = 0;
	
	for (int i = 0; i < 12; i++)
		if (n & (1 << i))
			ret += 1;

	return ret;
}

int solution(int answer, int taken)
{
	//K개 이상의 과목을 이미 들은 경우
	if (bitCount(taken) >= K) return 0;

	//M학기가 지난 경우
	if (answer == M) return INF;

	int& res = cache[answer][taken];
	if (res != -1) return res;
	res = INF;

	//이번학기에 들을 수 있는 과목과 아직 안들은 과목
	int canTake = (semester[answer] & ~taken);

	//아직 선수과목을 다 듣지 않은 과목은 제외
	for (int i = 0; i < N; i++) {
		if ((canTake & (1 << i)) && (taken & lecture[i]) != lecture[i])
			canTake &= ~(1 << i);
	}

	//모든 부분집합 중에서 L개 이하의 과목을 듣는경우 다음 학기로 넘어간다.
	for (int take = canTake; take > 0; take = ((take - 1) & canTake)) {
		if (bitCount(take) > L) continue;
		res = min(res, solution(answer + 1, taken | take) + 1);
	}

	//이번학기를 건너뛰는 경우
	res = min(res, solution(answer + 1, taken));
	return res;
}

int main() {
	
	cin >> T;

	while (T--) {
		cin >> N >> K >> M >> L;
		memset(lecture, 0, sizeof(lecture));
		memset(semester, 0, sizeof(semester));
		memset(cache, -1, sizeof(cache));

		for (int R, i = 0; i < N; i++) {
			cin >> R;

			for (int pre_lec, j = 0; j < R; j++) {
				cin >> pre_lec;
				lecture[i] |= pre_lec;
			}
		}

		for (int C, i = 0; i < M; i++) {
			cin >> C;

			for (int num, j = 0; j < C; j++) {
				cin >> num;
				semester[i] |= num;
			}
		}

		int result = solution(0, 0);

		if (result != INF)	cout << result << endl;
		else				cout << "IMPOSSIBLE" << endl;
	}

	return 0;
}
```





