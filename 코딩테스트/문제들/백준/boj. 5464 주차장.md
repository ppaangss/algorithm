https://www.acmicpc.net/problem/5464
# 회고

문제 알고리즘이 크게 어려운 것은 없었다. 단지 구현문제 였다.

근데 많이 빡셌다. 문제 이해를 잘 해야하고 적절한 자료구조를 사용해보자.

주석을 정말 잘 달아놓는 것도 중요할 것 같다.

# 풀이

```cpp

#include <iostream>
#include <vector>
#include <algorithm> // lower_bound, upper_bound 포함
#include <string>

#include <queue>

using namespace std;

int N;
int M;

struct compare {
    bool operator()(int a,int b) {
        return a > b;
    }
};


int main() {

    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    cin >> N;
    cin >> M;

    priority_queue<int, vector<int>, compare > pq; // 인덱스 기준
    vector<int> v(N+1);
    vector<int> wei(M+1); // 차 단위무게당 요금

    for (int i = 0; i < N; i++) { // 주차장 자리
        int tmp; cin >> tmp;
        v[i] = tmp;
        pq.push(i);
    }

    for (int i = 1; i <= M; i++) { // 차 무게
        cin >> wei[i];
    }

    queue<int> qu; // 대기차량

    int sum = 0; // 총 요금

    vector<int> car_pri(M + 1); // 차가 주차된 자리

    for (int i = 0; i < 2 * M; i++) {
        int tmp; cin >> tmp;

        if (tmp > 0) { // 들어온 차
            if (pq.empty()) { // 자리가 없어
                qu.push(tmp);
            }
            else { // 자리가 있어
                sum += wei[tmp] * v[pq.top()]; // 요금 미리 계산
                
                car_pri[tmp] = pq.top();
                pq.pop(); // 자리 끝

                //cout << "sum-" << sum << "_tmp-" << tmp << "\n";
            }
        }
        else { // 나가는 차
            pq.push(car_pri[(tmp * -1)]); // 차가 주차된 자리는 다시 나옴, 우선순위 큐에 요금 넣기
            while (1) {
                if (qu.empty()) { // 대기중인 차 없음
                    break;
                }
                if (pq.empty()) { // 주차 자리 없음
                    break;
                }

                int tmp1 = qu.front(); // 대기 중인 차
                sum += wei[tmp1] * v[pq.top()]; // 요금 미리 계산
                //cout << "sum-" << sum << "_tmp-" << tmp << "\n";
                car_pri[tmp1] = pq.top(); // 해당 차는 주차장에 넣음
                qu.pop(); // 대기 중인 차 주차함
                pq.pop(); // 자리 끝
                
            }
        }
    }

    cout << sum << "\n";

    return 0;
}

```

