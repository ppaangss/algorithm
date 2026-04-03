# 함수 만들 때 반환값이 void가 아니면 return이 있어야 한다.

`signal: segmentation fault (core dumped)` 

프로그래머스 에서 문제를 풀다가 갑자기 이런 에러가 생겼다. 함수 때문인건 알았는데 도대체 뭔 오류인지 몰랐다.

결국 문제는 반환형이 int였는데 return 이 없었던 실수를 했던 것이다.

이런 실수 하지 않도록 주의!!