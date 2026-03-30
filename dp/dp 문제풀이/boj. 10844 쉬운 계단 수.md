# 개요

### [boj. 10844 쉬운 계단 수](https://www.acmicpc.net/problem/10844)

### 유형

- dp

### 회고

상태를 찾는 대표적인 dp 문제이다. 이 문제의 상태는 자릿수와 맨 오른쪽 숫자이다. 맨 오른쪽 숫자가 어떻게 되느냐에 따라 다음에 올 수 있는 숫자가 결정되기 때문이다. (상태전이)

# 해결과정

여기서 상태는 먼저 자릿수 N을 잡을 수 있다. 

그다음에 다음 상태에 영향을 주는 것은 마지막 오른쪽 숫자이므로 상태를 잡고 숫자는 총 10개이므로 $dp[N][10]$ 의 형태를 구성한다.

이때 0과 9는 각각 1과 8에서만 올 수 있으므로 예외로 처리해주면 된다.

그리고 0이란 숫자는 없으므로 처음에 한 자리수 일 때 0에만 0을 넣어주면 끝난다.

# 풀이

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

#define MOD 1000000000

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; cin >> N;

	vector<vector<int>> vt(2,vector<int>(10,1));

	vt[0][0] = 0;

	for (int i = 2; i <= N; i++) {
		vt[1][0] = vt[0][1] % MOD;
		for (int j = 1; j < 9; j++) {
			vt[1][j] = (vt[0][j - 1] + vt[0][j + 1]) % MOD;
		}
		vt[1][9] = vt[0][8] % MOD;

		for (int j = 0; j < 10; j++) {
			vt[0][j] = vt[1][j];
			//cout << vt[0][j] << " ";
		}//cout << "\n\n";
	}

	int sum = 0;
	for (int i = 0; i < 10; i++) {
		sum += vt[0][i];
		sum = sum % MOD;
	}

	cout << sum << "\n";


	return 0;
}
```

