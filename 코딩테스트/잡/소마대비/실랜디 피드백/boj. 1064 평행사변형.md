https://www.acmicpc.net/problem/1064
# 회고

수학 관련 함수를 좀 잘 알아놓으면 이런 문제들은 편하겠다.

예를 들면 제곱근, 제곱 등

일직선인거 곱으로 하는거 센스 좋았다. 왜냐 0 나누는 것도 문제였고 double 비교하는건 절대 안됨. 아닌가? 절대 권장하지 않는데 곱하기는 괜찮다. 나누기가 있을 때 문제가 생길 수 있다!!

# 풀이

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <math.h>
using namespace std;

bool check(vector<pair<int, int>>& v) {

	int x1 = v[0].first;
	int y1 = v[0].second;

	int x2 = v[1].first;
	int y2 = v[1].second;

	int x3 = v[2].first;
	int y3 = v[2].second;

	long long disx1 = x2 - x1;
	long long disy1 = y2 - y1;

	long long disx2 = x3 - x1;
	long long disy2 = y3 - y1;

	if (disx1 * disy2 == disx2 * disy1) {
		return false;
	}
	return true;
}

double cal(pair<int, int> a , pair<int, int> b) {
	int x1 = a.first;
	int y1 = a.second;

	int x2 = b.first;
	int y2 = b.second;

	long long sum1 = (x2 - x1) * (x2 - x1);
	long long sum2 = (y2 - y1) * (y2 - y1);

	return sqrt((sum1 + sum2));
}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	vector<pair<int, int>> v;


	for (int i = 0; i < 3; i++) {
		int tmp = 0; cin >> tmp;
		int tmp1 = 0; cin >> tmp1;

		v.push_back({ tmp,tmp1 });
	}

	// 세 점이 일직선인지 확인

	if (!check(v)) {
		cout << "-1" << "\n";
		return 0;
	}



	vector<double> res;
	double res1 = cal(v[0], v[1]);
	res.push_back(res1);
	//cout << res1 << "\n";

	res1 = cal(v[0], v[2]);
	res.push_back(res1);
	//cout << res1 << "\n";

	res1 = cal(v[1], v[2]);
	res.push_back(res1);
	//cout << res1 << "\n";

	vector<double> resRec;

	resRec.push_back(res[0] * 2 + res[1] * 2);
	resRec.push_back(res[1] * 2 + res[2] * 2);
	resRec.push_back(res[0] * 2 + res[2] * 2);

	//for (auto x : resRec) {
	//	cout << x << "\n";
	//}

	double max1 = *max_element(resRec.begin(), resRec.end());
	double min1 = *min_element(resRec.begin(), resRec.end());


	cout << fixed;
	cout.precision(15);
	cout << max1 - min1 << "\n";

	return 0;
}
```



