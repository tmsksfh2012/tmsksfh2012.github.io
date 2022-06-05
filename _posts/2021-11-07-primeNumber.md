---
title: "[BaekJoon] 백준 11653: 소인수분해"
categories: 
  - BaekJoon
last_modified_at: 2021-05-09T23:45:00+09:00
toc: true
---

**문제:**

정수 N이 주어졌을 때, 소인수분해하는 프로그램을 작성하시오.<br/>

**입력:**

첫째 줄에 정수 N (1 ≤ N ≤ 10,000,000)이 주어진다.<br/>

**출력:**

N의 소인수분해 결과를 한 줄에 하나씩 오름차순으로 출력한다. N이 1인 경우 아무것도 출력하지 않는다.<br/>

**---**

**풀이:**

소인수분해 문제를 해결하기 위해서는 우선 소수를 판정하는 방법에 대해 알아야한다.<br/>

어떤 수 x가 소수일 조건은 약수로 1과 자기 자신만을 가져야 한다. 따라서 x를 [2, x−1] 범위 내의 수로 나누었을때, 나누어 떨어지는 수가 없으면 그 수는 소수이다. 이를 코드로 표현하면 다음과 같다.<br/>

```cpp
bool IsPrime(int x) {
    for (int i = 2; i < x; ++i) {
        if (x % i == 0)
            return false;
    }
    return true;
}
```

위 알고리즘의 시간복잡도는 O(x)이다. 이 시간복잡도를 줄일 수가 있는데 이론은 다음과 같다. √x 보다 큰 수 d가 x의 약수라고 하자. (√x < d)<br/>
그럼, x = d∗m이 성립하는 m이 존재한다. d와 m의 곱은 x보다 작아야 하므로 (m < √x < d)라는 부등식을 세울 수 있다.<br/>
즉, √x보다 작은 수 y가 x의 약수이면 y와 곱했을때, x가 나오는 수가 반드시 존재하는 것이다.
따라서, 우리는 다음과 같이 소수 판정의 범위를 줄일 수가 있다.<br/>

```cpp
bool IsPrime(int x) {
    for (int i = 2; i*i <= x; ++i) {
        if (x % i == 0)
            return false;
    }
    return true;
}
```

이를 통해 우린 시간복잡도를 O(√x)로 줄일 수 있다.<br/>

소수를 판정하는 대표적인 방법으로는 에라토스테네스의 체가 있다. 에라토스테네스의 체는 1과 자기자신 사이에 약수의 배수를 모두 지우면 남는 수가 약수라는 이론이다.<br/>

* [에라토스테네스의 체](https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4)<br/>

이제 이를 활용해 문제를 풀어보도록 하자.<br/>

예를 들어, 72가 있으면 2∗2∗2∗3∗3으로 표현된다. 이를 살펴보면 2로 가능한 나누어 표현하고 그 다음으로 3으로 가능한 나누어 표현하였다.<br/>

따라서 우리는 어떤 수 x에 대해 작은 수부터 가능할 때까지 나누어주며 그 수를 출력해주면 된다. 범위는 앞선 알고리즘에서 살펴본 바로 [2, √x−1] 까지로 하면 된다. 결과적으로 2의 배수, 3의 배수, 5의 배수...가 차례대로 지워지면서 소인수들이 갯수만큼 출력이 된다.<br/>

**코드**

```cpp

#include <iostream>
using namespace std;

void __init() {
  cin.tie(0);
  ios::sync_with_stdio(0);
}

void Factorization(int num) {
  for(int i = 2; i*i <= num; i++) {
    while(num % i == 0) {
      cout << i << "\n";
      num /= i;
    }
  }
  if(num != 1) cout << num;
}

int main() {
  int num;

  __init();

  cin >> num;
  Factorization(num);
}
```