https://www.acmicpc.net/problem/3190
- 구현 & 시뮬레이션
# 회고

시뮬레이션 문제 답게 조건이 많고 꼼꼼히 확인하면서 코드를 작성해 나가면 됐다.

아쉬운점 몇가지만 인지하면 될 것 같았다.

먼저 배열이나 벡터에 대한 인덱스는 항상 생각하자. 이번에 회전에 대한 벡터를 주어졌을 때 
회전을 다 한 경우 접근하면 인덱스 밖을 접근해 오류가 생긴다. 답에는 영향이 없었지만 다시 한 번 생각해야한다. ((꼭!!, 실제는 내가 맞았는지 틀렸는지 모르기 때문에 더욱 섬세해야한다.)

두번째로는 데크를 고민해보는 연습이 필요할 것 같다. 데크를 쓰면 상당히 편리한 점이 많다.

2차원 맵 인덱스를 정리해놓자.



# 풀이

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <queue>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; 
	int K;
	int L;

	cin >> N;
	cin >> K;

	vector<vector<int>> v ( N+1, vector<int>(N+1));
	// 1은 뱀
	// 0은 빈 공간
	// 2는 사과

	vector<pair<int, char>> hj;

	for (int i = 0; i < K; i++) {
		int tmp; cin >> tmp;
		int tmp2; cin >> tmp2;

		v[tmp][tmp2] = 2;
	}

	cin >> L;

	for (int i = 0; i < L; i++) {
		int tmp; cin >> tmp;
		char tmp2; cin >> tmp2;
		hj.push_back({tmp,tmp2});
	}

	int time1 = 0;

	// 뱀의 머리 위치
	int nx = 1;
	int ny = 1;
	int cdir = 0; // 0은 ->, 1은 아래, 2는 <-, 3은 위
	int index1 = 0; // 회전 인덱스
	queue<pair<int, int>> qu; // 이동 흔적
	qu.push({ 1,1 });
	v[1][1] = 1;

	while (1) {

		if (cdir == 0) {
			ny++;
		}
		else if (cdir == 1) {
			nx++;
		}
		else if (cdir == 2) {
			ny--;
		}
		else if (cdir == 3) {
			nx--;
		}

		time1++;
		//cout << time1 << "\n";
		//cout << "nx-" << nx << " ny-" << ny << "\n";

		if (!(nx > 0 && nx <= N && ny > 0 && ny <= N)) {
			// 머리가 벽에 띵
			break;
		}
		if (v[nx][ny] == 1) {
			// 머리가 몸에 띵
			break;
		}

		else if (v[nx][ny] == 2) {
			qu.push({ nx,ny });
			v[nx][ny] = 1;
		}

		else if (v[nx][ny] == 0) {
			v[nx][ny] = 1;
			qu.push({ nx,ny });
			pair<int, int> tail = qu.front();
			qu.pop();

			v[tail.first][tail.second] = 0;
		}

		// 방향조절

		if (time1 == hj[index1].first) {
			char hj1 = hj[index1].second;

			if (hj1 == 'D') { // 오른쪽 이동
				cdir = (cdir + 1) % 4;
			}
			else if (hj1 == 'L') {
				cdir = (cdir + 3) % 4;
			}
			index1++;
		}

		//for (int i = 1; i <= N; i++) {
		//	for (int j = 1; j <= N; j++) {
		//		cout << v[i][j] << " ";
		//	}cout << "\n";
		//}cout << "\n";
	}

	cout << time1 << "\n";

	return 0;
}


```