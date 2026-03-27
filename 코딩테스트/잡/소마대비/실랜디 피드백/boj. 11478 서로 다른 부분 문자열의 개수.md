https://www.acmicpc.net/problem/11478
# 회고

일단 3중 반복문으로는 해결이 안된다. N <= 1000이면, $O(N^3)$ 이면 시간초과 난다.

substr을 써볼까 했는데 이거 다시 복습해야됨. 

결과적으로 substr을 쓰니까 풀리긴 했다. 하지만 정답이 아니었다. 왜냐하면 substr도 결국은 시간복잡도가 $O(len)$ 이다. 즉, 내가 구현한 방법과는 크게 다르지않다. 단지 왜 풀렸냐면 set 대신 hash set을 써서 풀렸던 것 같다.

정답은 이중 반복문으로 해결하는 것이다. 충분히 가능한 문제였다.

# 풀이

**substr을 사용한 풀이**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <set>
using namespace std;

int main() {

	string s1; cin >> s1;

	//vector<string> res;
	set<string> st;

	for (int i = 1; i <= s1.size(); i++) { // 부분 문자열 개수 1 2 3 4 5
		for (int j = 0; j <= s1.size() - i; j++) { // 시작 인덱스  

			string tmp = substr(s1, j, j + i);
			
			if (st.find(tmp) == st.end()) {
				st.insert(tmp);
				//res.push_back(tmp);
				//cout << tmp << "\n";
			}
		}
	}

	cout << st.size() << "\n";

	return 0;
}
```

**이중반복문**

```cpp
#include <iostream>
#include <string>
#include <set>

using namespace std;

int main() {
    // 입출력 속도 향상
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    string s;
    cin >> s;

    set<string> distinct_substrings;

    // 모든 부분 문자열 추출
    for (int i = 0; i < s.length(); i++) {
        string sub = "";
        for (int j = i; j < s.length(); j++) {
            sub += s[j]; // 한 글자씩 더해가며 부분 문자열 생성
            distinct_substrings.insert(sub);
        }
    }

    // 중복 제거된 부분 문자열의 개수 출력
    cout << distinct_substrings.size() << "\n";

    return 0;
}
```