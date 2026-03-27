
# 유클리드 호제법

a를 b로 나눈 나머지를 r이라고 할 때, $GCD(a, b) = GCD(b, r)$ 성립 한다.

이 과정을 나머지가 0이 될 때 까지 반복하면 그때의 나누는 수가 바로 최대공약수이다.

이때,` a x b = 최대공약수 x 최소공배수` 라는 식이 성립한다.

따라서` 최소공배수 = a x b / 최대공약수` 를 하면 쉽게 구할 수 있다.


```cpp
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}

int lcm(int a, int b) {
    return (a / gcd(a, b)) * b;
}
```

# 내장 함수

C++17 표준부터는 직접 구현할 필요 없이 내장 함수를 쓸 수 있습니다. (헤더: `<numeric>`)

```cpp
#include <numeric>

int a = 12, b = 18;
int g = gcd(a, b); // 최대공약수: 6
int l = lcm(a, b); // 최소공배수: 36
```

만약 C++17 미만 버전이라면 직접 유클리드 호제법을 구현해야 한다.
