# S01. 연결 리스트 (Linked List)


### 1. 리스트(list)란?
- 순서를 가진 데이터의 모임(목록)
- e.g. TODO 리스트, 요일(일요일, 월요일, ..., 토요일)


### 2. 리스트의 주요 연산
- 원소의 참조, 삽입(insert), 삭제(remove), 검색(search), ...


### 3. 대표적인 리스트 구현 방법
- 배열
    - 연속된 메모리 공간
    - 원소의 삽입삭제 비효율적
    - 구현 쉬움

- 연결 리스트   
    - 임의의 메모리 공간
    - 원소의 삽입삭제 효율적
    - 구현 어려움


### 4. 연결 리스트(Linked List)란?
- 데이터와 링크로 구성된 노드(node)가 연결되어 있는 자료 구조
    - 데이터(data): 정수, 문자열, 복합 자료형 등
    - 링크(link, next): 다음 노드를 가리키는 포인터(엄밀하게 말하면 다른 노드를 가리키는 포인터)
    - 노드(node): 데이터와 링크로 이루어진 연결 리스트 구성 단위
    - ex. [Header] -> [[data][next]] -> [[data][next]] -> [[data][next]] -> NULL

    ```cpp
    struct Node
    {
        int data;
        Node* next;
    }
    ```


### 5. 연결 리스트 클래스
```cpp
struct Node
{
    int data;
    Node* next;
}

class LinkedList
{
private:
    Node* head;

public:
    LinkedList() : head(NULL) {}
    ~LinkedList();

    void push_front(int val);
    void pop_front();

    void insert(int index, Node* node); // 이건 수업 시간에 안 다룸

    void print_all();
    bool empty();
}
```


### 6. 연결 리스트 맨 앞에 노드 삽입하기
```cpp
void push_front(int val)
{
    Node* new_node = new Node {val, NULL};

    // 연결 리스트의 초기 상태 empty가 아닐 때
    if (head != NULL)
    {
        new_node->next = head;
    }
    head = new_node;
}
```


### 7. 연결 리스트 맨 앞 노드 삭제하기
```cpp
void pop_front()
{
    if (head == NULL)
        return;

    Node* delete_node = head;
    head = head->next;

    delete delete_node;
    delete_node = nullptr;
}
```


### 8. 연결 리스트 전체 데이터 출력하기
```cpp
void print_all()
{
    Node* currentNode = head;

    while (currentNode != NULL)
    {
        std::cout << currentNode->data << ", ";
        currentNode = currentNode->next;
    }
}
```


### 9. 연결 리스트 비어있는지 확인하기
```cpp
bool empty()
{
    return (head == NULL);
}
```


### 10. 연결 리스트 제거
```cpp
~LinkedList()
{
    while (!empty())
    {
        pop_front();
    }
}
```


### 11. 구현한 연결 리스트 클래스 동작 확인
```cpp
#include <iostream>

struct Node
{
    int data;
    Node* next;
};

class LinkedList
{
public:
    LinkedList() : head(NULL) {}
    ~LinkedList() 
    {
        while (!empty())
        {
            pop_front();
        }
    }

    void push_front(int val)
    {
        Node* new_node = new Node {val, NULL};

        if (head != NULL)
        {
            new_node->next = head;
        }
        head = new_node;
    }

    void pop_front()
    {
        if (head == NULL)
            return;

        Node* delete_node = head;
        head = head->next;

        delete delete_node;
        delete_node = nullptr;
    }

    void print_all()
    {
        Node* current_node = head;

        while (current_node != NULL)
        {
            std::cout << current_node->data << " ";
            current_node = current_node->next;
        }
        std::cout << std::endl;
    }

    bool empty()
    {
        return head == NULL;
    }

public:
    Node* head;
};

int main()
{
    LinkedList ll;

    ll.push_front(10);
    ll.push_front(20);
    ll.push_front(30);

    ll.print_all(); // 30 20 10

    ll.pop_front();
    ll.pop_front();

    ll.print_all(); // 10

    return 0;
}
```


### 12. 연결 리스트의 장단점
- 장점
    - 임의의 위치에 원소의 삽입/삭제가 효율적: O(1) - 이거 위치를 알고 있을때임!!
    - 크기 제한이 없음

- 단점
    - 임의의 원소 접근이 비효율적: O(N) - 이거 때문에 삭제할 위치를 찾는 과정은 비효율적임
    - 링크를 위한 여분의 공간 사용: next 포인터
    - 구현이 복잡



---
# S02. 이중 연결 리스트


### 1. 연결 리스트 종류
- 단순 연결 리스트 (singly linked list)
    - 다음 노드에 대한 링크만 가지고 있는 연결 리스트
    - 한쪽 방향으로만 순회(traverse)가 가능 (단방향 연결 리스트)
        - data
        - next

- 이중 연결 리스트 (doubly linked list)
    - 이전 노드와 다음 노드에 대한 링크를 가지고 있는 연결 리스트
    - 양방향 순회가 가능 (양방향 연결 리스트)
        - data
        - next
        - prev

- 원형 연결 리스트
    - 일반적인 연결 리스트의 마지막 노드 링크가 처음 노드를 가리키도록 구성된 자료 구조
    - 처음 노드가 다시 나타나면 순회를 멈춤


### 2. 이중 연결 리스트의 구조
- [ [prev] [data] [next] ]
```cpp
struct Node
{
    int data;
    Node* prev;
    Node* next;
};
```

- 더미 노드 (sentinel node)
    - head node: prev는 NULL / next는 첫 번째 노드
    - trailer node: prev는 마지막 노드 / next는 NULL

```cpp
class DoublyLinkedList
{
    int count;
    Node* header;
    Node* trailer;
};
```


### 3. 이중 연결 리스트 클래스
```cpp
struct Node
{
    int data;
    Node* prev;
    Node* next;
};

class DoublyLinkedList
{
public:
    DoublyLinkedList();
    ~DoublyLinkedList();

    void push_front(int val);
    void push_back(int val);

    void pop_front();
    void pop_back();

    void insert(Node* p, int val);
    void erase(Node* p);

    void print_all(); // 정방향 출력
    void print_reverse(); // 역방향 출력

    bool empty();
    unsigned int size();

private:
    unsigned int count;
    Node* header;
    Node* trailer;
};
```


### 4. 이중 연결 리스트 생성
```cpp
DoublyLinkedList()
{
    count = 0;
    header = new Node {0, NULL, NULL};
    trailer = new Node {0, NULL, NULL};

    header->next = trailer;
    trailer->prev = header;
}
```


### 5. 이중 연결 리스트에 원소 삽입
```cpp
void insert(Node* p, int val)
{
    Node* new_node = new Node {val, p->prev, p};

    new_node->prev->next = new_node;
    new_node->next->prev = new_node;

    count++;
}

void push_front(int val)
{
    insert(header->next, val);
}

void push_back(int val)
{
    insert(trailer, val);
}
```


### 6. 이중 연결 리스트에서 노드 삭제하기
```cpp
void erase(Node* p)
{
    p->next->prev = p->prev;
    p->prev->next = p->next;

    delete p;
    p = nullptr;
    count--;
}

void pop_front()
{
    if (!empty())
        erase(header->next);
}

void pop_back()
{
    if (!empty())
        erase(trailer->prev);
}
```


### 7. 이중 연결 리스트 전체 데이터 출력하기
```cpp
void print_all()
{
    Node* current_node = header->next;

    while (current_node != trailer)
    {
        std::cout << current_node->data << " ";
        current_node = current_node->next;
    }
    std::cout << std::endl;
}

void print_reverse()
{
    Node* current_node = trailer->prev;

    while (current_node != header)
    {
        std::cout << current_node->data << " ";
        current_node = current_node->prev;
    }
    std::cout << std::endl;
}
```


### 8. 이중 연결 리스트 기타 멤버 함수
```cpp
bool empty()
{
    return count == 0;
}

unsigned int size()
{
    return count;
}
```


### 9. 이중 연결 리스트 제거
```cpp
~DoublyLinkedList()
{
    while (!empty())
    {
        pop_front();
    }
    delete header;
    delete trailer;
}
```


### 10. 이중 연결 리스트의 장단점
- 장점 (단일 연결 리스트 대비)
    - 링크가 양방향이므로 양방향 검색이 가능

- 단점 (단일 연결 리스트 대비)
    - 이전 노드 링크를 위한 여분의 공간 사용
    - 데이터의 삽입과 삭제 구현이 좀 더 복잡



---
# S03. 향상된 이중 연결 리스트 클래스


### 1. DoublyLinkedList 클래스에 추가할 기능
- 반복자(iterator) 지원
- 데이터 검색 기능
- 범용 데이터 저장을 위한 클래스 템플릿 작성


### 2. 반복자 지원
- DoublyLinkedList 클래스에 begin()과 end() 멤버 함수 추가
```cpp
iterator begin() const
{
    return iterator(header->next);
}

iterator end() const
{
    return iterator(trailer);
}
```


### 3. 반복자 기반으로 전체 코드 수정
```cpp
#include <iostream>

struct Node
{
    int data;
    Node* prev;
    Node* next;
};

class DoublyLinkedList
{
public:
    class iterator
    {
    private:
        Node* ptr;

    public:
        iterator() : ptr(NULL) {}
        iterator(Node* p) : ptr(p) {}

        int& operator*() { return ptr->data; }

        iterator& operator++() // ++it / 이 강의에서 it++은 생략
        {
            this->ptr = this->ptr->next;
            return *this;
        }

        iterator& operator--()
        {
            ptr = ptr->prev;
            return *this;
        }

        bool operator==(const iterator& it) const
        {
            return this->ptr == it.ptr;
        }

        bool operator!=(const iterator& it) const
        {
            return this->ptr != it.ptr;
        }

        friend class DoublyLinkedList;
    };

public:
    DoublyLinkedList() 
    {
        this->count = 0;
        this->header = new Node {0, NULL, NULL};
        this->trailer = new Node {0, NULL, NULL};

        header->next = trailer;
        trailer->prev = header;
    }

    ~DoublyLinkedList()
    {
        while (!empty())
        {
            pop_front();
        }

        delete header;
        delete trailer;
    }

    void push_front(int val)
    {
        insert(begin(), val);
    }

    void push_back(int val)
    {
        insert(end(), val);
    }

    void pop_front()
    {
        if (!empty())
        {
            erase(begin());
        }
    }

    void pop_back()
    {
        if (!empty())
        {
            erase(--(end()));
        }
    }

    void print_all()
    {
        Node* current_node = header->next;

        while (current_node != trailer)
        {
            std::cout << current_node->data << " ";
            current_node = current_node->next;
        }
        std::cout << std::endl;
    }

    void print_reverse()
    {
        Node* current_node = trailer->prev;

        while (current_node != header)
        {
            std::cout << current_node->data << " ";
            current_node = current_node->prev;
        }
        std::cout << std::endl;
    }

    bool empty()
    {
        return count == 0;
    }

    unsigned int size()
    {
        return count;
    }

    // iterator 관련 메소드
    iterator begin() const
    {
        return iterator(header->next);
    }

    iterator end() const
    {
        return iterator(trailer);
    }

private:
    void insert(const iterator& pos, const int& val)
    {
        Node* p = pos.ptr;
        Node* new_node = new Node {val, p->prev, p};

        new_node->next->prev = new_node;
        new_node->prev->next = new_node;

        count++;
    }

    void erase(const iterator& pos)
    {
        Node* p = pos.ptr;

        p->prev->next = p->next;
        p->next->prev = p->prev;

        delete p;
        p = nullptr;
    
        count--;
    }

private:
    Node* header;
    Node* trailer;
    unsigned int count;
};

int main()
{
    DoublyLinkedList dll;
    dll.push_back(10);
    dll.push_back(20);

    dll.push_front(30);
    dll.push_front(40);

    dll.print_all(); // 40 30 10 20
    dll.print_reverse(); // 20 10 30 40

    dll.pop_back();
    dll.print_all(); // 40 30 10

    dll.pop_front();
    dll.print_all(); // 30 10

    std::cout << dll.size() << std::endl;

    return 0;
}
```


### 4. 데이터 검색 기능
- DoublyLinkedList 클래스에 find() 멤버 함수 추가
```cpp
iterator find(const int val)
{
    Node* curr = header->next;
    while (curr != trailer && curr->data != val)
    {
        curr = curr->next;
    }

    return iterator(curr);
}

// main
    auto it = dll.find(20);
    if (it != dll.end())
        dll.insert(it, 50);
```


### 5. 범용 데이터 저장을 위한 클래스 템프릿 작성
- DoublyLinkedList 클래스를 클래스 템플릿으로 변환하기
    - Node와 DoublyLinkedList 클래스를 모두 typename T를 사용하는 클래스 템플릿으로 변경
    - Node와 DoublyLinkedList 클래스에서 데이터를 표현하는 int를 T로 바꾸고, Node를 Node<T>로 변경
```cpp
template <typename T>
struct Node
{
    T data;
    Node* prev;
    Node* next;
}

template <typename T>
class DoublyLinkedList
{
private:
    unsigned int count;
    Node<T>* header;
    Node<T>* trailer;
}
```


### 6. 전체 소스 코드
```cpp
#include <iostream>

template <typename T>
struct Node
{
    T data;
    Node* prev;
    Node* next;
};

template <typename T>
class DoublyLinkedList
{
public:
    class iterator
    {
    private:
        Node<T>* ptr;

    public:
        iterator() : ptr(NULL) {}
        iterator(Node<T>* p) : ptr(p) {}

        T& operator*() { return ptr->data; }

        iterator& operator++() // ++it / 이 강의에서 it++은 생략
        {
            this->ptr = this->ptr->next;
            return *this;
        }

        iterator& operator--()
        {
            ptr = ptr->prev;
            return *this;
        }

        bool operator==(const iterator& it) const
        {
            return this->ptr == it.ptr;
        }

        bool operator!=(const iterator& it) const
        {
            return this->ptr != it.ptr;
        }

        friend class DoublyLinkedList;
    };

public:
    DoublyLinkedList() 
    {
        this->count = 0;
        this->header = new Node<T> {T(), NULL, NULL};
        this->trailer = new Node<T> {T(), NULL, NULL};

        header->next = trailer;
        trailer->prev = header;
    }

    ~DoublyLinkedList()
    {
        while (!empty())
        {
            pop_front();
        }

        delete header;
        delete trailer;
    }

    void push_front(const T& val)
    {
        insert(begin(), val);
    }

    void push_back(const T& val)
    {
        insert(end(), val);
    }

    void pop_front()
    {
        if (!empty())
        {
            erase(begin());
        }
    }

    void pop_back()
    {
        if (!empty())
        {
            erase(--(end()));
        }
    }

    void print_all()
    {
        Node<T>* current_node = header->next;

        while (current_node != trailer)
        {
            std::cout << current_node->data << " ";
            current_node = current_node->next;
        }
        std::cout << std::endl;
    }

    void print_reverse()
    {
        Node<T>* current_node = trailer->prev;

        while (current_node != header)
        {
            std::cout << current_node->data << " ";
            current_node = current_node->prev;
        }
        std::cout << std::endl;
    }

    bool empty()
    {
        return count == 0;
    }

    unsigned int size()
    {
        return count;
    }

    // iterator 관련 메소드
    iterator begin() const
    {
        return iterator(header->next);
    }

    iterator end() const
    {
        return iterator(trailer);
    }

    iterator find(const T& val)
    {
        Node<T>* curr = header->next;

        while (curr != trailer && curr->data != val)
        {
            curr = curr->next;
        }

        return iterator(curr);
    }

    void insert(const iterator& pos, const T& val)
    {
        Node<T>* p = pos.ptr;
        Node<T>* new_node = new Node<T> {val, p->prev, p};

        new_node->next->prev = new_node;
        new_node->prev->next = new_node;

        count++;
    }

    void erase(const iterator& pos)
    {
        Node<T>* p = pos.ptr;

        p->prev->next = p->next;
        p->next->prev = p->prev;

        delete p;
        p = nullptr;
    
        count--;
    }

private:
    Node<T>* header;
    Node<T>* trailer;
    unsigned int count;
};

int main()
{
    DoublyLinkedList<int> dll;
    dll.push_back(10);
    dll.push_back(20);

    dll.push_front(30);
    dll.push_front(40);

    // dll.print_all(); // 40 30 10 20
    // dll.print_reverse(); // 20 10 30 40

    auto it = dll.find(20);
    if (it != dll.end())
        dll.insert(it, 50);

    for (auto n : dll) // begin / end / ++ / != / * 로직이 있으면 됨
    {
        std::cout << n << " "; // 40 30 10 50 20
    }
    std::cout << std::endl;

    return 0;
}
```



---
# S04. std::list와 std::forward_list


### 1. std::list
- 이중 연결 리스트를 구현한 컨테이너
```cpp
template <class T, class Allocator = std::allocator<T>>
class list;
```

- 어느 위치라도 원소의 삽입 또는 삭제를 상수 시간으로 처리: O(1)
- 그러나 특정 위치에 곧바로 접근할 수 없음
    - std::vector처럼 []연산자를 이용한 랜덤 엑세스는 지원 안 함
    - begin(), end() 등의 함수로 얻은 반복자와 ++, -- 연산자로 위치 이동
    - std::vector가 좀 더 효율적으로 동작하는 경우가 많고 실제로도 많이 사용함
- <list>에 정의되어있음

- 반복자 무효화의 안정성: 삽입하거나 삭제가 일어나도 다른 원소를 가리키는 반복자나 참조자가 무효화되지 않음
- 메모리 오버헤드 발생: 앞/뒤 포인터 (최소 16바이트 오버헤드 발생)


### 2. std::list의 주요 연산
- front(), back()
- begin(), cbegin(), rbegin(), crbegin(), end(), cend(), rend(), crend()
- insert(), push_front(), push_back(), emplace(), emplace_front(), emplace_back()
- clear(), erase(), pop_front(), pop_back()
- splice(): 다른 리스트의 원소를 현재 리스트로 이동 / 현재 리스트 원소 증가 / 다른 리스트 원소 감소
- remove(), remove_if(): 특정 값 / 조건 삭제
- reverse(): 원소의 순서를 역순으로 변경
- unique(): 중복 원소를 삭제 - **연속적일 경우에만** / 1 2 1 2 X / 1 1 2 2 O
- sort(): 정렬 / std::sort 전역 함수로 정렬할 수 없으므로 따로 지원해줌
- ...


### 3. std::list 예제 코드
```cpp
#include <iostream>
#include <list>

using std::cout;
using std::endl;
using std::list;

int main()
{
    list<int> l1;
    l1.push_back(10); // 10
    l1.push_front(20); // 20 10

    list<int> l2 {10, 20, 30, 40};

    for (auto a : l2)
    {
        cout << a << " ";
    }
    cout << endl;

    //
    l2.splice(l2.end(), l1); // l2 맨마지막 위치에 l1을 전부 다 붙여라 / l1은 원소가 없어짐
    cout << "l1" << endl;
    for (auto a : l1)
    {
        cout << a << " ";
    }
    cout << endl;

    cout << "l2" << endl;
    for (auto a : l2)
    {
        cout << a << " ";
    }
    cout << endl;

    //
    l2.sort();
    cout << "l2 sort" << endl;
    for (auto a : l2)
    {
        cout << a << " ";
    }
    cout << endl;

    //
    l2.unique();
    cout << "l2 unique" << endl;
    for (auto a : l2)
    {
        cout << a << " ";
    }
    cout << endl;

    return 0;
}
```


### 4. std::forward_list
- 단순 연결 리스트를 구현한 컨테이너
```cpp
template <class T, class Allocator = std::allocator<T>>
class forward_list;
```

- begin() 함수로 (순방향) 반복자를 얻을 수 있고, 오직 ++연산만 사용 가능
- std::list보다 빠르게 동작하고, 적은 메모리를 사용
- C++11부터 지원
- <forward_list>에 정의되어있음
- 양방향을 굳이 사용하지 않을 것이면 forward_list 사용을 권장


### 5. std::forward_list와 std::list 차이점
- 맨 앞 원소만 참조할 수 있음
- size() 함수를 지원해주지 않음 - 최소한의 오버헤드를 지향하기 때문!!
- std::distance() 함수를 사용해서 원소 개수를 알 수 있음


### 6. std::forward_list 예제 코드
```cpp
#include <iostream>
#include <forward_list>

using std::cout;
using std::endl;
using std::forward_list;

int main()
{
    forward_list<int> l1 {10, 20, 30, 40};

    l1.push_front(10);
    l1.push_front(20);
    l1.push_front(30); // 30 20 10 10 20 30 40

    for (const auto& a : l1)
    {
        cout << a << " ";
    }
    cout << endl;

    //
    l1.remove(40);
    for (const auto& a : l1)
    {
        cout << a << " ";
    }
    cout << endl;

    //
    l1.remove_if([](int n) { return n > 20; });
    for (const auto& a : l1)
    {
        cout << a << " ";
    }
    cout << endl;


    return 0;
}
```


### 7. 추가 학습
- vector vs list
    - 캐시 지역성
        - vector는 메모리가 연속적이라 캐시 효율이 매우 높음
        - list는 캐시 미스가 빈번하게 발생하고, 이 때문에 O(1)이지만 실성능이 vector보다 느린 경우가 있음

- sort
    - std::sort는 임의 접근 반복자를 요구함 (vector, deque)
    - list의 sort는 합병정렬 기반으로 구성 및 Stable Sort 수행

- forward_list의 before_begin(): 맨 앞 노드보다도 이전 노드(포인터)
    ```cpp
    auto it = l1.before_begin();
    l1.insert_after(it, 50); // 맨 앞에 50 삽입
    ```
    - 이거를 굳이 왜 사용하냐고 하면은
    - insert_after은 해당 노드의 뒤에 넣는 것인데 만약 맨 앞에 추가해야되는 상황이 있다면
    - 이 때 조건부로 push_front를 사용하면 되지만
    - 코드의 일관성을 위해서 사용

- splice의 핵심
    - **복사하거나 이동이 아닌 노드의 포인터 연결만 바꿈**
    - O(1) / 게임 서버에서 대기열 관리나 오브젝트 풀링할 때 매우 유용함

- std::distance() 비용
    - std::distance(l1.begin(), l1.end()) => O(N)
    - vector / DoublyLinkedList의 size는 O(1)
