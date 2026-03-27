https://www.acmicpc.net/problem/6064
# 회고

최소공배수 뭐시기 그런거

..





```cpp

#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

#include <queue>

using namespace std;


int func() {

    int N; cin >> N;
    int M; cin >> M;
    int x; cin >> x;
    int y; cin >> y;

    // 큰거를 N에 넛자!!

    if (N < M) {
        int tmp = N;
        N = M;
        M = tmp;

        tmp = x;
        x = y;
        y = tmp;
    }

    int n = 0;
    int m = 0;

    while (1) {
        if (n > M) { // 없는 거 아닌가?
            break;
        }
        while (1) { // m 반복
            int num1 = N * n + x;
            int num2 = M * m + y;

            if (num1 == num2) { // 발견!!
                
                //cout << "M-" << M << "_N-" << N << "\n";
                //cout << "n-" << n << "_m-" << m << "\n";
                //cout << "x-" << x << "_y-" << y << "\n";
                //cout << "\n";

                return N * n + x;
            }
            else if (num1 < num2) { // 커지면 더 이상 발견 안해도 됨.
                break;
            }
            m++;
        }

        n++;
        m = 0;
    }
    return -1;
}

int main() {
    // 입출력 속도 최적화
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int T; cin >> T;

    vector<int> res;

    for (int i = 0; i < T; i++) {
        res.push_back(func());
    }

    for (auto x : res) {
        cout << x << "\n";
    }



    return 0;
}

/*




*/

```