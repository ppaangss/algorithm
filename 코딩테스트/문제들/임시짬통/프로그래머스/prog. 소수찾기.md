
https://school.programmers.co.kr/learn/courses/30/lessons/42839

**분류**
- 소수 - 단일 숫자 판별
	- [[소수]]
- set
	- 중복제거
- 순열 
	- [[순열]]


# 풀이

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <set>

using namespace std;

vector<char> num;

bool isPrime(int n) {
	if (n <= 1) return false; // 1보다 작은 수 제거
	if (n == 2) return true; // 2는 소수다.
	if (n % 2 == 0) return false; // 짝수 제거

	for (int i = 3; i * i <= n; i += 2) {
		if (n % i == 0) return false;
	}
	return true;
}

int solution(string numbers) {

	// set으로 중복 방지
	set<int> nums;
	// 문자열 정렬
	sort(numbers.begin(), numbers.end());

	do {
		for (int len = 1; len <= numbers.size(); len++) {
			int val = stoi(numbers.substr(0, len));
			nums.insert(val);
		}
	} while (next_permutation(numbers.begin(), numbers.end()));

	int answer = 0;
	for (int n : nums) {
		if (isPrime(n)) answer++;
	}
	return answer;
}

int main() {
	string s; cin >> s;
	cout << solution(s) << "\n";
}

```

