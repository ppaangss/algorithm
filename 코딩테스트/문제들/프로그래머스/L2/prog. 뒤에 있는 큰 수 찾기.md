https://school.programmers.co.kr/learn/courses/30/lessons/154539
# 회고

**1회차**

일단은 스택 공부를 조금 해야할 것 같다. 어떤 상황에서 스택을 쓸 수 있는지에 대해 알아야 할듯.

백준에 있는 boj. 17298 오큰수 랑 문제가 똑같은 거 같다. 그래서 복습겸 풀어서 맞았다.

# 해결과정

일단 문제를 봤을 때 뒤에 있는 큰 수를 찾아야 하므로 역순으로 접근헀다.

하지만 이것도 문제가 있었다. 이 문제의 함정은 뒤에 있는 숫자중 가장 큰 숫자를 찾는 것이 아니다. 뒤에 있는 숫자중 큰 숫자중에서 가장 가까운 것을 찾는 것이다.

그래서 리스트로 풀게 되면 결국 최악의 경우 시간복잡도가 $O(N^2)$  이되어 배열 크기가 1,000,000 이므로 시간초과가 발생한다.

이 문제는 결국 자료구조 하나를 써야했고, 답은 스택이었다. (우선순위 큐도 가능함)

근데 문제를 풀 때 전혀 스택이라는 것을 판단하지 못했다. 그래서 나는 스택이라는 문제를 어떻게 해야 판단할 수 있었을까?를 고민했다.

## 스택이라는 것을 어떻게 판단해야할까? (예시: 9,1,5,3,6,2)

1. 시간복잡도 판단

결국 배열의 길이가 1,000,000 이므로 무조건 $O(N)$ 이나 $O(N log N)$ 으로 풀어야 한다.

2. 아직 해결되지 않는 데이터 보관

앞에서부터 차례대로 해결을 해봤을 떄, 9를 고민해야하는데 9는 결국 뒤에 나올 숫자들에 의해 결정된다. 즉, 조건이 맞을 때 까지 결정을 미뤄야 하는 데이터를 보관할 때는 스택이나 큐를 고민하게 된다.

3. 최근 데이터와의 비교

숫자를 앞에서부터 확인할 때 `방금 내 바로 앞에 있던 숫자가 나보다 작냐?` 를 확인해야한다.

여기서 약간 고민할 수 있다. `왜 큐는 안되는가?` 그리고 `왜 내 바로 앞에 있던 숫자를 확인해야하는가?` 가 걸렸다.

결국 처리의 문제였다. 만약에 9를 넣고 다음 숫자로 넘겨질 때 9가 판단될 경우는 9보다 숫자가 큰 경우이다. 즉, 9가 판단이 안되는 경우는 9보다 작은 경우이다.

그렇다면 판단이 안되 다음숫자 (1) 또한 데이터를 보관하면 그 숫자는 반드시 9보다 작으며, 바로 다음 숫자를 판단할 때 (5), 판단 순서를 9가 아닌 1부터 해야한다. 즉, 가장 나중에 들어온 숫자부터 차례대로 검사를 해야하기 때문에 스택을 써야한다.

그렇지만 이 문제는 우선순위 큐로도 풀 수 있었다.

## 우선순위 큐 풀이법

차근차근 해가면 된다. 이것도 결국 데이터를 저장하는 방법에 대한 시간복잡도를 줄이면 된다.

일단 값과 인덱스 모두 필요하므로 우선순위 큐에 넣는다. 

우선순위 큐를 선택하는 이유는 간단하다. `합격 커트라인` 을 만들기 위함이다.

앞에서부터 계속해서 처리한다. 이때 만족하는 것이 없으면 데이터 보관을 해야한다.

해당 숫자를 방문하는 것은 그 숫자를 고민하는 것이 아니라, 이전 숫자들을 판단하는 도구로 사용하면 편하다. 
9 다음 1을 비교대상으로 선택한다. 그러면 9는 1보다 크므로 안된다. 그다음 1을 우선순위 큐에 넣는다 이 과정을 반복한다. 5를 비교대상으로 선택한다. 1은 5보다 작으므로 1에 해당하는 값은 5이고 1을 우선순위 큐에서 제거한다. 9는 5보다 크므로 안된다. 5를 우선순위 큐에 넣는다. 이런식으로 반복한다.

최종 시간복잡도는 최소 힙 `O(logN)` 이고 배열 순회를 한 번 하므로 `O(N)` 으로 총 `O(NlogN)` 이다.
스택보다는 시간복잡도가 최소 힙으로 인해 크지만, 배열의 크기가 1,000,000 이므로 충분히 가능한 답안이다.

# 코드

**스택**

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
#include <queue>
#include <stack>
using namespace std;

vector<int> solution(vector<int> numbers) {
    int n = numbers.size();
    // 정답 배열을 -1로 초기화 (뒷 큰수가 없는 경우 대비)
    vector<int> answer(n, -1);
    stack<int> s;

    for (int i = 0; i < n; i++) {
        // 스택이 비어있지 않고, 현재 숫자가 스택 top 위치의 숫자보다 큰 경우
        while (!s.empty() && numbers[s.top()] < numbers[i]) {
            // 스택 top에 기록된 인덱스의 뒷 큰수는 현재 숫자임
            answer[s.top()] = numbers[i];
            s.pop();
        }
        // 현재 인덱스를 스택에 추가
        s.push(i);
    }

    return answer;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(NULL);

    vector<int> v = { 9,1,5,3,6,2 };
    vector<int> res = solution(v);

    for (auto x : res) {
        cout << x << "\n";
    }
    
	return 0;
}

```


**우선순위 큐**

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
#include <queue>

using namespace std;

vector<int> solution(vector<int> numbers) {
    int n = numbers.size();
    vector<int> answer(n, -1);

    // {값, 인덱스} 형태의 최소 힙 (값이 작은 순서대로 정렬)
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

    for (int i = 0; i < n; i++) {
        // 큐가 비어있지 않고, 현재 숫자가 큐에서 가장 작은 값보다 큰 경우
        while (!pq.empty() && pq.top().first < numbers[i]) {
            answer[pq.top().second] = numbers[i];
            pq.pop();
        }
        // 현재 숫자와 인덱스를 큐에 넣음
        pq.push({ numbers[i], i });
    }

    return answer;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(NULL);

    vector<int> v = { 9,1,5,3,6,2 };
    vector<int> res = solution(v);

    for (auto x : res) {
        cout << x << "\n";
    }
    
	return 0;
}
```

오큰수

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <stack>
#include <set>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; 

	cin >> N;

	stack<int> st;
	vector<int> v;
	vector<int> res(N,-1);

	for (int i = 0; i < N; i++) {
		int num1; cin >> num1;
		v.push_back(num1);
	}
	
	for (int i = 0; i < N; i++) {
		while (1) { // 스택에 있는 것들 비교

			if (st.empty()) {
				break;
			}

			int index1 = st.top();
			//cout << v[index1] << "\n";

			if (v[index1] >= v[i]) {
				break;
			}

			res[index1] = v[i];
			st.pop();
		}

		st.push(i);
	}	

	for (int i = 0; i < N; i++) {
		cout << res[i] << " ";
	}

	return 0;
}


```






