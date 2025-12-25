# S01. 스택과 std::stack


### 1. 스택(Stack)이란?
- 물건을 쌓아 올린 더미 또는 쌓는 행위
- 자류 구조에서 스택은 데이터를 쌓아 올리듯이 저장하는 선형 자료 구조로서
- **후입선출** 원리에 따라 삽입과 삭제가 수행됨
- **후입선출**: 나중에 들어온 데이터가 먼저 출력됨 (Last In First Out)
- 데이터의 입출력이 한쪽 방향에서만 수행되는 리스트
- 스택의 사용 예
    - 텍스트 편집기의 실행 취소 기능
    - 웹브라우저에 뒤로 가기
    - 함수 호출 시 복귀 주소 기억


### 2. 스택의 주요 연산
- push(e): 스택의 맨 위에 원소 e를 넣기
- pop(): 스택의 맨 위에 있는 원소를 삭제
- top(): 스택의 맨 위에 있는 원소를 참조. peek()
- empty(): 스택이 비어있으면 true 반환
- size(): 스택의 원소 수 반환


### 3. **배열**을 이용한 스택의 구현
- 미리 정해진 크기의 배열을 할당하고, 가장 최근에 입력된 자료 위치를 가리키는 t 변수를 사용
    - 초기 t == -1
    - push(e): t값은 1증가시키고, 해당 위치에 e를 삽입
    - pop(): t값을 1감소
    - top(): arr[t] 값을 반환

- 장점: 구현이 간단하고, 삽입이나 삭제가 빠르게 동작
- 단점: **미리 정해진 크기**보다 많은 데이터를 삽입할 경우, 스택 오버플로우 에러가 발생


### 4. **연결 리스트**를 이용한 스택의 구현
- **단순 연결 리스트**를 사용하여, 새로 추가한 데이터를 연결 리스트의 맨 앞에 삽입하는 방식
    - push(e): push_front(e)
    - pop(): pop_front()
    - top(): head->next 값을 반환

- 장점: 크기가 제한되지 않음
- 단점: 구현이 복잡하고, 삽입이나 삭제 시간이 배열에 비해 오래 걸림 (노드 간의 연결 - O(1)이라고는 하지만 배열은 단순히 t 값으로 제어하면 되기 때문임)


### 4. std::vector를 이용한 스택의 구현
```cpp
template <typename T>
class Stack
{
public:
    Stack() {}

    void push(const T& e) { v.push_back(e); }
    void pop() { v.pop_back(); } // v안에 size가 내장되어있음
    const T& top() const { return v.back(); }

    bool empty() const { return v.empty(); }
    int size() const { return v.size(); }

private:
    std::vector<T> v;
}
```

- 시간 복잡도: O(1)


### 5. std::vector를 이용한 스택 구현 예제 코드
```cpp
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

template <typename T>
class Stack
{
public:
    Stack() {}

    void push(const T& e)
    {
        v.push_back(e);
    }

    void pop()
    {
        v.pop_back();
    }

    const T& top() const
    {
        return v.back();
    }

    bool empty() const
    {
        return v.empty();
    }

    std::size_t size() const
    {
        return v.size();
    }

private:
    vector<T> v;
};

int main()
{
    Stack<int> stk;
    stk.push(10); // 10
    stk.push(20); // 10 20
    stk.push(30); // 10 20 30

    stk.pop(); // 10 20

    cout << stk.top() << endl; // 20
    stk.push(40); // 10 20 40

    while (!stk.empty())
    {
        auto& e = stk.top();
        cout << e << " ";
        stk.pop();
    }
    cout << endl;

    cout << stk.empty() << endl;

    return 0;
}
```


### 6. std::stack
- LIFO 방식의 스택 자료 구조를 구현한 **컨테이너 어댑터**
    - 컨테이너 어댑터: 기본적인 STL(vector, list)을 활용해서 구현
```cpp
template <class T, class Container = std::deque<T>>
class stack;
```

- 템플릿 매개변수 T는 스택에 저장할 타입을 지정
- Container에는 내부에서 사용할 컨테이너를 지정
- Container에는 deque(default), vector, list를 지정할 수 있음
- <stack>에 정의되어 있음


### 7. std::stack 사용 예제
```cpp
#include <iostream>
#include <stack>

using std::cout;
using std::endl;
using std::stack;


int main()
{
    stack<int> stk;
    stk.push(10); // 10
    stk.push(20); // 10 20
    stk.push(30); // 10 20 30

    stk.pop(); // 10 20

    cout << stk.top() << endl; // 20
    stk.push(40); // 10 20 40

    while (!stk.empty())
    {
        auto& e = stk.top();
        cout << e << " ";
        stk.pop();
    }
    cout << endl;

    cout << stk.empty() << endl;

    return 0;
}
```


### 8. 추가 학습
- 왜 기본 컨테이너가 deque일까?
    - vector는 스택이 커지면 데이터 전체를 복사하는 재할당이 일어남
    - deque는 새로운 메모리 블록만 할당하면 되기 때문에 안정적임
    - list보다도 안정적인 이유는 dequesms 일정 크기의 블록 단위로 메모리가 연속되어 있어 **캐시 지역성**이 보장됨

- push() => emplace() 지원

- top과 pop의 분리 이유
    - pop에서 값을 반환하다가 복사 생성자에서 예외가 발생하면 스택에서는 이미 데이터가 제거되었는데 반환된 값은 받지 못한 데이터 유실 상태가 발생




---
# S02. 스택의 응용: 문자열과 벡터 순서 뒤집기


### 1. 
