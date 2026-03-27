https://school.programmers.co.kr/learn/courses/30/lessons/169198?language=cpp
# 회고

입사각 반사각에 대한 거리를 구하는 문제는 대칭시켜 풀 수 있다는 것을 알아야 풀 수 있는 문제였다.
그러면 이런 수학적 지식을 공부를 약간은 해가야 하는게 나을듯? 간단한 수준같은거

그 외에는 무난한 구현 문제였으며, 엣지케이스를 잘 항상 생각하는 버릇을 들여야 할 것 같다. 문제 제출 누르기 전에 충분한 생각을 하고 엣지케이스 고민 많이하고 제출을 내자!!


# 해결과정

입사각, 반사각과 관련된 점 사이의 거리는 이제 한 점을 대칭시켜 거리를 구하면 된다.

이때, 두 점이 수직이나 수평으로 놓여 있다면 계산할 가짓수가 줄어든다.

나중에 알았지만 시작점과 끝점이 뭐냐도 굉장히 중요한 문제였다. 이게 만약 위 아래로 겹쳐있을 때, 시작점이 위인 공이라고 가정하면 그 형태의 반사는 위로도 갈 수 있었던 것을 나는 놓쳤다. 그래서 그런 엣지 케이스를 잘 생각해야 한다.

# 풀이

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
#include <queue>
#include <stack>

using namespace std;

vector<int> solution(int m, int n, int startX, int startY, vector<vector<int>> balls) {

    vector<int> answer;

    for (int i = 0; i < balls.size(); i++) {

        int x = balls[i][0];
        int y = balls[i][1];

        vector<vector<int>> sss;
        vector<int> disRes;

        if (x == startX) {
            sss.push_back({ -x,y });
            sss.push_back({ 2 * m - x,y });
            if (y > startY) { // 시작 공이 더 아래에 있다.
                sss.push_back({ x,-y });
            }
            else {
                sss.push_back({ x, 2 * n - y });
            }
        }

        else if (y == startY) {
            sss.push_back({ x,-y });
            sss.push_back({ x, 2 * n - y });
            if (x > startX) {
                sss.push_back({ -x,y });
            }
            else {
                sss.push_back({ 2 * m - x,y });
            }
        }
        else {
            sss.push_back({ -x,y });
            sss.push_back({ x,-y });
            sss.push_back({ 2 * m - x,y });
            sss.push_back({ x, 2 * n - y });
        }

        for (int i = 0; i < sss.size(); i++) {
            int lsX = sss[i][0];
            int lsY = sss[i][1];

            int dis = (lsX - startX) * (lsX - startX) + (lsY - startY) * (lsY - startY);
            disRes.push_back(dis);
        }

        int num = *min_element(disRes.begin(), disRes.end());

        answer.push_back(num);
    }

    return answer;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(NULL);

    vector<vector<int>> v = { {4, 6} };
    
    vector<int> v1 = solution(10, 10, 2, 8, v);

    for (auto x : v1) {
        cout << x << "\n";
    }

    
	return 0;
}

```

