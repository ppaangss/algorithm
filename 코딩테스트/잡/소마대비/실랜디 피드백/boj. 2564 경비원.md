https://www.acmicpc.net/problem/2564
# 회고

조건문 떡칠로 풀었다.

근데 그거보다는 이제 일직선 상이라고 생각하고 하면 더 쉽게 풀 수 있다.

# 풀이

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

using namespace std;

int N;
int M;

int func(pair<int,int> s , pair<int, int> e) {

	// 시계
	int dis1 = 0;

	int s1 = s.first;
	int s2 = s.second;
	while (1) {

		if (s1 == 1) { // 현재 위치가 북쪽
			s2++;
		}
		else if (s1 == 2) { // 현재 위치가 남쪽
			s2--;
		}
		else if (s1 == 3) { // 서
			s2--;
		}
		else if (s1 == 4) { // 동
			s2++;
		}

		//cout << s1 << s2 << "\n";
		dis1++;

		if (s1 == e.first && s2 == e.second) {
			break;
		}

		if (s1 == 1) { // 현재 위치가 북쪽
			if (s2 == N) {
				s1 = 4;
				s2 = 0;
			}
		}
		else if (s1 == 2) { // 현재 위치가 남쪽
			if (s2 == 0) {
				s1 = 3;
				s2 = M;
			}
		}
		else if (s1 == 3) { // 서
			if (s2 == 0) {
				s1 = 1;
				s2 = 0;
			}
		}
		else if (s1 == 4) { // 동
			if (s2 == M) {
				s1 = 2;
				s2 = N;
			}
		}
	}

	//cout << "-----\n";
	// 반시계
	s1 = s.first;
	s2 = s.second;

	int dis2 = 0;
	while (1) {

		if (s1 == 1) { // 현재 위치가 북쪽
			s2--;
		}
		else if (s1 == 2) { // 현재 위치가 남쪽
			s2++;
		}
		else if (s1 == 3) { // 서
			s2++;
		}
		else if (s1 == 4) { // 동
			s2--;
		}

		//cout << s1 << s2 << "\n";
		dis2++;

		if (s1 == e.first && s2 == e.second) {
			break;
		}

		if (s1 == 1) { // 현재 위치가 북쪽
			if (s2 == 0) {
				s1 = 3;
				s2 = 0;
			}
		}
		else if (s1 == 2) { // 현재 위치가 남쪽
			if (s2 == N) {
				s1 = 4;
				s2 = M;
			}
		}
		else if (s1 == 3) { // 서
			if (s2 == M) {
				s1 = 2;
				s2 = 0;
			}
		}
		else if (s1 == 4) { // 동
			if (s2 == 0) {
				s1 = 1;
				s2 = N;
			}
		}
	}

	return min(dis1,dis2);
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> N;
	cin >> M;

	int K; cin >> K;

	int min1 = 0; // 최단 거리 합

	vector<pair<int, int>> v;
	for (int i = 0; i < K; i++) {
		int tmp1; cin >> tmp1;
		int tmp2; cin >> tmp2;
		v.push_back({ tmp1,tmp2 });
	}

	int h1; cin >> h1;
	int h2; cin >> h2;

	pair<int, int> s1 = { h1,h2 };
	pair<int, int> e1 = v[0];

	for (int i = 0; i < K; i++) {
		e1 = v[i];
		//cout << func(s1, e1) << "\n";
		min1 += func(s1, e1);
	}

	cout << min1 << "\n";


	return 0;
}
```