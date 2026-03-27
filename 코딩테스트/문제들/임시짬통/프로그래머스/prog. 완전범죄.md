
https://school.programmers.co.kr/learn/courses/30/lessons/389480

일단 처음에는 단순히 하나의 도둑질은 A아니면 B 이기 때문에 조합을 생각해줬다.
그래서 간단하게 next_permutation으로 해봤는데 테스트케이스는 잘 나오는데 시간초과가 역시 문제였다.

그래서 일단 백트래킹으로 가지치기를 잘 해준다면.?
그래도 말이 안됨. 완전탐색은 불가능 시간초과
결국 A또는 B가 하는 경우의 완전탐색으로 할 때 시간복잡도는 2^40 이 된다.

---
완전탐색이 안될 때는 보통 2가지 경우이다. 그리디 or DP

그리디의 경우 불가능하다. 
DP로 해결을 해야할 것 같다.

이 문제는 DP 배낭 문제였다.

i 번째 물건까지 고려했을 때, B도둑의 흔적이 j인 경우, A 도둑의 최소흔적 
이렇게 접근해야한다.

이 문제는 결국 B의 흔적이라는 예산 안에서 A의 흔적을 얼마나 아낄 수 있는가?를 묻는 것이다.
**자료구조**
- **가로축 (j)**: B가 남긴 흔적의 합 ($0$부터 $m-1$까지)
- **세로축 (i)**: 현재까지 살펴본 물건의 개수
- **칸의 값 (Value)**: 그 상황에서 **A가 남긴 최소 흔적**

**상태 전이**
예시: 현재 물건 `info[i] = [A흔적 2, B흔적 3]` 현재 상태가 `dp[i][j] = 5` (즉, i번째 물건까지 훔쳤을 때 B의 흔적이 `j`이고 A의 흔적이 `5`인 상태)라고 가정해 봅시다.
1. **A가 훔칠 때**: B의 흔적(`j`)은 변하지 않습니다. 대신 A의 흔적만 늘어납니다.
    - `dp[i+1][j]` 칸에 `5 + 2 = 7`을 적습니다. (단, 7이 기존에 적힌 값보다 작을 때만!)
2. **B가 훔칠 때**: B의 흔적(`j`)이 3만큼 늘어납니다. A의 흔적은 그대로입니다.
    - `dp[i+1][j + 3]` 칸에 원래 A의 흔적이었던 `5`를 그대로 적습니다.

"B의 흔적 $j$를 고정해두고, 그 상황에서 가능한 A의 흔적 중 가장 작은 놈들만 골라서 다음 물건으로 넘겨주자!"

시간복잡도의 경우 ??

# 풀이


```c++
#include <vector>
#include <algorithm>

using namespace std;

const int INF = 1e9;

int solution(vector<vector<int>> info, int n, int m) {
    int len = info.size();
    
    // dp[i][j]: i번째 물건까지 고려했을 때, B의 흔적이 j일 때 A의 최소 흔적
    // m의 최대값이 120이므로 121 크기로 설정
    vector<vector<int>> dp(len + 1, vector<int>(m, INF));
    
    dp[0][0] = 0;
    
    for (int i = 0; i < len; ++i) {
        int costA = info[i][0];
        int costB = info[i][1];
        
        for (int j = 0; j < m; ++j) {
            if (dp[i][j] == INF) continue;
            
            // 1. A도둑이 훔치는 경우
            int nextA = dp[i][j] + costA;
            if (nextA < n) {
                dp[i + 1][j] = min(dp[i + 1][j], nextA);
            }
            
            // 2. B도둑이 훔치는 경우
            int nextB = j + costB;
            if (nextB < m) {
                dp[i + 1][nextB] = min(dp[i + 1][nextB], dp[i][j]);
            }
        }
    }
    
    int answer = INF;
    // 마지막 물건까지 다 훔쳤을 때, B의 흔적이 0 ~ m-1인 경우 중 A의 최소값 찾기
    for (int j = 0; j < m; ++j) {
        answer = min(answer, dp[len][j]);
    }
    
    return (answer == INF) ? -1 : answer;
}
```