
- 행마다 독립적인 vector
- 행마다 열 길이가 달라도 된다.

## 벡터의 맨 앞, 맨 뒤 원소 접근

```c++
v.front();
v.back();
```

## 선언과 초기화

```c++
#include <vector>
using namespace std;

vector<vector<int>> a;              // 빈 2차원 벡터
vector<vector<int>> b(3);           // 행 3개, 열은 아직 없음
vector<vector<int>> c(3, vector<int>(4));      // 3x4, 전부 0
vector<vector<int>> d(3, vector<int>(4, 7));   // 3x4, 전부 7

```

## 크기 확인

```c++
int n = c.size();        // 행 개수
int m = c[0].size();    // 열 개수 (비어있으면 접근하면 안 됨)
```

## 순회

```c++
vector<vector<int>> v = { {60,50},{30,70},{60,30},{80,40} };
for (int i = 0; i < v.size(); i++) {
	for (int j = 0; j <v[i].size(); j++) {
		// ...
	}
}
```

## 정렬
### 행 전체를 기준으로 정렬
- 기본 정렬 기준 : 사전순 정렬

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {

	vector<vector<int>> v = { {10,2}, {1,3}, {2,4}, {9,8}, {7,7}, {9,1} };
	sort(v.begin(), v.end());

	for (auto a : v) {
		cout << a[0] << ' ' << a[1] << '\n';
	}
}
```

- a[1]을 기준으로 정렬, 같으면 a[0] 기준으로 정렬

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// 비교 함수 직접 작성
bool cmp(vector<int>& v1, vector<int>& v2) {
	if (v1[1] == v2[1])
		return v1[0] < v2[0];
	else
		return v1[1] < v2[1];
}

int main() {

	vector<vector<int>> v = { {10,2}, {1,3}, {2,4}, {9,8}, {7,7}, {9,1} };
	sort(v.begin(), v.end(), cmp);

	for (auto a : v) {
		cout << a[0] << ' ' << a[1] << '\n';
	}
}
```

