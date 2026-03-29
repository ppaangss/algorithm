
- 그리디
- 우선순위 큐
# 회고

어떤 방식으로 풀어야 할지 생각하고 

어떤 알고리즘과 자료구조를 쓸건지 판단하고 문제를 풀어가기 시작하면 간단한 문제였다.

정렬 + 우선순위 큐를 이용해 시간복잡도 까지 고려하면서 문제를 풀어냈다.

# 풀이

```cpp
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

int main() {

	int N; 
	int K; 
	
	cin >> N;
	cin >> K;

	vector<pair<int, int>> vt;

	for (int i = 0; i < N; i++) {
		int tmp; cin >> tmp;
		int tmp2; cin >> tmp2;
		vt.push_back({ tmp,tmp2 });
	}

	vector<int> bag;

	for (int i = 0; i < K; i++) {
		int tmp; cin >> tmp;
		bag.push_back(tmp);
	}

	sort(vt.begin(), vt.end(), [](auto a, auto b) {
		if (a.first == b.first) return a.second > b.second;
		return a.first < b.first;
	});

	sort(bag.begin(), bag.end());

	//for (auto x : vt) {
	//	cout << "first-" << x.first << " second-" << x.second << "\n";
	//}

	//for (auto x : bag) {
	//	cout << "가방-" << x << "\n";
	//}

	long long sum = 0;

	priority_queue<int> pq; // 가치 , 인덱스 넣어야함.

	int index1 = 0; // 가방의 인덱스
	for (int i = 0; i < K; i++) {
		
		int wei = bag[i];
		//cout << "가방시작 ---" << "\n";
		//cout << "현재 무게-" << wei << "\n";
		while (1) {
			if (index1 >= N) {
				// 이제 보석 순회는 끝났습니다.
				break;
			}

			if (vt[index1].first <= wei) { // 현재 가방에 넣을 수 있다!! 무게가 낮거나 같아.
				//cout << "현재 우선순위큐에 넣는다-" << vt[index1].second << "\n";
				pq.push(vt[index1].second);
			}
			else { // 현재 가방에 넣을 수 없다. 무게가 높아.
				// 순회 종료
				break;
			}

			index1++;
		}

		// 넣을 만큼 넣었다.

		if (pq.empty()) { // 이 가방에는 못넣습니다.
			//cout << "현재 가방 없음" << "\n";
			continue;
		}

		// 이 가방에 가방 큰 가치를 하나 넣을게요
		//cout << "가방에 넣는다-" << pq.top() << "\n";
		sum += pq.top();
		pq.pop();
	}

	cout << sum << "\n";



	return 0;
}

/*
보석 정렬 가능

7
5
1 10
2 5
3 4
2 3
3 10
1 8
1 9
1
1
2
2
3


3
2
3 10
15 100
10 7
15
10



*/
```