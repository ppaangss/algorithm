
먼저, 완전탐색의 경우를 따져보면 

dp로 가능한지 확인
이전에 썻던 값을 재활용할 수 있는 가?
예를들어 N을 3개 쓴 것을 구하려고 할 때 경우의 수는
N을 1개 + N을 2개 쓴 것과
N을 2개 + N을 1개 쓴 것의 조합으로 전부 구할 수 있다.

dp에 무엇을 저장할 것인가? 를 고민하는게 어려웠다.
또, dp를 어떤 방식으로 채워나가야 할 지 선택하는게 어려웠다.

일단은 dp 인덱스 i 를 N이 이용된 횟수로 설정했다.
그런 경우 2차원 dp를 이용해야 한다. N이 i개 이용된 식은 여러개이기 때문이다.
대신 중복을 제거할 수 있도록 set을 이용하면 된다.


# 풀이

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
//
#include <queue>
#include <stack>
#include <unordered_set>

using namespace std;

int solution(int N, int number) {
    int answer = -1;

    if (N == number) return 1;

    vector<unordered_set<int>> dp(9);

    dp[1].insert(N);

    for (int i = 2; i <= 8; i++) {

        // NN, NNN 으로 되어있는 숫자
        int num = N;
        for (int j = 2; j <= i; j++) {
            num = num * 10 + N;
        }
        dp[i].insert(num);

        // 사칙연산으로 만들어진 것들 삽입 
        for (int j = 1; j < i; j++) {
            for (int x : dp[j]) {
                for (int y : dp[i - j]) {
                    dp[i].insert(x + y);
                    if(x - y > 0){
                        dp[i].insert(x - y);
                    }
                    dp[i].insert(x * y);
                    if (x / y > 0) {
                        dp[i].insert(x / y);    
                    }
                }
            }
        }

        if (dp[i].find(number) != dp[i].end()) { //DP set에 number에 해당하는 값이 있으면 k+1을 반환
            return i;
        }
    }

    return answer;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    vector<int> v1 = { 10,10,10,10,10,10,10,10,10,10 };
    //vector<int> v2 = { 1, 30, 5 };
     
    //string s1 = "(())()";
    //string s2 = ")()(";

    int i1 = 100;
    int i2 = 100;
    
    //for (auto x : solution(v1,v2)) {
    //    cout << x << "\n";
    //}

   
    cout << solution(5,12);
}
```