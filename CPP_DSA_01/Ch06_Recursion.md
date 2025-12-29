# S01. 재귀 알고리즘


### 1. 재귀(recursion, 순환) 알고리즘이란?
- 알고리즘이나 함수가 수행 도중에 자기 자신을 다시 호출하여 문제를 해결하는 기법
- 많은 종류의 문제가 재귀적으로 해결 가능
    - 예. 피보나치 수열, 이진 탐색, 병합 정렬, 퀵 정렬 등
    - 마트료시카 인형


### 2. 재귀 함수(recursion function)
- 자기 자신을 다시 호출하는 함수. 순환 함수
- 자기 자신을 완전히 그대로 호출하지 않고, 함수의 인자를 특정한 방식으로 변경하여 호출


### 3. 재귀 함수의 구성
- 기저 조건(base case): 재귀를 종료하기 위한 조건이 있어야함. **종료 조건**
- 재귀 호출(recursive case): 자기 자신을 다시 호출하는 부분 / 재귀를 반복하다보면 반드시 기저 조건으로 수렴해야함


### 4. 자연수의 합 구하기
- 양의 정수 N이 입력으로 주어질 경우, 1부터 N까지의 합을 반환하는 함수 sum(N)을 작성하시오
```cpp
#include <iostream>

using std::cout;
using std::endl;

int sum(int n)
{
    int s = 0;
    for (int i = 1; i <= n; i++)
    {
        s += i;
    }

    return s;
}

// sum(N) = N + sum(N - 1)
int sum_recursive(int n)
{
    if (n <= 1)
        return 1;
    else
        return n + sum_recursive(n - 1);
}

int main()
{
    cout << sum(10) << endl;
    cout << sum_recursive(10) << endl;

    return 0;
}
```


### 5. 재귀 함수의 특징
- 모든 재귀 함수는 반복문을 사용하는 함수로 변환 가능
- 재귀 함수의 시간 복잡도
    - O(N)
    - 함수의 실행 위치 / 인자값을 스택 메모리에 저장하거나 등의 함수 호출 오버헤드가 존재함
    - 함수 호출마다 스택 프레임이 생성되어 공간 복잡도가 선형적으로 증가함 O(N)

- 반복문 함수의 시간 복잡도
    - O(N)

- 재귀 함수가 반복문 함수보다 성능적인 부분에서 안 좋을 수 있음

- 장점: 코드를 간결하게 작성 가능
- 단점: 디버깅이 어려움 / 스택 오버플로우 주의 / 반복문 사용 코드보다 낮은 효율


### 6. 재귀 함수와 반복문 함수의 연산 시간 비교
- 1부터 n까지의 자연수의 합을 재귀 방법과 반복문 사용 방법으로 각각 계산하고, 그 실행 시간을 비교
- n값은 1부터 20000까지 변화시시켠서 입력으로 전달
```cpp
#include <iostream>
#include <chrono>

using std::cout;
using std::endl;

int sum(int n)
{
    int s = 0;
    for (int i = 1; i <= n; i++)
    {
        s += i;
    }

    return s;
}

// sum(N) = N + sum(N - 1)
int sum_recursive(int n)
{
    if (n == 1)
        return 1;
    else
        return n + sum_recursive(n - 1);
}

int main()
{
    auto start1 = std::chrono::steady_clock::now();
    cout << sum(20000) << endl;
    auto end1 = std::chrono::steady_clock::now();
    auto msec1 = std::chrono::duration<double>(end1 - start1).count() * 1000;
    cout << msec1 << endl;

    auto start2 = std::chrono::steady_clock::now();
    cout << sum_recursive(20000) << endl;
    auto end2 = std::chrono::steady_clock::now();
    auto msec2 = std::chrono::duration<double>(end2 - start2).count() * 1000;
    cout << msec2 << endl;

    return 0;
}
```

- gcc는 최적화 옵션으로 컴파일하면 재귀함수 호출을 반복문 형태로 알아서 최적화해서 성능상의 차이가 없어보일 수 있음
- 그렇다고 항상 최적화가 동작이 되는 것이 아님!! 항상 주의해야함!!
- Visaul C++은 최적화(Release Mode)로 실행해도 반복문 형태가 재귀 형태보다 성능이 좋음
- **함수 호출의 오버헤드가 영향을 줌**
- 그럼에도 상황에 따라서 코드가 직관적이고 간결해질 수 있어서 많이 사용함


### 7. 추가 내용
- 꼬리 재귀 최적화 (Tail Call Optimization)
    - 정의: 함수의 마지막 연산이 자기 자신을 호출하는 것으로 끝날 때, 컴파일러가 이를 반복문으로 변환하여 스택을 새로 쌓지 않게 함
    - 조건: return n + sum_recursive(n - 1)은 호출 후 더하기 연산이 있기 때문에 TCO가 불가능
    - TCO가 가능한 형태
        ```cpp
        int sum_tail(int n, int total = 0)
        {
            if (n <= 0) return total;
            return sum_tail(n - 1, total + n);
        }
        ``` 

- 재귀 vs 스택 자료구조
    - 재귀는 일정 수준 이상 깊어지면 스택 오버플로우가 발생할 수 있음
    - 차라리 스택 자료구조를 사용하는 방법을 고려해볼 수 있음



---
# S02. 재귀 알고리즘 주요 예제 01


### 1. 팩토리얼(factorial, 계승)
- 20보다 같거나 작은 자연수 N이 입력으로 주어질 경우, 1 * 2 * 3 .. * (N - 1) * N을 계산하는 팩토리얼 함수를 작성하시오
- 20보다 작은 자연수일 이유: int 자료형으로는 안됨 (-21억 ~ 21억 사이)
```cpp
#include <iostream>

using std::cout;
using std::endl;

long long factorial(int n)
{
    if (n <= 0)
        return 1;

    return n * factorial(n - 1);
}

int main()
{
    cout << factorial(5) << endl;
    cout << factorial(10) << endl;
    cout << factorial(20) << endl; // 243290200817664000

    return 0;
}
```


### 2. 피보나치 수(Fibonacci numbers)
- 첫째 및 둘째 항이 1이며 그 뒤의 모든 항은 바로 앞 두 항의 합인 수열
    - 1 1 2 3 5 8 13 21 ...
- 편의상 0번째 항을 0으로 설정하기도 함
    - f(0) = 0
    - f(1) = 1
    - f(n) = f(n - 2) + f(n - 1) | (n > 1)
```cpp
#include <iostream>

using std::cout;
using std::endl;

long long fibonacci(int n)
{
    if (n <= 1)
        return n;

    else
        return fibonacci(n - 2) + fibonacci(n - 1);
}

int main()
{
    for (int i = 1; i <= 10; i++)
    {
        cout << fibonacci(i) << " ";
    }
    cout << endl;
    // 직접 계산 5
    // - 3 4
    // - 1 2 / 2 3
    // - 1 / 0 1 / 0 1 / 1 2
    // - 1 / 0 1 / 0 1 / 1 / 0 1

    return 0;
}
```


### 3. 재귀에 의한 피보나치 수 계산 문제점
- 중복되는 부분 문제(overlapping subproblem)로 인해 계산 효율이 떨어짐
- fibonacci(50): 1분 정도 걸림
    - F(5) - F(4) / F(3) - F(3), F(2) / F(2), F(1)
    - 위와 같이 F(N)이 중복되는 현상이 발생함
- 캐시를 사용하여 문제를 해결할 수 있음
    - 이전에 계산된 F(N)을 계산할 수 있음 => **동적 계획법**


### 4. 문자열 뒤집기
- 문자열의 각 문자 순서를 역순으로 변경
- 1. 두 번째 문자부터 시작하는 부분 문자열을 뒤집어 변환
- 2. 1에서 반환된 문자열 뒤에 첫 번째 문자를 추가
```cpp
#include <iostream>
#include <string>

using std::cout;
using std::endl;
using std::string;

string reverse(const string& str)
{
    if (str.length() == 0)
        return "";
    else
        return reverse(str.substr(1)) + str[0];
}

int main()
{
    cout << reverse("Hello") << endl;

    return 0;
}
```


### 5. 최대 공약수 (greatest common divisor)
- gcd(a, b): 두 개의 자연수 a와 b가 있을 때, a와 b 모두 약수 중에서 가장 큰 정수
- 유클리드 알고리즘을 이용하여 재귀적으로 구할 수 있음
    - a | if b = 0
    - gcd(b, a % b) | otherwise

- 유클리드 알고리즘 예:
    - gcd(24, 18)
    - gcd(18, 6)
    - gcd(6, 0)
    - "6"

```cpp
#include <iostream>

using std::cout;
using std::endl;

int gcd(int a, int b)
{
    if (b == 0)
        return a;

    else   
        return gcd(b, a % b);
}

int main()
{
    cout << gcd(24, 18) << endl;

    return 0;
}
```


### 6. 최소 공배수(lowest common multiple)
- lcm(a, b): 두 정수 a와 b가 있을 때, a와 b로 모두 나누어 떨어지는 가장 작은 정수
- 두 정수의 곱은 두 정수의 최대 공약수와 최소 공배수의 곱과 같다는 성질을 이용하여 구할 수 있음
    - a * b = gcd(a, b) * lcm(a, b)
    - lcm(a, b) = (a * b) / gcd(a, b)

```cpp
#include <iostream>

using std::cout;
using std::endl;

int gcd(int a, int b)
{
    if (b == 0)
        return a;
    else
        return gcd(b, a % b);
}

int lcm(int a, int b)
{
    return (a * b) / gcd(a, b);
}

int main()
{
    cout << lcm(3, 6) << endl;

    return 0;
}
```


### 7. C++ 최대 공약수 & 최소 공배수 계산 함수
```cpp
template <class M, class N>
constexpr std::common_type_t<M, N> gcd(M n, N n);

template <class M, class N>
constexpr std::commom_type_t<M, N> lcm(M m, N n);
```

- C++ 17 지원
- <numeric>에 정의되어 있음

```cpp
#include <iostream>
#include <numeric>

int main()
{   
    std::cout << std::gcd(10, 15) << std::endl;
    std::cout << std::lcm(10, 15) << std::endl;

    return 0;
}
```


### 8. 추가 내용
- 최소 공배수의 오버플로우 트랩
    - (a * b) / gcd(a, b)를 할 경우 a * b에서 오버플로우가 발생할 수도 있음
    - (a / gcd(a, b)) * b 순서로 하는 것이 좋음


---
# S03. 재귀 알고리즘 주요 예제 02


### 1. 
