# 개요
### [boj. 1912 연속합]([boj.%201912.md](https://www.acmicpc.net/problem/1912))

### 유형

- dp

### 회고

dp를 어떻게 정의하냐도 굉장히 중요한 과정인 것 같다.

이 문제의 경우 dp는 $i$ 번째 까지의 최대 연속합을 정의하는 것이 매우 중요했다.
# 해결과정

선형 dp + 조건 (연속성) 이 달린 문제였다.

$dp[i]$ 를 $i$ 번째 까지의 최대 연속합을 뜻하도록 정의했다.

그러면, 이 문제에서 dp 조건은 해당 $dp[i]$ 는 해당 숫자 구간 $num[i]$ 를 반드시 포함해야 해야하는 것이 조건이다.

왜냐하면 연속성이라는 조건을 유지하기 위해서이다. 만약 $dp[i]$ 가 $num[i]$ 를 포함하지 않아도 된다고 정의하는 순간 연속성을 유지할 수 없다.

$i+1$ 번째 값을 계산할 때, 이 값이 앞의 덩어리와 이어지려면 반드시 바로 직전인 $i$ 번째 값이 포함되어 있어야만 한다.

그래서 dp를 구한 다음 dp에서 최댓값을 구하면 그것이 정답이다.

# 풀이

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; cin >> N;

	vector<int> num(N);

	for (int i = 0; i < N; i++) {
		cin >> num[i];
	}

	vector<int> dp(N);
	dp[0] = num[0];

	for (int i = 1; i < N; i++) {
		
		dp[i] = num[i];
		if (dp[i - 1] > 0) {
			dp[i] += dp[i - 1];
		}
	}

	//for (int i = 0; i < N; i++) {
	//	cout << dp[i] << " ";
	//}cout << "\n";

	int answer = *max_element(dp.begin(), dp.end());

	cout << answer << "\n";
	return 0;
}

/*

*/
```