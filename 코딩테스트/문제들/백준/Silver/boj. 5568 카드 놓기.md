https://www.acmicpc.net/problem/5568
# 회고

순열에 관한 문제 그리고 조합을 사용하여 중복을 제외하면 간단한 문제였다.

순열을 어떻게 구현할 것인가에 대한 방식은 2가지가 있다.

재귀를 쓸거냐 아니면 next_permutation 함수를 쓸거냐는 다음과 같이 요약할 수 있다.

- `next_permutation`을 쓰는 경우:
    
    - $n$의 크기가 작을 때 ($n \le 10$).
        
    - 전체 원소를 일렬로 세우는 경우 ($n=k$).
        
    - 코드를 짧고 직관적으로 짜고 싶을 때.
        
- 재귀(DFS)를 쓰는 경우:
    
    - $n$은 큰데 $k$는 작을 때 (예: 100개 중 3개 뽑기).
        
    - 탐색 도중 특정 조건에서 가지치기(Pruning)가 가능할 때.
        
    - 대부분의 코딩 테스트 실전 환경.


가지치기라는게 모호했는데 그냥 내가 평소 했던 visited 배열을 쓴게 가지치기중 하나라는 것을 알았다. 또 다른 가지치기는 예를 들면 합이 10이 되는 3개의 숫자를 찾는데 이미 2개를 뽑았는데 합이 10이 넘었다면 3번째 숫자를 볼 필요도 없이 다른 경우를 확인하면 된다. 이것도 하나의 가지치기 예제라고 볼 수 있다.

set에 대한 함수에 대해 복습해야겠다. insert, find, end ...

# 풀이

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <set>
#include <algorithm>

using namespace std;

int main() {
    int n, k;
    cin >> n >> k;

    vector<string> cards(n);
    for (int i = 0; i < n; i++) cin >> cards[i];

    // next_permutation을 쓰기 위해 먼저 정렬
    sort(cards.begin(), cards.end());

    set<string> resultSet;

    // n개 중 k개를 뽑는 모든 경우의 수 탐색
    do {
        string temp = "";
        for (int i = 0; i < k; i++) {
            temp += cards[i];
        }
        resultSet.insert(temp); // set이 중복을 알아서 걸러줌
    } while (next_permutation(cards.begin(), cards.end()));

    // 위 방식은 nPk를 모두 돌기 때문에, 
    // 정확히는 조합(Combination) 후 순열을 돌리는 게 효율적이지만 
    // n이 최대 10이라 이 방법으로도 충분히 통과합니다.

    cout << resultSet.size() << endl;

    return 0;
}
```




