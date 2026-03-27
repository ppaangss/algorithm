https://www.acmicpc.net/problem/2477
# 회고

순수 구현의 문제였다. 근데 좀 어려웠다. 이게 일단 문제에 대한 규칙성 판단하는 것도 오래걸렸고 코드 짜는것도 상당히 어려웠음.

이런거는 뭐 외운다고 되는건 아니고 연습만이 살길인듯.
# 풀이

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>

using namespace std;



int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int K; cin >> K;

	queue<pair<int, int>> qu; // 나중에 순회, 큰 변의 마지막 인덱스 이후로 2,3번째 변 길이를 구하기
	vector<pair<int, int>> v;

	vector<int> val[5]; // 방향 개수
	for (int i = 0; i < 6; i++) {
		int tmp; cin >> tmp;
		int tmp2; cin >> tmp2;

		val[tmp].push_back(tmp2);

		qu.push({ tmp,tmp2 });
	}

	//for (int i = 0; i < 4; i++) {
	//	cout << val[i].size() << "\n";
	//}

	// 큰 변 찾기
	int index1 = -1;
	int index2 = -1;
	int max1 = 1; // 큰 직사각형 넓이
	int max2 = 1; // 작은 직사각형 넓이
	for (int i = 1; i < 5; i++) {
		//cout << "asd" << "\n";
		if (val[i].size() == 1) { // 1이면
			max1 *= val[i][0];

			if (index1 == -1) {
				index1 = i;
				//cout << "index1-" << index1 << "\n";
			}
			else {
				index2 = i;
				//cout << "index2-" << index2 << "\n";
			}
		}
	}

	int cnt1 = 0;

	while (1) {
		pair<int, int> curQu = qu.front();
		int cx = curQu.first;
		qu.pop();
		qu.push(curQu);

		//cout << cx << "\n";

		if (cx == index1 || cx == index2) {
			cnt1++;
		}
		else {
			cnt1 = 0;
		}

		if (cnt1 == 2) {
			break;
		}
	}

	qu.pop(); // 첫번째
	int tmp1 = qu.front().second;
	max2 *= tmp1;

	qu.pop(); //두번째
	tmp1 = qu.front().second;
	max2 *= tmp1;

	cout << (max1 - max2) * K << "\n";

	return 0;
}

/*


7
4 60
2 20
4 100
1 50
3 160
2 30




*/
```