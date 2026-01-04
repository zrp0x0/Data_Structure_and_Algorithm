# S01. 해싱과 해시 함수


### 1. 해싱(hashing)
- hash: 잘게 썰은 것. 긁어 모은 것. 아무렇게나 뒤섞다
- 해싱(hashing): 각각의 데이터를 고유한 숫자 값으로 표현하고, 이를 이용하여 특정 데이터의 존재 여부를 확인하거나 또는 원본 데이터를 추출하는 작업
- 응용 분야: 빠른 자료 탐색(O(1)), 변조 탐지 및 에러 검출(MD5, SHA)



### 2. 해싱이 필요한 경우의 예
- 요구사항
    - 정수를 저장하고 있는 컨테이너가 있고, 이 컨테이너에 특정 정수가 들어 있는지를 빠르게 판단하고 싶음

- 해결 방법
    - 적절한 크기의 bool 타입 배열을 하나 만들고, 이 배열에서 입력 정수에 해당하는 인덱스의 원소 값을 true로 설정
    ```cpp
    bool data[8] = {}; // 0으로 초기화(false)
    data[2] = true;
    data[4] = true;
    data[5] = true;
    data[7] = true;
    ```

- 문제점
    - 정수 범위가 너무 크다면? 데이터가 실수라면? 데이터가 숫자가 아니라면?
    - 해결: 어떠한 타입의 값이든 **원하는 범위의 정수**로 매핑하는 함수를 만들어 사용!
        - 매핑하는 함수 == **해시 함수**
            - hash(x) = value
            - x: key


### 3. 간단한 해싱 클래스
```cpp
class hash_set
{
private:
    int sz;
    std::vector<int> data;

public:
    hash_set(int n) : sz(n), data(sz, -1) {}

    int hash(int key) { return key % sz; }

    void insert(int value) { data[hash(value) = value]; }
    bool find(int value) { return data[hash(value)] == value; }
    void erase(int value) { data[hash(value)] = -1; }
}

int main()
{
    hash_set num_set(7);

    num_set.insert(10);
    num_set.insert(15);

    return 0;
}
```


### 4. 간단한 해싱 클래스 사용 예제
```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;

class hash_set
{
public:
    hash_set(int size) : sz(size), data(sz, -1) {}

    int hash(int key) 
    {
        return key % sz;
    }

    void insert(int value)
    {
        data[hash(value)] = value;
    }

    bool find(int value)
    {
        return data[hash(value)] == value;
    }

    void erase(int value)
    {
        data[hash(value)] = -1;
    }

    void print()
    {
        for (const auto& n : data)
        {
            cout << n << " ";
        }
        cout << endl;
    }

private:
    int sz; 
    std::vector<int> data;
};

int main()
{
    hash_set num_set(7);

    num_set.insert(10);
    num_set.insert(15);
    num_set.insert(20);
    num_set.insert(28); // 0
    num_set.insert(7); // 0, 28과 중복되어서 덮어써버림
    num_set.print();

    cout << (num_set.find(7) ? "found" : "not found") << endl;

    num_set.erase(7);
    num_set.print();

    return 0;
}
```


### 5. 해시 함수(hash function)
- 주어진 데이터로부터 **(가급적) 고유한 숫자 값**을 계산하는 함수
- 보통 함수의 출력은 고정된 범위의 정수로 매핑됨
- 예를 들어, h(k) = k%n 함수는 입력 k를 [0, n-1] 범위의 정수로 매핑


### 6. 키(key)
- 해시 함수의 입력으로, 입력 데이터 자체(정수, 실수, 문자열 등)이거나 또는 데이터를 구분하는 값


### 7. 해시 값(hash value)
- 해시 함수의 출력으로, 보통 해시 테이블의 인덱스로 사용됨
- 해시 코드(hash code) 또는 단순히 해시(hash)라고도 부름


### 8. 해시 테이블(hash table)
- 입력 데이터가 저장되는 배열로서, 해시 함수에 의해 계산된 인덱스에 데이터가 저장됨
- 해시 맵(hash map) - (key, value)
- 해시 셋(hash set) - (key)


### 9. 버킷(bucket)
- 해시 테이블에서 하나의 데이터가 저장된 공간
    - 배열이면 배열의 한 공간
- 슬롯(slot) / cell


### 10. **충돌**(collision)
- 해시 함수가 서로 다른 키에 대해 같은 해시 값을 반환함으로써, 다수의 키가 같은 해시 값을 갖게 되는 현상
- **체이닝(chaining), 오픈 어드레싱(open addressing)등의 방법으로 해결**


### 11. 실수 해시 함수
- 주어진 실수를 조작하거나 또는 실수의 각 숫자를 조합하여 해시 값 생성
- 예를 들어, 실수 키 X가 주어질 경우, 0 ~ 1 사이의 임의의 실수 A를 곱한 결과의 소수점 아래 부분을 s로 설정
- 이후 s에 정수 M을 곱한 후, 소수점 아래를 버림으로써 [0, m-1] 사이의 정수를 얻을 수 있음


### 12. 문자열 해시 함수 (다항식 롤링 해싱)
- 문자열의 각 문자를 ASCII 코드로 변환(0 ~ 128)이 값을 조합하여 해시 값 생성
- 예를 들어, "cat" 문자열을 정수 형태로 변환할 경우, 'c'=99, 'a'=97, 't'=116이므로 
- 99 * 128^2 + 97*128^1 + 116*128^0 = 1634548을 얻을 수 있음
- 이후 해시 테이블 크기에 맞게 나머지 연산을 수행


### 13. 좋은 해시 함수의 조건
- 주어진 데이터의 형태에 맞게 가공해서 생성
- 빠르고 효율적인 연산
- 해시 값이 균일하게 분포
    - 충돌 최소화
    - 해시 테이블 사용 효율이 높을 것
    - 사용할 키의 모든 정보를 이용


### 14. 추가 내용
- 해시는 항상 O(1)의 탐색/삽입/삭제가 아님(평균!!)
    - 하나의 인덱스로 몰리는 충돌이 발생하면 O(N)이 될 수도 있음

- 좋은 해시 함수의 조건
    - **동일한 입력**에 대해서는 반드시 동일한 해시 값이 나와야 함(실행될 때마다 달라지면 절대 안됨)



---
# S02. 해시 충돌



### 1. 
