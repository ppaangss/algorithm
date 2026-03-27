
## 개요

배열, 벡터 내에서 가장 큰 or 작은 값의 위치를 찾아준다.
값을 직접 반환하는 것이 아닌 그 값이 위치한 **주소**를 반환한다.
값을 얻고 싶으면 `*` 을 사용하여 값을 참조하면 된다.

## 설명

- 반환값: 범위 내에서 가장 큰 첫 번째 요소의 **Iterator**(포인터와 유사함)를 반환합니다. 만약 범위가 비어있다면 `last`(끝 주소)를 반환합니다.
- 시간복잡도: O(N)
- 비교 방식: 기본적으로는 `<` 연산자를 사용해 비교하지만, 사용자가 직접 비교 함수(Custom Comparator)를 넣을 수도 있습니다.

## 코드 예시

```c++
#include <algorithm>
#include <vector>

// 형식
// max_element(시작 주소, 끝 주소);
int max = *max_element(v.begin(), v.end());
int min = *min_element(v.begin(), v.end());
```



