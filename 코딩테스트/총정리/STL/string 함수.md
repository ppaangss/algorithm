
# 함수 요약

| 함수                         | 기능                     | 예시 (string s = "Hello")           |
| -------------------------- | ---------------------- | --------------------------------- |
| `s.size()` / `length()`    | 문자열의 길이 반환             | `s.size()` → `5`                  |
| `s.substr(pos, len)`       | `pos`부터 `len`만큼 자르기    | `s.substr(0, 2)` → `"He"`         |
| `s.find(str)`              | `str`이 처음 나타나는 위치(인덱스) | `s.find("el")` → `1`              |
| `s.push_back(ch)`          | 맨 뒤에 문자 `ch` 추가        | `s.push_back('!')` → `"Hello!"`   |
| `s.pop_back()`             | 맨 뒤의 문자 하나 제거          | `s.pop_back()` → `"Hell"`         |
| `s.replace(pos, len, str)` | 특정 범위를 `str`로 교체       | `s.replace(0, 2, "A")` → `"Allo"` |
| `s.erase(pos, len)`        | 특정 범위의 문자 삭제           | `s.erase(1, 2)` → `"Hlo"`         |
| `s.insert(pos, str)`       | `pos` 위치에 `str` 삽입     | `s.insert(5, "!!")` → `"Hello!!"` |
| `s.append(str)` / `+=`     | 뒤에 문자열 이어 붙이기          | `s += " World"` → `"Hello World"` |
| `s.empty()`                | 비어있는지 확인 (bool)        | `s.empty()` → `false`             |
| `s.clear()`                | 모든 문자 제거 (빈 문자열로)      | `s.clear()` → `""`                |
| `s.front()` / `back()`     | 첫 번째 / 마지막 문자 참조       | `s.back()` → `'o'`                |

|**함수**|**기능**|**예시**|
|---|---|---|
|`to_string(n)`|숫자(`int`, `double` 등)를 문자열로|`to_string(123)` → `"123"`|
|`stoi(s)`|문자열을 `int`로 변환|`stoi("10")` → `10`|
|`stoll(s)`|문자열을 `long long`으로 변환|`stoll("1234567890")`|
|`stod(s)`|문자열을 `double`로 변환|`stod("3.14")` → `3.14`|
# 자주 쓰는 것들 예시

- substr
- back

# string에서 특정 문자열이 있는지 확인하기

### `string::find` 작동 원리

1. 시작점 설정: 기본적으로 문자열의 0번 인덱스(가장 왼쪽)부터 시작합니다.
2. 비교: 찾고자 하는 대상(문자열 혹은 문자)과 현재 위치의 문자를 비교합니다.
3. 이동: 일치하지 않으면 오른쪽으로 한 칸 이동하여 다시 비교합니다.
4. 결과 반환:
    - 찾았을 때: 일치하는 구간이 시작되는 첫 번째 인덱스 번호를 반환합니다.
    - 못 찾았을 때: `string::npos`라는 특수한 상수(매우 큰 정수값)를 반환합니다.



```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	string str = "AAABAAAAB";

	cout << "str-" << str << "\n";

	// 1. 맨 앞에꺼 

	size_t pos1 = str.find("B");
	cout << "B의 맨 첫 위치: " << pos1 << "\n"; // 출력: 8 (0부터 시작해서 8번째 인덱스)

	// 2. 존재하지 않는 문자열 찾기

	size_t pos2 = str.find("CCC");
	if (pos2 == string::npos) {
		cout << "찾을 수 없습니다." << "\n"; // 이 문구가 출력됨
	}

	// 3. 중복을 허용해서 몇개 있는지 찾기

	cout << "중복 허용 AA 찾기" << "\n";
	size_t pos3 = str.find("AA");
	while (pos3 != str.npos) {
		cout << "발견된 인덱스: " << pos3 << "\n";

		pos3 = str.find("AA", pos3 + 1);
	}

	// 4. 중복을 허용하지 않고 몇개 있는지 찾기

	cout << "중복 불가 AA 찾기" << "\n";
	string target = "XXXX";
	size_t pos4 = str.find("AA");
	while (pos4 != str.npos) {
		cout << "발견된 인덱스: " << pos4 << "\n";

		pos4 = str.find("AA", pos4 + target.length());
	}



	return 0;
}


/*

XXXX....XXXXXX.....XX


XXXXXXXXXXXX...

XX..XXXX.X
*/
```