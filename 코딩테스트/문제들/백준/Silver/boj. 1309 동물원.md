# 개요

### 문제

 [문제](https://www.acmicpc.net/problem/1309)
### 문제 유형

- DP
### 회고

나는 마지막 결과에만 `%`를 붙여서 틀렸다. 그러나 이것은 마지막만 해당하는게 아닌 중간과정에서 오버플로우를 방지해야했었다. 

`~로 나눈 나머지를 구하라` 라는 말이 나오면 무조건 계산하는 모든 과정에 `%` 를 붙인다고 생각하자. 


# 해결과정

### 2차원 상태를 이용해 구현

이 문제의 경우 선형 DP라고 볼 수 있다. 왜냐하면 N번째 줄이 직전의 N-1 줄에 대해 영향을 받기 때문이다. 

이 문제의 핵심은 상태를 나누는 것이 중요하다.

N번째 줄에 사자를 배치하는 방법은 딱 3가지 뿐이다.

1. 사자를 아무데도 놓지 않는다. (공백)
2. 왼쪽 칸에만 사자를 놓는다. (왼쪽)
3. 오른쪽 칸에만 사자를 놓는다. (오른쪽)

해당 규칙을 가지고 점화식을 세웠다.

각 상태를 `dp[N][상태]`라고 정의해 보자.

- `dp[i][0]`: $i$번째 줄에 사자를 안 놓는 경우
    
    - 직전 줄($i-1$)이 어떤 상태든 상관없이 어디에든 놓을 수 있다. (안 놓기, 왼쪽, 오른쪽 모두 가능)
    - $dp[i][0] = dp[i-1][0] + dp[i-1][1] + dp[i-1][2]$
        
- `dp[i][1]`: $i$번째 줄 왼쪽에 사자를 놓는 경우
    
    - 직전 줄($i-1$)에 사자가 없거나, 오른쪽에 있어야 합니다. (왼쪽에 있으면 안 됨)
    - $dp[i][1] = dp[i-1][0] + dp[i-1][2]$
        
- `dp[i][2]`: $i$번째 줄 오른쪽에 사자를 놓는 경우
    
    - 직전 줄($i-1$)에 사자가 없거나, 왼쪽에 있어야 합니다. (오른쪽에 있으면 안 됨)
    - $dp[i][2] = dp[i-1][0] + dp[i-1][1]$

여기까지 생각한 뒤에 코드를 구현했다.

### 1차원 DP로 압축

여기까지는 굳이 하지 않아도 되는 과정이다. 단지, 메모리 공간이 매우 빡빡해 2차원을 사용하지 못하고 1차원을 사용해야 하는 경우 필요한 과정일 수 있다.


# 풀이

### 2차원 상태 이용한 dp (실패)

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <unordered_set>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; cin >> N;

	vector<vector<long long>>dp(N + 1, vector<long long>(3, 0));

	// 0은 오른쪽에 사자가 있다.
	// 1은 사자가 없다.
	// 2는 왼쪽에 사자가 있다.
	dp[1][0] = 1;
	dp[1][1] = 1;
	dp[1][2] = 1;

	for (int i = 2; i <= N; i++) {
		dp[i][0] = dp[i - 1][1] + dp[i - 1][2];
		dp[i][1] = dp[i - 1][0] + dp[i - 1][1] + dp[i - 1][2];
		dp[i][2] = dp[i - 1][0] + dp[i - 1][1];
	}

	long long sum = 0;
	for (int i = 0; i < 3; i++) {
		int tmp = dp[N][i] % 9901;
		sum += tmp;
		sum %= 9901;
	}

	cout << sum << "\n";

	return 0;
}
```


### 2차원 상태 이용한 dp (성공)

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int N; 
    if (!(cin >> N)) return 0;

    // dp[i][0]: 오른쪽에 사자
    // dp[i][1]: 사자 없음
    // dp[i][2]: 왼쪽에 사자
    vector<vector<int>> dp(N + 1, vector<int>(3, 0));

    // N=1 초기값
    dp[1][0] = 1;
    dp[1][1] = 1;
    dp[1][2] = 1;

    for (int i = 2; i <= N; i++) {
        // 매 단계마다 9901로 나눠서 숫자가 커지는 것을 방지합니다.
        dp[i][0] = (dp[i - 1][1] + dp[i - 1][2]) % 9901;
        dp[i][1] = (dp[i - 1][0] + dp[i - 1][1] + dp[i - 1][2]) % 9901;
        dp[i][2] = (dp[i - 1][0] + dp[i - 1][1]) % 9901;
    }

    // 최종 결과 출력
    int ans = (dp[N][0] + dp[N][1] + dp[N][2]) % 9901;
    cout << ans << "\n";

    return 0;
}
```

### 