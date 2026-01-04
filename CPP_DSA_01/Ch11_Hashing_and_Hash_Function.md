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



### 1. 충돌(collision)
- 해시 함수가 서로 다른 키에 대해 같은 해시 값을 반환함으로써, 해시 테이블에서 두 개 이상의 데이터가 같은 위치에 저장되려는 현상
- 해시 테이블의 크기가 저장할 데이터의 개수보다 작으면 반드시 충돌이 발생
- 해시 테이블의 크기가 저장할 데이터의 개수보다 크더라도 해시 함수의 동작에 따라 충돌이 발생할 수 있음


### 2. 대표적인 충돌 처리 기법
- 체이닝(chaining)
- 오픈 어드레싱(open addressing)
- 뻐꾸기 해싱(cuckoo hashing)


### 3. 체이닝(chaining)
- 해시 테이블의 특정 위치에서 하나의 키를 저장하는 것이 아니라 하나의 연결 리스트를 저장하는 기법
- 개별 체이닝(seperate chaining)
- 새로 삽입된 키에 의해 충돌이 발생하면 리스트에 새로운 키를 추가

```cpp
hash(X) = X % 7
hash_set num_set(7);
num_set.insert(10);
num_set.insert(20);
num_set.insert(27); // 20 - 27
```

- 단일 연결 리스트 사용시 앞에 저장 / 이중 연결 리스트 사용시 뒤에 저장해도 됨
- 해시 맵을 사용할 경우, (key, value)를 저장


### 4. 체이닝 특징
- 삽입 연산은 O(1)의 시간 복잡도로 동작
- (최악의 경우)모든 데이터가 같은 해시 값을 가질 경우, 탐색 연산은 O(N)의 시간 복잡도로 동작
- 삭제 연산의 경우, 먼저 탐색을 수행하므로 최악의 경우 O(N)의 시간 복잡도로 동작
- **해시 값이 중복되는 충돌을 최대한 방지할 수 있도록 해야함**


### 5. 부하율과 재해싱
- 부하율(load factor): 해시 테이블에서 각각의 리스트에 저장되는 키의 평균 개수. 적재율
    - a = n / m
        - n: 해시 테이블에 저장할 전체 키의 개
        - m: 해시 테이블 크기 (연결 리스트 개수)
        - 체이닝에서 부하율이 1을 넘는다는 것은 한 버킷에 여러 개의 노드가 달려있다는 뜻
        - 단순히 부하율만 보면 안됨. 하나의 버킷에 모든 데이터가 다 달려있을 수도 있음
        - 빈 버킷의 비율, 최대 체인 길이 등을 보고 해시 함수나, 리해싱을 고려해야함

- 부하율이 1보다 작으면 메모리의 낭비가 될 수 있고, 1보다 크면 탐색, 삭제 연산이 느리게 동작할 수 있음
    - 재해싱(rehashing)을 통해 부하율이 1에 가까운 값이 되도록 조정
    - 테이블의 크기를 키우는 동작
    - 모든 데이터를 다시 옮겨야하므로 O(N)의 시간이 걸리는 무거운 작업
    - 하지만 보통 아주 가끔 발생하므로 평균(Amortized) 시간 복잡도는 여전히 O(1)으로 유지됨


### 6. 체이닝을 구현한 해싱 클래스
```cpp
class hash_set
{
private:
    int sz;
    std::vector<std::list<int>> data;

public:
    hash_set(int n) : size(n), data(sz) {}

    int hash(int key) { return key % sz; }

    void insert(int value)
    {
        data[hash[value]].push_back(value);
    }

    bool find(int value)
    {
        auto& entries = data[hash(value)]; // 리스트를 반환
        return std::find(entries.begin(), entries.end(), value) != entries.end();
    }

    void erase(int value)
    {
        auto& entries = data[hash(value)];
        auto it = std::find(entires.begin(), entries.end(), value);

        if (it != entries.end())
            entries.erase(it);
    }
}
```


### 7. 체이닝을 구현한 해싱 클래스 예제 코드
```cpp
#include <iostream>
#include <list>
#include <vector>
#include <algorithm>

using namespace std;

class hash_set
{
public:
    hash_set(int size) : sz(size), data(sz) {}

    int hash(int key)
    {
        return key % sz;
    }

    void insert(int value)
    {
        if (find(value)) return; // 이미 있으면 무시
        data[hash(value)].push_back(value);
    }

    bool find(int value)
    {

        const auto& lst = data[hash(value)];

        return std::find(lst.begin(), lst.end(), value) != lst.end();
    }

    void erase(int value)
    {
        auto& lst = data[hash(value)];
        const auto& it = std::find(lst.begin(), lst.end(), value);

        if (it != lst.end())
            lst.erase(it);
    }

    void print() const
    {
        // int count = 0;
        // for (const auto& lst : data)
        // {
        //     cout << count << " : ";
        //     print_lst(lst); 
        //     count++;
        // }

        for (int i = 0; i < sz; i++)
        {
            cout << i << " : ";
            for (const auto& n : data[i])
            {
                cout << n << " ";
            }
            cout << endl;
        }
    }
 
    void print_lst(const list<int>& lst) const
    {
        for (const auto& n : lst)
        {
            cout << n << " ";
        }
        cout << endl;
    }

private:
    int sz;
    vector<list<int>> data;
};

int main()
{
    hash_set num_set(7);

    num_set.insert(10);
    num_set.insert(2);
    num_set.insert(16);
    num_set.insert(20);
    num_set.insert(27);
    num_set.insert(34);

    num_set.print();

    if (num_set.find(34))
    {
        cout << "Found" << endl;
    }
    else
    {
        cout << "Not Found" << endl;
    }

    num_set.erase(34);

    num_set.print();

    return 0;
}
```


### 8. 오픈 어드레싱(open addressing)
- 해시 충돌이 발생할 경우, 해시 테이블에서 다른 비어 있는 버킷을 찾아 데이터를 저장하는 방식. 열린 주소 지정
- 해시 테이블의 크기가 데이터 개수보다 커야함
- 새로운 위치 탐사(probing) 방식
    - 선형 탐사(linear probing)
    - 제곱 탐사(quadratic probing)
    - 이중 해싱(double hashing)

- 하상 O(1)일까? 여기에는 조건이 필요함
    - 부하율이 낮게 유지(0.7 이하)
    - 해시 함수가 데이터를 균일하게 뿌려주오 클러스터링을 최소화해야 한다는 전제가 필수적임


### 9. 선형 탐사(linear probing)
- 특정 해시 값에서 충돌이 발생하면 해당 위치에서 하나씩 다음 위치로 이동하면서 비어 있는 위치에 원소를 삽입
- 즉, h(key)가 사용중이라면 h(key) + 1, h(key) + 2, 순서로 빈 위치를 찾음
- 데이터가 특정 위치에 군집화(clustering)되는 경우, 해싱 효율이 떨어질 수 있음


### 10. 오픈 어드레싱: 제곱 탐사(quadratic probing)
- 고정 크리고 이동하는 선형 탐사와 달리 제곱수 크기로 이동하면서 비어 있는 위치를 찾는 방식
- 즉, h(key)가 사용중이라면 h(key) + 1^2, h(key) + 2^2, 순서로 빈 위치를 찾음
- 군집화를 피하기 위해 간격을 두고 데이터를 삽입하는 방식

- **C++ GCC는 chaining을 사용함!!**


### 11. 추가 내용
- 오픈 어드레싱 나머지 연산
    - index = hash % size;
    - index = hash & (size - 1);


- 오픈 어드레싱의 삭제 연산
    - 만약 A, B가 동일한 해시값을 가지게 되어 A는 3, B는 4에 저장
    - A를 삭제한 후 B를 검색하려고 하면 일단 3을 먼저 봄
    - 근데 데이터가 없으면, 데이터가 없다고 판단하고 탐색 종료
        - 이를 방지하기 위해 더미 값을 넣음

- 캐시 지역성
    - 체이닝: 리스트 노드들이 메모리 여기저기 흩어져 있어 CPU 캐시 효율이 낮음
    - 오픈 어드레싱: 모든 데이터가 배열에 붙어 있어 CPU 캐시 효율이 높음
    - **따라서 실제 실행 속도에 차이가 있을 수 있음**


    
