
다익스트라로 풀었음.
모든 경우의 수를 구해야 하기 때문에 플로이드 워셜을 쓰는게 더 좋았다.

## 풀이

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

// 컨테이너
#include <unordered_map>
#include <queue>

#define INF 87654321

using namespace std;

struct graph {
    int node;
    int dis;
};

vector<graph> gr[101];


int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    int N; cin >> N;
    int M; cin >> M;
    int R; cin >> R;

    vector<int> item(N + 1);
    for (int i = 0; i < N; i++) {
        int tmp; cin >> tmp;
        item[i + 1] = tmp;
    }
    vector<vector<int>> v(N + 1, vector<int>(N + 1, INF));
    for (int i = 1; i <= N; i++) {
        v[i][i] = 0;
    }

    for (int i = 0; i < R; i++) {
        int a1, a2, a3;
        cin >> a1 >> a2 >> a3;

        v[a1][a2] = a3;
        v[a2][a1] = a3;
    }

    for (int k = 1; k <= N; k++) {
        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                if (v[i][j] > v[i][k] + v[k][j]) {
                    v[i][j] = v[i][k] + v[k][j];
                }
            }
        }
    }

    int max = 0;
    for (int i = 1; i <= N; i++) {
        int sum = 0;
        for (int j = 1; j <= N; j++) {
            if (v[i][j] <= M) {
                sum += item[j];
            }
        }

        if (max < sum) {
            max = sum;
        }
    }
    
    cout << max << "\n";
}
```