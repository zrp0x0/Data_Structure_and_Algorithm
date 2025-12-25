# S01. 배열과 std::array


### 1. 배열이란
- 같은 종류의 데이터가 **연속적**으로 저장되어 있는 자료 구조
- 다섯 학생의 점수를 저장하려면?
    ```cpp
    int score1, score2, score3, score4, score5; // Bad
    int scores[5]; // Good
    ```
- BA(Base Address): 배열의 시작 주소
- sizeof(type): 원소 하나에 필요한 메모리 크기


### 2. 배열의 특징
- **인덱스(index)**를 사용하여 원하는 **원소(element)**에 곧바로 접근 가능: O(1)
- **캐시 지역성(cache locality)**
    - 배열의 각 원소는 서로 인접해 있기 때문에 하나의 원소에 접근할 때 그 근방에 있는 원소도 함께 캐시(cache)로 가져옴
- 반복문에서 배열을 사용하면 효율적인 프로그래밍 가능
- 상수 또는 상수표현식으로 크기를 지정(크기 불변)
    - C언어의 VLA(Variable Length Array): 가변 크기의 배열 가능
    - C++에서는 공식적으로 지원하지 않음
- 스택 메모리 영역에 할당 -> 보통 1MB 할당 (윈도우)


### 3. C 스타일 배열
```cpp
#include <iostream>
using std::cout;
using std::endl;

int main()
{
    int scores[5] = {50, 60, 70, 80, 90}; // 초기화하지 않으면 쓰레기 값이 들어감 / {}: 0으로 초기화 / {_, _, ...}: 임의의 _값으로 초기화

    int sum = 0;
    int scores_size = (sizeof(scores)) / (sizeof(scores[0])); // sizeof는 연산자
    // int scores_size = std::size(scores); // C++ 17

    for (int i = 0; i < scores_size; i++)
    {
        sum += scores[i];
    }

    float mean = 0;
    mean = (float)sum / scores_size;

    cout << "Mean score: " << mean << endl;

    return 0;
}
```


### 4. C 스타일 2차원 배열
- 2d_array[행번호][열번호]
```cpp
int mat[2][3] = {
    {1, 2, 3}, // [0][0], [0][1], [0][2]
    {4, 5, 6}  // [1][0], [1][1], [1][2]
};
```
- C언어에서의 2차원 배열은 인덱스 번호를 두 개를 사용하지만
- 결국 1차원 배열, 즉 연속된 메모리 공간에 할당됨
    - 실제 메모리 저장 순서(행 단위): [0][0] [0][1] [0][2] [1][0] [1][1] [1][2]
```cpp
int s = 0;
for (int r = 0; r < 2; r++)
{
    for (int c = 0; c < 3; c++)
    {
        s += mat[r][c];
    }
}
```
- 만약 r(row), c(column)의 순서를 바꿔서 사용한다면, 
- [0][0] [1][0] [0][1] [1][0] [0][2] [1][2]로 접근함
- 캐시 지역성이라는 특징을 살릴 수 없음 (실제 프로그램 동작 속도가 느려짐)


### 5. std::array
- C++에서 C 스타일 배열을 대체하는 **고정 크기** 컨테이너 (C++ 11)
- **원소의 타입**과 **배열 크기**를 매개변수로 사용하는 클래스 템플릿
- <array> 헤더 파일에 정의되어 있음
```cpp
template<class T, std::size_t N>
struct array;
// T: array에 담을 자료형 타입
// N: array에 담을 자료형 개수

// 간략한 예시
template <class T, std::size_t>
class array
{
public:
    T data[N];

    T& operator[](int index) // 연산자 오버로딩 사용
    {
        return data[index];
    }
}
```


### 6. std::array 특징
- C 스타일 배열처럼 사용할 수 있는 [] 연산자 오버로딩 제공
- 대입 연산자 지원 (깊은 복사)
- 배열 크기를 정확하게 알 수 있음 (array::size())
- 반복자 연산자 지원


### 7. std::array 사용 예제
```cpp
#include <iostream>
#include <array>
using std::cout;
using std::endl;

int main()
{
    // int scores[5] = {50, 60, 70, 80, 90};
    std::array<int, 5> scores = {50, 60, 70, 80, 90}; 
    // 반드시 크기를 입력해야함 (C 스타일은 크기를 입력하지 않고 원소의 개수로 크기를 정할 수 있음)
    // ex) int scores[] = {50, 60, 70, 80, 90}; => 자동으로 5개로 잡힘
    // C++ 17에서는 std:: array scores = {50, 60, 70, 80, 90}; <int, 5> 자동 추론 가능

    int sum = 0;
    // int scores_size = (sizeof(scores)) / (sizeof(scores[0]));
    // int scores_size = std::size(scores);
    int scores_size = scores.size();

    for (int i = 0; i < scores_size; i++)
    {
        sum += scores[i]; // [] 연산자 오버로딩
    }

    float mean = 0;
    mean = (float)sum / scores_size;

    cout << "Mean score: " << mean << endl;

    return 0;
}
```


### 8. std::array 단점
- 배열 크기를 명시적으로 지정해야함
- 항상 스택 메모리를 사용 (대용량 데이터 저장 X)
- 개발자들은 고정 크기 배열이 아닌 가변 크기 배열을 더 선호함
- 위의 3개를 극복한 std::vector가 많이 사용됨


### 9. std::array를 함수 인자로 넘길 시 주의사항
- pass by value가 일어남
- C 배열은 배열의 시작 주소가 넘어가는 반면 std::array는 배열 자체가 복사가 됨
- 보통 const std::array<int, 5>& arr로 넘김
    - 대신 덕분에 std::array는 객체라 크기 정보를 잃어버리지 않음


### 10. 추가 보완 사항
- [] vs .at()
    - []은 성능은 빠르나 인덱스를 벗어난 접근도 가능해 위험함
    - .at()은 성능은 조금 느리나 인덱스 범위를 벗어나면 예외를 발생시킴



---
# S02. 동적 배열과 std::vector


### 1. 동적 메모리 할당
- 프로그램 실행 중 필요한 크기의 메모리 공간을 할당하여 사용하는 기법
- 가변적으로 크기를 변할 수 있음
- **동적으로 할당된 메모리는 사용이 끝나면 명시적으로 할당된 메모리를 해제해야함**
- C: malloc / calloc 함수로 메모리를 할당하고 free 함수로 메모리 해제
- C++: new 연산자로 메모리를 할당 / delete 연산자로 메모리 해제 (malloc/free 사용도 가능)


### 2. 동적 메모리 할당을 이용한 동적 배열 생성 및 해제
- new[] 연산자를 이용하여 동적 배열을 위한 메모리를 할당하고, delete[] 연산자를 이용하여 동적 배열 메모리를 해제
```cpp
int* ptr = new int[3];
// ptr[0], ptr[1], ptr[2]처럼 배열로 사용
delete[] ptr;
ptr = nullptr;
```
- 동적 메모리 할당은 힙(Heap) 메모리 영역을 사용하므로 대용량 배열도 할당 가능


### 3. 동적 배열 예제 코드
```cpp
#include <iostream>
using std::cout;
using std::endl;

int main()
{
	int *ptr = new int[3]{}; // 기본값으로 초기화 해주는 것이 좋음
	ptr[0] = 10;
	ptr[1] = 20;

	for (int i = 0; i < 3; i++)
	{
		cout << ptr[i] << endl;
	}

    delete[] ptr;
    ptr = nullptr; // nullptr로 해주어 재사용을 방지해주는 것이 좋음
}
```
- 기본값 초기화: {}을 사용하면 사용하고자하는 자료형/객체의 기본값(기본생성자)로 초기화해줌
- ptr=nullptr: 메모리를 해제한 후 반드시 다시 사용할 수 없도록 nullptr를 넣어주는 것이 좋음


### 4. 메모리를 자동으로 해제하는 동적 배열 클래스
- 동적 할당한 메모리를 실수로 해제하지 않는 경우가 있음
- 자동으로 해제되도록 만들 수 있음
```cpp
#include <iostream>
using namespace std;

class DynamicArray
{
public:
	explicit DynamicArray(int n) : _size(n)
	{
		_arr = new int[_size]{};
        cout << "DynamicArray(int n)" << endl;
	}

	~DynamicArray() 
    { 
        delete[] _arr; 
        cout << "~DynamicArray()" << endl;
    }

	unsigned int size() { return _size; }

	int &operator[](const int i) { return _arr[i]; }
	const int &operator[](const int i) const { return _arr[i]; }

private:
	unsigned int _size;
	int *_arr;
};

int main()
{
	DynamicArray da(5);
	da[0] = 10;
	da[1] = 20;
	da[2] = 30;

	for (int i = 0; i < da.size(); i++)
    {
        cout << da[i] << endl;
    }

	return 0;
}
```
- main 함수가 끝나면 메모리가 자동으로 해제됨


### 5. 템플릿으로 구현한 동적 배열 클래스
```cpp
#include <iostream>
using namespace std;

template <typename T>
class DynamicArray
{
public:
	explicit DynamicArray(unsigned int size) : _size(size)
	{
		_arr = new T[_size]{};
        cout << "DynamicArray(unsigned int size)" << endl;
	}

	~DynamicArray() 
    { 
        delete[] _arr; 
        cout << "~DynamicArray()" << endl;
    }

	unsigned int size() { return _size; }

	T& operator[](const int i) { return _arr[i]; }
	const T& operator[](const int i) const { return _arr[i]; }

private:
	unsigned int _size;
	T* _arr;
};

int main()
{
	DynamicArray<float> da(5);
	da[0] = 1.1;
	da[1] = 2.2;
	da[2] = 3.3;

	for (int i = 0; i < da.size(); i++)
    {
        cout << da[i] << endl;
    }

	return 0;
}
```


### 6. std::vector란
- C++에서 C 스타일 배열을 대체하는 **가변 크기** 컨테이너
    - 초기화 과정에 데이터의 크기를 지정하지 않아도 됨 / 할 수도 있음
    - **배열의 크기를 확장할 수 있음**
- 원소의 타입을 매개변수로 사용하는 클래스 템플릿
- <vector> 헤더 파일에 정의되어 있음
```cpp
template<
    class T, // Type
    class Allocator = std::allocator<T> // 내부에서 동적 메모리를 할당하는 방식을 지정할 수 있음
> class vector;
```
- std::allocator: 메모리 할당 시 new / 해제 시 delete // 왠만하면 건들지 않음

```cpp
template <class T, class Allocator = std::allocator<T>>
class vector
{
    T* data;
    unsigned int size; // 배열의 원소 개수
    unsigned int capacity; // 배열의 전체 용량
}
```



--
# S03. std::vector 사용법과 동작 방식
- vector는 array와 다르게 힙에 할당이 되고 가변 길이 배열이지만
- 데이터 연속성은 보장이 됨 (인덱스 접근 가능)


### 1. std::vector 객체 생성과 원소 참조
```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
	vector<int> v1; // 완전히 비어있는 형태
	// v1.push_back(10);
	// v1.push_back(20);
	// v1.push_back(30);

	vector<int> v2(10);					// 10개의 공간, 0으로 초기화
	vector<int> v3(10, 1);				// 10개의 공간, 1로 초기화
	vector<int> v4{10, 20, 30, 40, 50}; // 5개의 원소가 들어있는 공간

    vector<int> v5 = v4; // 깊은 복사 (복사 대입 연산자)
    vector<int> v6(v4); // 깊은 복사 (복사 생성자)

    vector<int> v7(v4.begin(), v4.begin() + 3); // 0 ~ 2까지 넣음

    for (int i = 0; i < v6.size(); i++)
    {
        cout << v6[i] << endl;
    }

	return 0;
}
```


### 2. std::vector를 이용한 2차원 벡터 생성과 원소 참조
- **vector의 2차원 배열은 각각 다른 힙에 할당함므로 캐시 지역성을 보장하지는 않음**
```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<vector<int>> mat1(2, vector<int>(3, 1)); // 2행, 초기화를 vector(3열)짜리로 초기화
    
    vector<vector<int>> mat2{
        {1, 2, 3}, 
        {4, 5, 6} 
    };

    for (int r = 0; r < mat2.size(); r++)
    {
        for (int c = 0; c < mat2[r].size(); c++)
        {
            cout << mat2[r][c] << " ";
        }
        cout << endl;
    }

    return 0;
}
```


### 3. std::vector 주요 멤버 함수와 기능
- operator[]: 특정 위치 원소의 참조를 반환 / .at()
- front(): 첫 번째 원소의 참조를 반환
- back(): 마지막 원소의 참조를 반환

- push_back(): 마지막에 원소를 추가
- emplace_back(): push_back()과 같지만 객체의 복제나 이동이 없어서 **효율적**
- pop_back(): 마지막 원소를 삭제 (마지막 원소를 반환하지 않음)

- insert(): 특정 위치에 원소를 삽입
- erase(): 특정 위치의 원소를 삭제

- clear(): 모든 원소를 삭제
- size(): 원소의 개수를 반환
- capacity(): 미리 할당되어있는 메모리 공간(용량)의 크기를 반환
- empty(): 벡터가 비어 있으면 true를 반환

- **emplace_back()**
    - push_back(10)은 임시 객체 10을 만든 뒤 벡터 안으로 복사/이동시키지만
    - emplace_back(10)은 벡터가 관리하는 힙 메모리 공간에 직접 int(10)을 생성함
    - emplace_back(객체의 생성자 인자들 입력...): 객체 생성자 인자를 입력받고 그걸 기반으로 객체를 생성 후 삽입


### 4. std::vector::push_back() 동작 방식
```cpp
vector<int> vec1 {1, 2, 3, 4}; // <= 101 번지 (size: 4, capacity: 4)

vec1.push_back(5); // <= 201번지로 이동 (size: 5, capacity: 8)
// - 이전 capacity의 1.5 ~ 2배 정도 크기로 복사
// - 101번지 메모리 해제

vec1.push_back(6); // <= 201 번지 (size: 6, capacity: 8)
```
- **주의 사항**
    - 만약 push_back등의 재할당이 일어나면 주소가 이동되기 때문에
    - 이전에 사용하던 iterator는 쓰레기 값을 가리키게 됨


### 5. std::vector::insert()와 std::vector::erase() 동작 방식
```cpp
// vec1 = {1, 2, 3, 4, 5, 6}

vec1.insert(vec1.begin(), 0); 
// - vec1 맨 앞에 0을 추가
// - 기본 vec1에 있는 값들을 뒤로 밀어서(복사) 0의 자리를 만들어줌
// - vec1 = {0, 1, 2, 3, 4, 5, 6}


vec1.erase(vec1.begin() + 1, vec1.begin() + 3);
// - 1, 2를 삭제함
// - 다시 빈 공간을 채워줌(복사)
// - 근데 공간을 채워주기 위해서 앞으로 가져와도 이전에 있던 위치의 값을 딱히 어떻게 하지 않음
// - 엄밀히 말하면 기본 자료형은 소멸자가 없기 때문, 기본 자료형이 아닌 소멸자가 있는 객체면 소멸자를 호출함
// - size 자체로 접근을 못하게 막아버림
```


### 6. std::vector 사용 예제
```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> v1 {1, 2, 3, 4};
    cout << v1.capacity() << " : " << v1.size() << endl;

    v1.push_back(5);
    cout << v1.capacity() << " : " << v1.size() << endl;

    v1.push_back(6);
    cout << v1.capacity() << " : " << v1.size() << endl;

    v1.insert(v1.begin(), 0);
    cout << v1.capacity() << " : " << v1.size() << endl;
    for (int i = 0; i < v1.size(); i++)
        cout << v1[i] << " ";
    cout << endl;

    v1.erase(v1.begin() + 1, v1.begin() + 3);
    cout << v1.capacity() << " : " << v1.size() << endl;
    for (int i = 0; i < v1.size(); i++)
        cout << v1[i] << " ";
    cout << endl;

    for (int i = 0; i < v1.capacity(); i++)
        cout << v1[i] << " ";
    cout << endl; // 쓰레기 값이 출력됨

    for (int i = 0; i < v1.capacity(); i++)
        cout << v1.at(i) << " "; // size(인덱스) 초과를 에러로 잡아줌
    cout << endl;

    return 0;
}
```


### 7. std::vector 주요 멤버 함수와 기능 Review
- operator[]: 특정 위치 원소의 참조를 반환 / .at()
- front(): 첫 번째 원소의 참조를 반환
- back(): 마지막 원소의 참조를 반환

- push_back(): 마지막에 원소를 추가
- emplace_back(): push_back()과 같지만 객체의 복제나 이동이 없어서 **효율적**
- pop_back(): 마지막 원소를 삭제 (마지막 원소를 반환하지 않음)
-- 위에 다 O(1)
-- capacity 증설 작업을 하는데 비용이 들긴하지만, 그 빈도가 매우 낮음

- insert(): 특정 위치에 원소를 삽입
- erase(): 특정 위치의 원소를 삭제
-- O(N): 데이터를 뒤 또는 앞으로 복사하는 비용이 발생

- clear(): 모든 원소를 삭제
- size(): 원소의 개수를 반환
- capacity(): 미리 할당되어있는 메모리 공간(용량)의 크기를 반환
- empty(): 벡터가 비어 있으면 true를 반환
-- O(1)

- reserve(100): capacity만 100으로 늘림. size는 그대로 (재할당 방지용 최적화)
- resize(100): size를 100으로 늘림. 늘어난 공간에는 기본값이 채워짐
-- 둘의 차이점은 reserve는 size는 0인거고, resize는 size를 늘려서 그 다음부터 사용을 하거나(push_back) 원소 접근([])으로 사용
-- 보통 reserve를 미리 사용해서 사용가능한 배열의 크기를 어느 정도 잡아놓아서 재할당 최적화를 진행함

- shrink_to_fit(): erase()를 하더라도 capacity는 줄어들지 않음
    - v1.shrink_to_fit(): 현재 size에 맞춰서 capacity를 줄여줌 (메모리 낭비 방지)
    - 주의: 메모리 이전을 함!! (O(N))



---
# S04. 주요 모던 C++ 문법


### 1. 주요 모던 C++ 문법
- auto
- using
- 범위 기반 for문
- 람다 표현식
- chrono 라이브러리


### 2. 타입 추론 auto 키워드
- 변수 선언 시 타입 자리에 auto 키워드를 지정하면 **컴파일 시간**에 자동으로 타입을 추론하여 결정
- 복잡한 타입 또는 범용적인 코드 작성 시 유리
- const 또는 레퍼런스(&) 속성을 사용해 auto를 한정할 수 있음
- auto는 선언과 동시에 초기화하지 않으면 컴파일 에러가 발생함

```cpp
vector<int> odds()
{
    return {1, 3, 5, 7, 9};
}   
auto a1 = 10; // int
auto a2 = 3.14f; // float
auto a3 = "hello"; // const char*
auto a4 = "hello"s; // std::string
auto a5 = sqrt(2.0); // double
auto a6 = odds(); // vector<int>
auto a7 = a6.begin(); // vector<int>::iterator
auto lambda = [](int x) {return x * 2;}
```


### 3. using
- 타입 정의
    - C++ 11 이전이라면 typedef를 사용하고, C++ 11 이후라면 using 사용을 권장
    - using 문법이 읽기 더 쉽고, 템플릿 별칭도 지원함

```cpp
// C++ 11 이전
typedef unsigned char uchar;
typedef pair<int, string> pis;
typedef double da10[10];
typedef void(*func)(int);

// C++ 11 이후
using uchar = unsigned char;
using pis = pair<int, string>;
using da10 = double[10];
using func = void(*)(int);
da10 arr{};
func fp = &my_function;

template <typename T>
using matrix1d = vector<T>;
matrix1d<float> vec(3);
```


### 4. 범위 기반 for 문(range-based for)
- 배열 또는 STL 컨테이너에 들어있는 모든 원소를 순차적으로 접근하는 C++의 새로운 반복문
- 사용자 정의 타입에 대해 범위 기반 for문을 지원하려면 std::begin(), std::end() 함수를 지원해야함
```cpp
vector<int> numbers {10, 20, 30};

for (int n : numbers)
{
    cout << n << " ";
}
cout << endl;

for (auto& n : numbers) // auto도 가능 / &를 사용하면 참조해서 가져올 수 있음
{
    cout << n << " ";
}
cout << endl;

string strs[] = {"I", "love", "you"};

for (const auto& s : strs) // &를 사용할 때는 보통 const를 사용해서 변경하지 못하게 하는 것이 좋음
{
    cout << s << " ";
}
cout << endl;
```
- &를 사용해서 데이터를 읽는 것이 훨씬 빠름


### 5. 람다 표현식 (Lambda Expression)
- C++ 11에서 지원하는 이름 없는 함수 객체
- 함수의 포인터 또는 함수 객체(functor)를 대체
- 캡처 블록(Capture Clause): []
    - [=]: 외부 변수를 모두 값으로 복사해서 사용
    - [&]: 외부 변수를 모두 참조해서 사용
    - 더 부가적인 내용이 있지만 필요할 때 배우자!!

```cpp
auto square = [](double a) {return a * a;}
cout << "square(1.414) = " << square(1.414) << endl;

vector<Person> students;
students.push_back({"Kim1", 20});
students.push_back({"Kim2", 30});
students.push_back({"Kim3", 40});

sort(students.begin(), students.end(), [](const Person& p1, const Person& p2) {
    return p1.name < p2.name; // 이름 기준 **오름차순 정렬**
});

for (const auto& p : students)
    cout << p.name << " : " << p.age << endl;
```


### 6. chrono 라이브러리
- OS 독립적으로 정밀한 시간 측정 가능
- 나노초 단위까지 측정 가능
- 특정 연산 전후로 time_point를 측정하고, time_point차이를 이용하여 실제 연산 시간을 계산
    - 전체 타입을 모두 명시하여 코드를 작성하면 복잡하므로 auto 사용 예제 코드 참고
- 프로그램 동작 시간을 제대로 측정하려면 g++ 사용 시에는 -O2 옵션을 사용하고
- Visual Studio에서는 Release 모드로 빌드해야함
- <chrono>에 정의되어 있음
```cpp
auto start = chrono::system_clock::now();
auto start = chrono::steady_clock::now(); // 추천

// Do Something!!

auto end = chrono::system_clock::now();
auto msec = chrono::duration<double>(end - start).count() * 1000;
cout << "Elapsed time: " << msec << "ms." << endl;
```
- g++ test.cpp -O2 -o test
