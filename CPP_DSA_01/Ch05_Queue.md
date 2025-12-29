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



---
# S02. 환형 큐(Circular Queue)


### 1. 배열을 이용한 큐의 구현
- 크기가 8인 배열에 3개의 데이터가 추가된 큐의 상태
    - [[front]10] [20] [[rear[30]]]

```cpp
template <typename T>
class Queue
{
private:
    T* arr;
    int front;
    int rear;
    int count;
    int capacity;
}
```

- 큐에 새로운 데이터 추가하기 (enqueue(40))
    - [[front]10] [20] [30] [[rear]]
    - [[front]10] [20] [30] [[rear]40]

```cpp
void enqueue(const T& e)
{
    rear++;
    arr[rear] = e;
    count++;
}
```
- 근데 이렇게 하면 Overflow가 발생할 수 있음!!

```cpp
void enqueue(const T& e)
{
    if (full())
    {
        cout << "Overflow Error!" << endl;
        return;
    }

    rear++;
    arr[rear] = e;
    count++;
}
```
- 배열이 가득 찾는지 확인해야함

- 큐에서 데이터 삭제하기
```cpp
void dequeue()
{
    front++;
    count--;
}
```
- 만약 배열이 비어있다면 오류가 발생함

```cpp
void dequeue()
{
    if (empty())
    {
        cout << "Underflow Error!" << endl;
        return;
    }

    front++;
    count--;
}
```


### 2. 배열을 이용한 큐의 구현에서 큐의 초기 상태
- rear = -1
- front = 0
- rear을 1증가 시키고 데이터를 넣고
- front에 있는 데이터를 꺼내고(안 꺼내도 되긴함) front를 1증가 시킴
```cpp
#define MAX_QUEUE 10

Queue(int sz = MAX_QUEUE)
{
    arr = new T[sz];
    front = 0;
    rear = -1;
    count = 0;
    capacity = sz;
}
```


### 3. 배열을 이용한 큐의 문제점
- 큐에 데이터의 삽입과 삭제가 지속적으로 발생할 경우, **배열의 앞 부분에 무효화되는 공간이 늘어남**
    - **rightward drift**
    - [10] [20] [30] [40] [[front]50] [[rear]60]

- 무효화되는 공간을 재사용하자!! => 환형 큐(원형 큐)


### 4. 환형 큐 (Circular Queue)
- 선입 선출 원칙에 따라 작업이 수행되고
- 마지막 위치가 마치 원을 이루듯이 다시 첫 번째 위치에 연결되는 선형 데이터 구조
- 큐의 front / rear가 시계 방향으로 한 칸씩 이동하는 상태
    - 실제 구현에서는 배열 전체 크기를 이용하여 **나머지(%)** 연산을 수행
- 큐가 empty() 또는 full() 상태인지를 확인하기 위해 원소의 개수를 따로 저장해야함


### 5. 환형 큐에서 데이터 추가
- full 검사
- rear을 1증가하고 rear에 원소 추가


### 6. 환형 큐에서 데이터 삭제
- empty 검사
- front에 있는 데이터 처리(삭제)
- front를 1증가


### 7. 환형 큐 구현
```cpp
#include <iostream>

#define MAX_QUEUE 10

using std::cout;
using std::endl;

template <typename T>
class Circular_Queue
{
public:
    Circular_Queue(int sz = MAX_QUEUE)
    {
        arr = new T[sz];
        capacity = sz;
        count = 0;
        rear_idx = -1;
        front_idx = 0;
    }   

    ~Circular_Queue()
    {
        delete[] arr;
    }

    void enqueue(const T& e)
    {
        if (full())
        {
            cout << "Overflow Error!" << endl;
            return;
        }   

        rear_idx = (rear_idx + 1) % capacity;
        arr[rear_idx] = e;
        count++;
    }

    void dequeue()
    {
        if (empty())
        {
            cout << "Underflow Error!" << endl;
            return;
        }

        front_idx = (front_idx + 1) % capacity;
        count--;
    }

    const T& front()
    {
        if (empty())
        {
            cout << "Queue is Empty!" << endl;
            return T();
        }

        return arr[front_idx];
    }

    bool full()
    {
        return count == capacity;
    }

    bool empty()
    {
        return count == 0;
    }

    int size()
    {
        return count;
    }

private:
    T* arr;
    int front_idx;
    int rear_idx;
    int count;
    int capacity;
};

int main()
{
    Circular_Queue<int> cq;

    cq.dequeue(); // Underflow Error
    cq.enqueue(10); 
    cq.enqueue(20);
    cq.enqueue(30);
    cq.enqueue(40);
    cq.enqueue(50);
    cq.enqueue(60);
    cq.enqueue(70);
    cq.enqueue(80);
    cq.enqueue(90);
    cq.enqueue(100);
    cq.enqueue(999); // Overflow Error

    cout << cq.front() << endl; // 10

    cq.dequeue(); // 20 ~ 100
    
    while (!cq.empty())
    {
        cout << cq.front() << " "; // 20 ~ 100
        cq.dequeue();
    }
    cout << endl;

    return 0;
}
```


### 8. 추가 내용
- 환형 큐 구현에 있어서 count 변수 없이 구현
    - 메모리를 조금이라도 더 아끼려면 count 변수를 없애고 구현할 수 있음
    - 이때는 배열 전체의 한 칸을 검사를 위해서 남겨두는 방향으로 구현함
    - empty: rear == front;
    - full: (rear + 1) % (capacity + 1) == front;
    ```cpp
    #include <iostream>
    using std::cout;
    using std::endl;

    class CircularQueue
    {
    public:
        CircularQueue(int capacity)
        {
            _capacity = capacity;
            _arr = new int[_capacity + 1];
            _rear = 0;
            _front = 0;
        }

        ~CircularQueue()
        {
            delete[] _arr;
        }

        void push(int e)
        {
            if (full())
            {
                cout << "Queue is Full!" << endl;
                return;
            }

            _arr[_rear] = e;
            _rear = (_rear + 1) % (_capacity + 1);
        }

        void pop()
        {
            if (empty())
            {
                cout << "Queue is Empty!" << endl;
                return;
            }

            _front = (_front + 1) % (_capacity + 1);
        }

        int front()
        {
            if (empty())
            {
                cout << "Queue is Empty!" << endl;
                return 0;
            }

            return _arr[_front];
        }

        bool empty()
        {
            return _rear == _front;   
        }

        bool full()
        {
            return ((_rear + 1) % (_capacity + 1)) == _front;
        }

    private:
        int* _arr;
        int _rear;
        int _front;
        int _capacity;
    };

    int main()
    {
        CircularQueue cq(5);
        cq.push(1);
        cq.push(2);
        cq.push(3);
        cq.push(4);
        cq.push(5); // 1 2 3 4 5
        cq.push(6); // Queue is Full!

        cq.pop(); // 2 3 4 5

        while (!cq.empty())
        {
            cout << cq.front() << " "; // 2 3 4 5
            cq.pop();
        }
        cout << endl;

        return 0;
    }
    ```

- enqueue / dequeue / front의 시간 복잡도는 O(1)


- 엄밀한 환형 큐
```cpp
#include <iostream>
#include <stdexcept> // 예외 처리를 위해 추가

template <typename T>
class Circular_Queue {
public:
    explicit Circular_Queue(size_t sz = 10) : capacity(sz), count(0), front_idx(0), rear_idx(sz - 1) {
        arr = new T[capacity];
    }

    ~Circular_Queue() {
        delete[] arr;
    }

    // 복사 방지 (엄밀성 추가)
    Circular_Queue(const Circular_Queue&) = delete;
    Circular_Queue& operator=(const Circular_Queue&) = delete;

    void enqueue(const T& e) {
        if (full()) throw std::overflow_error("Queue Overflow");

        rear_idx = (rear_idx + 1) % capacity;
        arr[rear_idx] = e;
        count++;
    }

    void dequeue() {
        if (empty()) throw std::underflow_error("Queue Underflow");

        front_idx = (front_idx + 1) % capacity;
        count--;
    }

    const T& front() const { // 읽기 전용 const 추가
        if (empty()) throw std::out_of_range("Queue is Empty");
        return arr[front_idx];
    }

    bool full() const { return count == capacity; }
    bool empty() const { return count == 0; }
    size_t size() const { return count; }

private:
    T* arr;
    size_t front_idx;
    size_t rear_idx;
    size_t count;
    size_t capacity;
};
```



---
# Ch03. 양방향 큐와 std::deque


### 1. 양방향 큐(deque)란?
- deque: Double Ended Queue
- **삽입과 삭제**가 **양쪽** 끝에서 모두 가능한 자료 구조
    - push_front
    - pop_front
    - push_back
    - pop_back


### 2. 양방향 큐의 주요 연산
- push_front(e)
- pop_front()

- push_back(e)
- pop_back()

- front()
- back()

- empty()
- size()


### 3. 배열을 이용한 양방향 큐 구현
- 환형 큐와 비슷한 방식으로 양방향 큐의 맨 앞과 맨 마지막 위치를 갱신
- 만약 인덱스를 벗어난다면 맨 뒤 혹은 앞으로 이동시키고 거기에 넣으면 됨
```cpp
과제
```


### 4. 연결 리스트를 이용한 양방향 큐 구현
- 이중 연결 리스트를 이용하여 연결 리스트의 맨 앞과 맨 뒤에 자료를 각각 추가/삭제 가능
- 
```cpp
과제
```


### 5. std::deque
```cpp
template <class T, class Allocator = std::allocator<T>>
class deque;
```

- 양 방향 큐의 동작을 지원하는 **순차 컨테이너**
- <dequeue>에 정의되어 있음
- 특징
    - **원소에 대해 임의 접근 동작이 O(1) 시간 복잡도로 동작**
    - 맨 앞과 맨 뒤에 자료를 추가 또는 삭제하는 연산이 O(1) 시간 복잡도로 동작
    - 중간 위치에 자료를 삽입 또는 삭제는 O(N) 시간 복잡도로 동작
    - std::stack, std::queue등의 STL 컨테이너 어댑터의 기본 컨테이너로 사용


### 6. std::deque의 구현 방식
- C++ 표준은 어떤 기능의 동작만을 정의할 뿐이며, 어떻게 구현해야하는지는 정의하지 않음
- 보통 std::deque는 단일 메모리 청크(chunk)를 사용하지 않으며, 대신 크기가 같은 여러 개의 메모리 청크를 사용하여 데이터를 저장함


### 7. std::deque 사용 예제
```cpp
#include <iostream>
#include <deque>

using namespace std;

int main()
{
    deque<int> dq = {10, 20, 30, 40};

    dq.push_front(50);
    dq.push_back(0);

    cout << dq.front() << endl;
    cout << dq.back() << endl;

    dq.pop_back();
    dq.pop_front();

    dq[dq.size() - 1 - 1] = 999; 

    for (int i = 0; i < dq.size(); i++)
    {
        cout << dq[i] << " "; // 임의 접근 지원함
    }
    cout << endl;

    return 0;
}
```


### 8. 추가 내용
- std::deque의 임의 접근 O(1)의 진실
    - 불연속적인 청크 단위인데 어떻게 상수 시간에 접근이 가능할까?
    - dq[i]: (블록 번호 = i / 블록 크기) / (블록 내 인덱스 = i % 블록 크기)를 계산하여 바로 접근함
    - 예. 블록 크기 == 4 / dq[18] 
        - 18 / 4 == 4(블록 번호)
        - 18 % 4 == 2(블록 인덱스)
        - 주의 블록 번호/인덱스는 0부터 시작

- vector vs deque의 메모리 연속성
    - vector는 모든 원소가 연속으로 있음
    - deque는 청크 안에서는 연속이지만 청크 자체는 불연속적임
    - 포인터 연산으로 순회하는 것은 vector가 더 빠를 수 있음
    - 왜냐하면 deque는 청크를 넘어가는 경우 다음 청크의 주소를 계산해야하기 때문임
