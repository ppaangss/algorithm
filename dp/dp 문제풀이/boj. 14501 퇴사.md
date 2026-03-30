# 개요
### [boj. 14501 퇴사](https://www.acmicpc.net/problem/14501)
### 유형

- dp
### 회고

이게 역방향이라는 생각 자체를 하기가 어렵다. 당연히 상태를 날짜로 잡고, 그날마다의 최대수익을 잡으면 될 줄 알았다. 근데 아니었다.

결국에는 현재의 선택이 나중에 영향을 준다. 가령 1일차에 3일짜리 상담을 진행하면 4일의 상태에 영향을 준다. 그러면 1일차의 선택의 유무를 4일차에 결정을 해야하므로 매우 복잡해지고 dp랑은 맞지 않는것이다.

이 문제의 본질은 "오늘 상담을 할지 말지 결정하려면, 상담이 끝난 '이후'에 벌 수 있는 돈을 알아야 한다"는 것이다.

미래의 수익을 안다면 깔끔하게 계산이 가능하므로, 역방향을 고려하는 방향이 맞다.

역방향으로 해야겠다 라는 접근이 너무 어렵다...
# 해결과정

이 문제의 본질은 해당 날짜의 상담을 할 것인가 안 할 것인가를 결정해야 한다.

그러면 결국 해당 날짜를 선택하는게 최대이익인지를 따질려면 미래의 수익을 알고 있어야함.

그러면 결국 역방향으로 문제를 풀어야한다.

맨 마지막날부터 시작해서 해당 날짜 $i$ 에 끝나는 값의 최선을 계속 구하면 된다.

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

	vector<pair<int, int>> vt(N+2,{0,0});

	vector<int> dp(N+2,0); // 0~N+1

	

	for (int i = 1; i <= N; i++) {
		int tmp; cin >> tmp;
		int tmp2; cin >> tmp2;

		vt[i] = { tmp,tmp2 };
	}

	for (int i = N; i > 0; i--) {

		int f1 = vt[i].first;
		int s1 = vt[i].second;

		if (i + f1 > N + 1) {
			dp[i] = dp[i + 1];
		}
		else {
			int num1 = dp[i + 1];
			int num2 = dp[i + f1] + s1;

			dp[i] = max(num1, num2);
		}

		//cout << dp[i] << " ";
	}

	cout << dp[1] << "\n";

	return 0;
}
```