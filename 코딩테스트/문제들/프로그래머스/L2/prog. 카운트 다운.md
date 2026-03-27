https://school.programmers.co.kr/learn/courses/30/lessons/131129

- BFS
- DP

# 회고

일단 경우의 수가 너무 많다. 1,2,3,4 ... 총 세보니 42가지였다. 

근데 일단 먼저 그리디를 해보았는데 전혀 답이 나올 것 같지 않았다.

그리디가 안되니까, 완전탐색 BFS로 가보자. 가능은 하다.

난 DP로 다시 풀었다. 꽤 간단하게 순조롭게 풀림.

아쉬웠던 것은 일단은 보통 정해져있는 값들은 배열에 넣고 시작하는게 무조건 이득이다. 코드로 풀어헤치는 것보다.

그래서 나는 처음에 모든 경우의 수를 일일히 구해서 배열에 넣었는데 그럴 필요 없이 코드로 구해서 넣을 수 있었다. 이게 참 아쉬웠다.
# 풀이

내 DP 풀이

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;

int dp[100005][2]; // 첫번째는 최소다크, 두번째는 최대 싱글, 불

struct point{
    int num; // 숫자
    int cnt; // 다트 최소 횟수
    int ball; // 싱글, 불
};

point dPoint[42] = {
    {1,1,1},
    {2,1,1},
    {3,1,1},
    {4,1,1},
    {5,1,1},
    {6,1,1},
    {7,1,1},
    {8,1,1},
    {9,1,1},
    {10,1,1},
    {11,1,1},
    {12,1,1},
    {13,1,1},
    {14,1,1},
    {15,1,1},
    {16,1,1},
    {17,1,1},
    {18,1,1},
    {19,1,1},
    {20,1,1},
    {21,1,0},
    {22,1,0},
    {24,1,0},
    {26,1,0},
    {27,1,0},
    {28,1,0},
    {30,1,0},
    {32,1,0},
    {33,1,0},
    {34,1,0},
    {36,1,0},
    {38,1,0},
    {39,1,0},
    {40,1,0},
    {42,1,0},
    {45,1,0},
    {48,1,0},
    {50,1,1},
    {51,1,0},
    {54,1,0},
    {57,1,0},
    {60,1,0}
};

void dpCal(int t){
    for(int i =0;i<42;i++){
        int cnum = dPoint[i].num;
        int ccnt = dPoint[i].cnt;
        int cball = dPoint[i].ball;
        
        if(t - cnum < 0){ // 불가능한 경우
            continue;
        }
        else if(t - cnum == 0){ // 한 다크로 가능한 경우일걸?
            dp[t][0] = ccnt;
            dp[t][1] = cball;
            // t가 이미 정해짐
            break;
        }
        else if(t - cnum > 0){
            int ncnt = dp[t-cnum][0];
            int nball = dp[t-cnum][1];
            
            int sumcnt = ncnt + ccnt; //
            int sumball = nball + cball; 
            
            if(dp[t][0] == 0){ // 아직 한 번도 안들어옴
                dp[t][0] = sumcnt;
                dp[t][1] = sumball;
            }
            else{
                if(dp[t][0] > sumcnt){ // 다트 수가 작으면 바꿔줌
                    dp[t][0] = sumcnt;
                    dp[t][1] = sumball;
                }
                else if(dp[t][0] == sumcnt){ // 같으면
                    if(dp[t][1] < sumball ) { //볼이 더 크면 바꿔줌 
                        dp[t][0] = sumcnt;
                        dp[t][1] = sumball;
                    }
                }         
            }
        }
    }
}

vector<int> solution(int target) {
    vector<int> answer;
    
    for(int i =1;i<= target; i++){
        dpCal(i);
        //cout << "i-" << i << "_cnt-" << dp[i][0] << "_ball-" << dp[i][1] << "\n";
    }

    answer.push_back(dp[target][0]);
    answer.push_back(dp[target][1]);
    
    return answer;
}
```

Gemini DP 풀이

```cpp
#include <vector>
#include <algorithm>

using namespace std;

const int INF = 987654321;

vector<int> solution(int target) {
    // 1. 다트 한 발로 얻을 수 있는 최선의 점수 목록 만들기
    // score_info[점수] = 해당 점수를 얻었을 때 (싱글 or 불)이면 1, 아니면 0
    vector<int> score_info(61, -1);
    
    for (int i = 1; i <= 20; i++) {
        score_info[i] = 1;          // 싱글 (우선순위 높음)
        if (score_info[i * 2] == -1) score_info[i * 2] = 0; // 더블
        if (score_info[i * 3] == -1) score_info[i * 3] = 0; // 트리플
    }
    score_info[50] = 1;             // 불

    // 실제 계산에 사용할 점수 리스트 추출
    vector<pair<int, int>> valid_scores;
    for (int i = 1; i <= 60; i++) {
        if (score_info[i] != -1) valid_scores.push_back({i, score_info[i]});
    }

    // 2. DP 테이블 초기화 {다트 수, 싱글+불 수}
    vector<pair<int, int>> dp(target + 1, {INF, 0});
    dp[0] = {0, 0};

    // 3. Bottom-Up DP 진행
    for (int i = 1; i <= target; i++) {
        for (auto& s : valid_scores) {
            int prev = i - s.first;
            if (prev < 0) continue;

            int cur_darts = dp[prev].first + 1;
            int cur_sb = dp[prev].second + s.second;

            // 조건 1: 다트 수가 더 적은 경우
            if (cur_darts < dp[i].first) {
                dp[i] = {cur_darts, cur_sb};
            }
            // 조건 2: 다트 수는 같은데 싱글+불 개수가 더 많은 경우
            else if (cur_darts == dp[i].first) {
                if (cur_sb > dp[i].second) {
                    dp[i].second = cur_sb;
                }
            }
        }
    }

    return {dp[target].first, dp[target].second};
}
```

Gemini BFS 풀이

```cpp
#include <vector>
#include <queue>

using namespace std;

struct State {
    int darts;
    int sb; // single or bull
};

vector<int> solution(int target) {
    // dist[점수] = {최소 다트 수, 최대 싱글+불 수}
    // 초기값은 다트 수를 아주 크게(INF) 설정
    vector<State> dist(target + 1, {100001, 0});
    
    // 점수 리스트 미리 만들기 {점수, 싱글/불 여부(1 or 0)}
    vector<pair<int, int>> scores;
    for (int i = 1; i <= 20; i++) {
        scores.push_back({i, 1});      // 싱글
        scores.push_back({i * 2, 0});  // 더블
        scores.push_back({i * 3, 0});  // 트리플
    }
    scores.push_back({50, 1});         // 불

    queue<int> q;
    q.push(0);
    dist[0] = {0, 0};

    while (!q.empty()) {
        int cur = q.front();
        q.pop();

        for (auto& s : scores) {
            int next = cur + s.first;
            int next_darts = dist[cur].darts + 1;
            int next_sb = dist[cur].sb + s.second;

            if (next > target) continue;

            // 1. 다트 수가 더 적거나
            // 2. 다트 수는 같은데 싱글+불 개수가 더 많으면 갱신
            if (next_darts < dist[next].darts) {
                dist[next] = {next_darts, next_sb};
                q.push(next);
            } else if (next_darts == dist[next].darts) {
                if (next_sb > dist[next].sb) {
                    dist[next].sb = next_sb;
                    // 다트 수가 같을 때는 큐에 다시 넣을 필요 없음 (이미 최단거리)
                }
            }
        }
    }

    return {dist[target].darts, dist[target].sb};
}

```