
# set 함수 요약

|**함수**|**기능**|**예시 (set<int> s)**|
|---|---|---|
|`s.insert(x)`|원소 `x` 삽입 (중복이면 무시)|`s.insert(10);`|
|`s.erase(x)`|원소 `x`를 찾아 삭제|`s.erase(10);`|
|`s.erase(it)`|반복자(`it`)가 가리키는 원소 삭제|`s.erase(s.begin());`|
|`s.find(x)`|`x`를 찾아 해당 위치의 반복자 반환|`if(s.find(10) != s.end())`|
|`s.count(x)`|`x`가 존재하는지 확인 (0 또는 1 반환)|`s.count(10);`|
|`s.size()`|현재 저장된 원소의 개수|`s.size();`|
|`s.clear()`|모든 원소 삭제|`s.clear();`|
|`s.lower_bound(x)`|`x`보다 **크거나 같은** 첫 번째 원소의 반복자|`auto it = s.lower_bound(5);`|
|`s.upper_bound(x)`|`x`보다 **큰** 첫 번째 원소의 반복자|`auto it = s.upper_bound(5);`|

# map 함수 요약

|**함수**|**기능**|**예시 (map<string, int> m)**|
|---|---|---|
|`m[key] = val`|키에 값을 할당 (추가 또는 수정)|`m["apple"] = 100;`|
|`m.insert({k, v})`|키와 값의 쌍(`pair`)을 삽입|`m.insert({"banana", 200});`|
|`m.erase(key)`|해당 키와 값을 삭제|`m.erase("apple");`|
|`m.find(key)`|키를 찾아 반복자 반환|`auto it = m.find("apple");`|
|`m.count(key)`|키의 존재 여부 확인 (0 또는 1)|`if(m.count("apple"))`|
|`m.size()`|현재 저장된 쌍(Pair)의 개수|`m.size();`|
|`m.clear()`|모든 데이터 삭제|`m.clear();`|
|`it->first`|반복자가 가리키는 **키(Key)** 접근|`cout << it->first;`|
|`it->second`|반복자가 가리키는 **값(Value)** 접근|`cout << it->second;`|

# 주의사항

- 해시셋의 경우 정렬이 보장되어 있지 않아 `lower_bound`와 `upper_bound`는 사용이 불가능하다.
- 맵에 해당 키가 있는지를 확인할 때는 인덱스 접근을 사용하지 말고 find나 count를 쓰자.
	- `if (m["orange"] == 0)` 이라고 쓰는 순간, "orange"라는 키가 없었더라도 맵에 자동으로 추가된다.
- hash set과 hash map의 경우 insert와 erase가 항상 $O(1)$ 인 것은 아니다. 평균적으로 $O(1)$ 인 것이므로 $O(1)$ 로 했다가 시간초과가 발생할 수 있다는 점을 유의하자.
	- 매번 충돌이 나는 경우 $O(N)$ 이 걸린다.


