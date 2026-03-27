https://www.acmicpc.net/problem/2042
- 세그먼트 트리
# 회고

일단 누적합으로 풀어봤다. 근데 일반적인 누적합은 아니었다. 계속해서 숫자가 바뀌게된다. 그렇다고 누적합을 구해놓고 숫자를 바꿔도 시간복잡도가 $O(N \times M)$  가 되어 시간초과가 발생한다.

그래서 2가 나올 때 누적합을 구해주는건 어떨까 생각했는데 이제보니 이것도 $O(N \times M)$ 였다...

이 문제는 세그먼트 트리로 푸는거란다.

아직 몰라





일단 long long 변수까지도 의심하자 모든 변수,배열 모두 의심해야된다.

# 풀이

### 누적합 (실패)

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

// 누적합 만들기
void FUNC(vector<long long>& vt, vector<long long>& sum) {

	sum[0] = 0;
	for (int i = 1; i < sum.size(); i++) {
		sum[i] = sum[i - 1] + vt[i];
	}

	//cout << "누적합 만듬" << "\n";
	//for (auto x : sum) {
	//	cout << "sum-" << x << "\n";
	//}
}

int main() {

	int N;
	int M;
	int K;

	cin >> N;
	cin >> M;
	cin >> K;
	vector<long long> vt;
	vt.push_back(0);

	vector<long long> sum(N+1);

	for (int i = 0; i < N; i++) {
		long long tmp1; cin >> tmp1;
		vt.push_back(tmp1);
	}

	for (int i = 0; i < M + K; i++) {
		long long a, b, c;
		cin >> a >> b >> c;

		if (a == 1) { // 1이면 그냥 교체
			vt[b] = c;
		}
		else { // 누적합 구해서 결과 도출
			FUNC(vt, sum);
			//cout << "답-";
			cout << sum[c] - sum[b - 1] << "\n";
		}
	}

	//for (auto x : vt) {
	//	cout << "vt-" << x << "\n";
	//}

	return 0;
}

/*

5 2 2
1
2
3
4
5
1 5 1
2 1 5
1 5 2
2 3 5
*/
```