# S01. 큐와 std::queue


### 1. 큐(queue)란
- queue: 땋아 늘인 머리 / **대기열**
- 자료 구조에서 큐는 데이터를 마치 줄을 세워 늘여 놓듯이 저장하는 **선형 자료 구조**로서,
- **선입선출(FIFO / First In First Out)** 원리에 따라 삽입과 삭제가 수행됨
    - 선입 선출: **먼저 들어온** 데이터가 **먼저 출력됨**
- 데이터 삽입은 뒤쪽(rear)에서, 데이터 삭제는 앞쪽(front)에서 수행되는 리스트
- ex. 매표소의 대기열, 프린터 큐, Windows 메시지 처리


### 2. 큐의 주요 연산
- enqueue(e): 큐의 맨 뒤(rear)에 원소 e를 추가. push(e)
- dequeue(): 큐의 맨 앞(front)에 있는 원소를 삭제. pop()
- front(): 큐의 맨 앞(front)에 있는 원소를 참조. peek()
- empty(): 큐가 비어 있으면 true를 반환
- size(): 큐의 원소 개수를 반환


### 3. 큐의 동작
```cpp
enqueue(10)
enqueue(20)
enqueue(30)
```
- [rear] [30] [20] [10] [front]

```cpp
dequeue()
front() // 20
```
- [rear] [30] [20] [front]


### 4. 스택 vs 큐
- 스택은 후입선출 (Last In First Out)
- 큐는 선입선출 (First In First Out)


### 5. 배열을 이용한 큐의 구현
- 미리 정해진 크기의 배열을 할당하고, 큐의 앞과 뒤를 나타내는 front와 rear 변수를 사용
    - enqueue(e): rear 값을 1증가시키고, 해당 위치 arr[rear]에 e를 대입
    - dequeue(): front 값을 1증가시킴
    - front(): arr[front] 값을 반환


### 6. 연결리스트를 이용한 큐의 구현
- 이중 연결 리스트를 이용하여, 새로 추가한 데이터를 연결 리스트 맨 앞에 삽입하는 방식
    - enqueue(e): push_back(e)
    - dequeue(): pop_front()
    - front(): header->next 값을 반환


### 7. std::list를 이용한 큐의 구현
```cpp
template <typename T>
class Queue
{
public:
    Queue() {}

    void enqueue(const T& e)
    {
        lst.push_back(e);
    }

    void dequeue()
    {
        lst.pop_front();
    }

    const T& front() const
    {
        return lst.front();
    }

    bool empty() const 
    {
        return lst.empty();
    }

    size_t size() const
    {
        return lst.size();
    }

private:
    std::list<T> lst;
}
```


### 8. 구현한 Queue 클래스 동작 확인
```cpp
#include <iostream>
#include <list>

using std::cout;
using std::endl;
using std::list;

template <typename T>
class Queue
{
public:
    Queue() 
    {

    }

    void enqueue(const T& e)
    {
        lst.push_back(e);
    }

    void dequeue()
    {
        lst.pop_front();
    }

    const T& front()
    {
        return lst.front();
    }

    bool empty()
    {
        return lst.empty();
    }

    size_t size()
    {
        return lst.size();
    }

private:
    list<T> lst;
};

int main()
{
    Queue<int> q;
    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);
    cout << q.front() << endl; // 10

    q.dequeue();
    cout << q.front() << endl; // 20

    while (!q.empty())
    {
        const int& e = q.front();
        cout << e << " "; // 20 30
        q.dequeue();
    }
    cout << endl;

    return 0;
}
```


### 9. std::queue
- FIFO 방식의 큐 자료 구조를 구현한 **컨테이너 어댑터**
```cpp
template <class T, class Container = std::deque<T>>
class queue;
```

- 템플릿 매개변수 T는 큐에 저장할 타입을 지정하고, Container에는 내부에서 사용할 컨테이너를 지정
- Container에는 dequeue 또는 list를 지정할 수 있음
- <queue>에 정의되어 있음
    - queue::push(e) // enqueue(e)
    - queue::pop() // dequeue()
    - queue::front() // front()


### 10. std::queue 사용 예제
```cpp
#include <iostream>
#include <queue>

using std::cout;
using std::endl;
using std::queue;

int main()
{
    queue<int> q;
    q.push(10);
    q.push(20);
    q.push(30);
    cout << q.front() << endl;

    q.pop();
    cout << q.front() << endl;

    while (!q.empty())
    {
        cout << q.front() << " ";
        q.pop();
    }
    cout << endl;

    return 0;
}
```


### 11. 추가 내용
- 왜 Container에는 vector를 사용하지 않을까?
    - queue는 앞의 원소를 가져올 수 있어야함
    - vector에서 pop_front는 O(N)의 시간이 걸림
    - queue의 pop_front(front)는 O(1)의 시간이 걸려야함
