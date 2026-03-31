
# deque는 인덱스 접근이 가능하다.

일반 `stack` 이나 `queue` 은 인덱스 접근이 불가능하다. 또한 `top()` 이나 `front()` 를 제외하면 접근조차 불가능하다.

하지만 `deque` 는 가능하다. `dq[0]` 이런식으로 접근이 가능하다. 그래서 `stack` 이나 `queue`를 써야하는데 인덱스 접근이 있으면 편리한 경우 `deque` 를 사용하면 좋을 것 같다.

또한, `deque` 는 `stack` 이나 `queue` 기능을 모두 쓸 수 있다.

- `push_back(x)`, `push_front(x)`: 뒤/앞에 원소 추가
- `pop_back()`, `pop_front()`: 뒤/앞에서 원소 제거

# deque는 정렬이 가능하다.

`vector` 를 제외한 다른 STL 자료구조 들은 정렬이 불가능하다.

하지만 `deque` 정렬이 가능하다.

`sort(dq.begin(),dq.end());` 가 가능하다. 