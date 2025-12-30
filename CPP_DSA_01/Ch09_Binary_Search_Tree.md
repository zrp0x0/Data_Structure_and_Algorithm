# S01. 이진 탐색 트리 01


### 1. 이진 탐색 트리(BST: binary search tree)
- 효율적인 자료의 탐색을 위한 이진 트리 기반의 자료 구조
    - 자료의 계층적인 구조를 표현하기 위해 트리를 사용하는 것이 아닐, 자료의 효율적인 관리를 위해 트리 구조를 사용 
    - 효율적인 관리
        - 데이터 삽입/삭제

- 모든 노드 N에 대해서,
- 왼쪽 서브 트리에 있는 모든 노드의 키값은 노드 N의 키 값보다 작고, 
- 오른쪽 서브 트리에 있는 모든 노드의 키값은 노드 N의 키 값보다 큼
    - **중복되는 값이 없다는 것을 암시 (매우 중요!!)**


### 2. 이진 탐색 트리의 특징
- 자료의 탐색, 삽입, 삭제가 모두 O(log N) 시간 복잡도로 동작
- 이진 탐색 트리를 **중위 순회하면 오름차순으로 정렬된 값을 얻을 수 있음**

- 자료구조 | 탐색(인덱스 접근이 아님) | 삽입 | 삭제
- 배열 | O(N) | O(1) | O(1) => 배열의 순서가 중요하지 않으면 중간에 삽입하고 그 자리에 있던 걸 뒤로 보내면 됨 / 삭제 또한 중간 삭제를 하고 맨 마지막 원소를 그 빈 공간에 넣으면 됨 
- 연결리스트 | O(N) | O(1) | O(1)
- 정렬된 배열 | O(log N) | O(N) | O(N)
- 이진 탐색 트리 | O(log N) | O(log N) | O(log N)
    - 경우에 따라서는 이진 탐색 트리가 O(N)으로 동작할 수도 있긴함 / 평균적으로는 O(log N)임


### 3. 이진 탐색 트리에서 탐색
  10
 5  14
4 7   18
 6   16 20
- find(7)
    - 10 > 7 왼쪽 서브 트리로 이동
    - 5 < 7 오른쪽 서브 트리로 이동
    - 7 == 7
    - true 혹은 nullptr 반환

- find(15)
    - 10 < 15 오른쪽 서브 트리로 이동
    - 14 < 15 오른쪽 서브 트리로 이동
    - 18 > 15 왼쪽 서브 트리로 이동
    - 16 > 15 왼쪽 서브 트리가 없음
    - false 혹은 nullptr 반환


### 4. 이진 탐색 트리에서 삽입
  10
 5  14
4 7   18
 6   16 20
- insert(9)
    - 10 > 9 왼쪽 서브 트리로 이동
    - 5 < 9 오른쪽 서브 트리로 이동
    - 7 < 9 오른쪽 서브 트리로 이동
    - 7의 오른쪽 자식으로 삽입

- insert(12)
    - 10 < 12 오른쪽 서브 트리로 이동
    - 14 > 12 왼쪽 서브 트리로 이동
    - 14의 왼쪽 자식으로 삽입


### 5. 이진 탐색 트리 구현하기 - 클래스 설계
```cpp
struct Node
{
    int data;
    Node* left;
    Node* right;
    
    Node(int d) : data(d), left(nullptr), right(nullptr) {}
}

class BinarySearchTree
{
private:
    Node* root = nullptr;

public:
    ~BinarySearchTree();
    void insert(int value);
    Node* find(int value);
    void inorder();
    ...

private:
    void insert_impl(Node* curr, int value);
    Node* find_impl(Node* curr, int value);
    void inorder_impl(Node* curr);
    ...
}
```


### 6. 이진 탐색 트리 구현하기 - 삽입 함수
```cpp
class BinarySearchTree
{
private: 
    Node* root = nullptr;

public:
    void insert(int value)
    {
        if (!root)
            root = new Node(value); // root 노드 생성
        else
            insert_impl(root, value);
    }

private:
    void insert_impl(Node* curr, int value)
    {
        if (value < curr->data)
        {
            if (!curr->left)
                curr->left = new Node(value);
            else    
                insert_impl(curr->left, value);
        }
        else
        {
            if (!curr->right)
                curr->right = new Node(value);
            else
                insert_impl(curr->right, value);
        }
    }
}
```


### 7. 이진 탐색 트리 구현하기 - 탐색 함수
```cpp
class BinarySearchTree
{
private:
    Node* root = nullptr;

public:
    Node* find(int value)
    {
        return find_impl(root, value);
    }

private:
    Node* find_impl(Node* curr, int value)
    {
        if (curr == nullptr)
            return nullptr;

        if (value == curr->data)
            return curr;
        else
        {
            if (value < curr->data)
                return find_impl(curr->left, value);
            else
                return find_impl(curr->right, value);
        }
    }
}
```


### 8. 이진 탐색 트리 구현 클래스 테스트 코드
```cpp
#include <iostream>

using std::cout;
using std::endl;

struct Node
{
    int data;
    Node* left;
    Node* right;

    Node(int data) : data(data), left(nullptr), right(nullptr) {}
};

class BinarySearchTree
{
private:
    Node* root = nullptr;

public:
    ~BinarySearchTree()
    {
        delete_node(root);
    }

    void insert(int value)
    {
        if (!root)
            root = new Node(value);
        else
            insert_impl(root, value);
    }

    Node* find(int value)
    {
        return find_impl(root, value);
    }

    void inorder()
    {
        inorder_impl(root);
    }

private:
    void insert_impl(Node* curr, int value)
    {
        if (value < curr->data)
        {
            if (!curr->left)
                curr->left = new Node(value);
            else
                insert_impl(curr->left, value);
        }
        else
        {
            if (!curr->right)
                curr->right = new Node(value);
            else
                insert_impl(curr->right, value);
        }
    }   

    Node* find_impl(Node* curr, int value)
    {
        if (curr == nullptr)
            return nullptr;

        if (value == curr->data)
            return curr;
        else
        {
            if (value < curr->data)
                return find_impl(curr->left, value);
            else
                return find_impl(curr->right, value);
        }
    }

    void inorder_impl(Node* curr)
    {
        if (curr)
        {
            inorder_impl(curr->left);
            cout << curr->data << " ";
            inorder_impl(curr->right);
        }
    }

    void delete_node(Node* node)
    {
        if (node)
        {
            delete_node(node->left);
            delete_node(node->right);
            cout << node->data << " Delete" << endl;
            delete node;
        }
    }
};

int main()
{
    BinarySearchTree bst;

    bst.insert(10);
    bst.insert(20);
    bst.insert(5);
    bst.insert(7);
    bst.insert(8);

    if (bst.find(8))
        cout << bst.find(8)->data << endl;
    else
        cout << "nullptr" << endl;

    if (bst.find(999))
        cout << bst.find(999) << endl;
    else
        cout << "nullptr" << endl;

    bst.inorder();
    cout << endl;

    return 0;
}
```


### 9. 추가 내용
- 탐색 재귀말고 반복문 사용
    - 단순히 아래로 내려가기만 하므로 반복문으로 바꾸는 것이 훨씬 효율적임
    ```cpp
    while(curr != nullptr)
    {
        if (value == curr->data) 
            return curr;
        
        if (value < curr->data)
            curr = curr->left;
        else
            curr = curr->right;
    }
    return nullptr;
    ```

- BST가 연결리스트보다 항상 빠른가?
    - 아니오!!
    - 트리가 편향되면 성능이 최악의 경우 O(N)!!
    - 이를 해결하기위해서 Balanced BST가 등장함



---
# S02. 이진 탐색 트리 02


### 1. 이진 탐색 트리에서 삭제
- 이진 탐색 트리에서 자료의 삭제는 해당 노드를 살제한 후, BST 속성을 만족할 수 있는 다른 적절한 노드를 찾아 해당 노드로 대체해야함
    - 자식 노드가 없는 경우: 해당 노드를 삭제하고 링크를 제거
    - 자식 노드가 하나만 있는 경우: 해당 노드를 삭제하고, 부모 노드의 포인터가 해당 노드의 자식 노드를 가리키도록 설정
    - 자식 노드가 두 개 있는 경우: 후속 노드의 값을 현재 노드로 복사하고, 후속 노드를 삭제
        - 후속 노드(successor): 현재 노드 다음으로 큰 숫자를 가진 노드(현재 노드의 오른쪽 서브 트리에서 가장 작은 값의 노드)
            - 후속 노드는 자식이 없거나 또는 오른쪽 자식만 있음
                - 1. 그냥 삭제 후 올리거나
                - 2. 오른쪽 자식 노드가 있을 경우 그 또한 후속 노드의 위치로 이동해주어야함
        - predecessor: 현재 노드 다음으로 작은 숫자를 가진 노드 (현재 노드의 왼쪽 서브 트리에서 가장 큰 값의 노드)


### 2. 이진 탐색 트리 구현하기 - 삭제 함수
```cpp
class BinarySearchTree
{
private:
    Node* root = nullptr;

public:
    void erase(int value)
    {
        root = erase_impl(root, value);
    }

private:
    // 노드 삭제 후, 부모 노드 포인터가 가리켜야할 노드의 주소를 반환해야 함
    Node* erase_impl(Node* node, int value)
    {
        if (!node)
            return nullptr;

        if (value < node->data)
            node->left = erase_impl(node->left, value);
        else if (value > node->data)
            node->right = erase_impl(node->right, value);
        else // value == node->data
        {
            if (node->left && node->right) 
            {
                // 자식 노드가 둘 다 있는 경우
                auto succ = successor(node);
                node->data = succ->data;
                node->right = erase_impl(node->right, succ->data);
            }
            else
            {
                // 자식 노드가 전혀 없거나 한쪽 자식만 있는 경우
                auto tmp = node->left ? node->left : node->right;

                delete node;
                return tmp;
            }
        }
        
        return node;
    }
}
```


### 3. 이진 탐색 트리 구현하기 - 후속 노드 찾기
```cpp
class BinarySearchTree
{
private:
    Node* successor(Node* node)
    {
        auto curr = node->right;
        while (curr && curr->left)
            curr = curr->left;
        return curr;
    }
}
```


### 4. 이진 탐색 트리 구현 클래스 테스트 코드 - 삭제 함수 추가 버전
```cpp
#include <iostream>

using std::cout;
using std::endl;

struct Node
{
    int data;
    Node* left;
    Node* right;

    Node(int data) : data(data), left(nullptr), right(nullptr) {}
};

class BinarySearchTree
{
private:
    Node* root = nullptr;

public:
    ~BinarySearchTree()
    {
        delete_node(root);
    }

    void insert(int value)
    {
        if (!root)
            root = new Node(value);
        else
            insert_impl(root, value);
    }

    Node* find(int value)
    {
        return find_impl(root, value);
    }

    // 삭제
    void erase(int value)
    {
        root = erase_impl(root, value);
    }

    void inorder()
    {
        inorder_impl(root);
    }

private:
    void insert_impl(Node* curr, int value)
    {
        if (value < curr->data)
        {
            if (!curr->left)
                curr->left = new Node(value);
            else
                insert_impl(curr->left, value);
        }
        else
        {
            if (!curr->right)
                curr->right = new Node(value);
            else
                insert_impl(curr->right, value);
        }
    }   

    Node* find_impl(Node* curr, int value)
    {
        if (curr == nullptr)
            return nullptr;

        if (value == curr->data)
            return curr;
        else
        {
            if (value < curr->data)
                return find_impl(curr->left, value);
            else
                return find_impl(curr->right, value);
        }
    }

    // 삭제
    Node* erase_impl(Node* node, int value)
    {
        /*
            10
           5  14
             12 16
               15
              
        */

        if (!node)
            return nullptr;

        if (value < node->data)
            node->left = erase_impl(node->left, value);
        else if (value > node->data)
            node->right = erase_impl(node->right, value);
        else
        {
            if (node->left && node->right)
            {
                // 자식 노드가 둘 다 있는 경우
                auto succ = successor(node);
                node->data = succ->data;
                node->right = erase_impl(node->right, succ->data);
            }
            else
            {   
                auto tmp = node->left ? node->left : node->right;

                delete node;
                return tmp;
            }
        }

        return node;
    }

    // 후속 노드 찾기 이해 완
    Node* successor(Node* node)
    {
        auto curr = node->right;
        while (curr && curr->left)
        {
            curr = curr->left;
        }
        return curr;
    }

    void inorder_impl(Node* curr)
    {
        if (curr)
        {
            inorder_impl(curr->left);
            cout << curr->data << " ";
            inorder_impl(curr->right);
        }
    }

    void delete_node(Node* node)
    {
        if (node)
        {
            delete_node(node->left);
            delete_node(node->right);
            cout << node->data << " Delete" << endl;
            delete node;
        }
    }
};

int main()
{
    BinarySearchTree bst;

    bst.insert(10);
    bst.insert(20);
    bst.insert(5);
    bst.insert(7);
    bst.insert(8);

    /*
     10
    5  20
     7
      8
    */

    if (bst.find(8))
        cout << bst.find(8)->data << endl;
    else
        cout << "nullptr" << endl;

    if (bst.find(999))
        cout << bst.find(999) << endl;
    else
        cout << "nullptr" << endl;

    bst.inorder();
    cout << endl;

    return 0;
}
```


### 5. 이진 탐색 트리의 문제점
- 원소의 사입 순서에 따라 이진 탐색 트리가 한쪽으로 치우친 형태로 구성될 수 있음
- 트리가 한쪽으로 치우칠 경우 트리의 높이가 h = n - 1 형태로 구성되므로 탐색, 삽입, 삭제 연산의 시간 복잡도가 O(N)으로 결정됨

- 5 3 7 2 4 9: h = 2 / n = 6
- 2 3 4 5 7 9: 한쪽으로 치우쳐진 형태: h = 5 / n = 6


### 6. 이진 탐색 트리의 문제점 해결 방법
- 한쪽으로 치우친 트리의 구성을 변경하여 균형 잡힌 트리 형태로 변경할 수 있음
    - **트리 회전**

- 균형 잡힌 트리의 예
    - AVL 트리, 레드 블랙 트리, B 트리, 스플레이트 트리 등 (본 강의에서 다루지는 않음)


### 7. 추가 내용
- Successor vs Predecessor
    - 삭제 시 반드시 후속 노드를 사용해야하는 것은 아님
    - 트리의 균형을 맞추기 위해 두 방식을 번갈아 가면서 사용하기도 함



---
# S03. std::set과 std::map


### 1. C++ STL 컨테이너
- 순차 컨테이너(sequence containers)
    - vector, array, deque, list, forward_list

- 연관 컨테이너(associative containers)
    - set, multiset, map, multimap

- 순서 없는 연관 컨테이너(unordered associative containers) (hash라는 기법을 사용함)
    - unordered_set, unordered_multiset, unordered_map, unordered_multimap

- 컨테이너 어댑터(container adaptors)
    - stack, queue, priority_queue


### 2. set과 map 차이
- set은 키만 저장
- map은 키와 값을 저장


### 3. set과 multiset 차이
- set은 고유한 키의 데이터만 저장(중복 허용 안함)
- multiset은 중복되는 데이터를 저장


### 4. set과 unordered_set 차이
- set은 내부에서 데이터를 정렬해서 저장
- unordered_set은 데이터를 정렬하지 않음


### 5. std::set
```cpp
template<class Key, class Compare = std::less<Key>, class Allocator = std::allocator<Key>>
class set;
```

- Key 타입의 키 값을 저장하는 연관 컨테이너
- 저장된 데이터는 키 값을 기준으로 정렬됨(오름차순)
- 데이터 삽입, 삭제, 탐색은 O(log N) 시간 복잡도로 동작
- std::set은 보통 레드 블랙 트리를 이용하여 구현됨
- 만약 중복되는 데이터를 set 구조로 저장하려면 std::multiset 컨테이너를 사용
- 만약 데이터를 정렬되지 않은 상태로 정장하려면 std::unordered_set 컨테이너를 사용
- 사용자 정의 타입을 저장할 경우, 비교 연산을 지원해야함
- <set>에 정의되어 있음


### 6. std::set 주요 멤버 함수와 설명
- begin(), end(), rbegin(), rend(): 순방향 및 역방향 반복자 반환
- insert(): 중복되지 않는 새로운 원소를 삽입(이미 있는 경우 함수가 그대로 반환됨) / emplace()
- erase(): 특정 원소를 삭제
- find(): 특정 키 값을 갖는 원소를 찾아 반복자를 반환. 원소를 끝까지 찾지 못하게 end()에 해당하는 반복자를 반환. 원소를 찾지 못하며 맨 마지막 원소 다음 반복자를 반환함
    - find() == end(): 해당 원소가 없다는 의미
    - C++ 20: contains()

- clear(): 모든 원소를 삭제
- size(): 원소의 개수를 반환
- empty(): set이 비어 있으면 true를 반환


### 7. std::set 사용 예제
```cpp
#include <iostream>
#include <string>
#include <set>

using namespace std;

int main()
{
    set<int> odds;
    odds.insert(1);
    odds.insert(7);
    odds.insert(5);
    odds.insert(3);
    odds.insert(9);
    // 내부적으로 오름 차순 형태로 정렬된 상태로 저장됨

    for (auto n : odds)
    {
        cout << n << " ";
    }
    cout << endl;

    // set<int> odds2 {1, 7, 5, 3, 9};

    set<int, greater<>> evens {2, 4, 6, 8}; // 내림차순 정렬: greater<>
    evens.insert(10);
    evens.insert(2); // 그냥 무시됨

    if (evens.find(5) != evens.end())
    {
        cout << "4 is found" << endl;
    }
    else
    {
        cout << "4 is not found" << endl;
    }


    for (auto n : evens)
    {
        cout << n << " ";
    }
    cout << endl;

    using psi = pair<string, int>;

    // struct psi
    // {
    //     string first;
    //     int second;
    // }; // 이헐게 하면 크기 비교를 할 수 없음 / 부등호 연산자 오버로딩 함수를 따로 작성해주어야함!!
    
    // 복합적인 자료형 저장
    // set<pair<string, int>> managers {{"Amelia", 29}, {"Noah", 25}, {"Olivia", 31}};
    set<psi> managers {{"Amelia", 29}, {"Noah", 25}, {"Olivia", 31}};
    managers.insert({"George", 35});
    // managers.insert(make_pair("George", 35));

    psi person1 {"Noah", 25};
    if (managers.find(person1) != managers.end())
    {
        cout << "Found" << endl;
    }
    else
    {
        cout << "Not Found" << endl;
    }

    managers.erase({"Sophia", 40});
    psi person2 {"Noah", 25};
    managers.erase(person2);

    for (const auto& m : managers)
    {
        cout << m.first << "(" << m.second << ")" << endl;
    }
    cout << endl;

    return 0;
}
```


### 8. std::map
```cpp
template<class Key, class T, class Compare = std::less<Key>>, class Allocator = std::allocator<std::pair<const Key, T>>>
class map;
```

- Key 타입의 키(key)와 T 타입의 값(value)의 쌍(pair)을 저장하는 연관 컨테이너
- 저장된 데이터는 키 값을 기준으로 정렬됨
- 데이터 삽입, 삭제, 탐색은 O(log N) 시간 복잡도로 동작
- std::map은 보통 레드 블랙 트리를 이용하여 구현됨
- 만약 중복되는 데이터를 map 구조로 저장하려면 std::multimap 컨테이너를 사용
- 만약 데이터를 정렬되지 않은 상태로 저장하려면 std::unordered_map 컨테이너를 사용
- **사용자 정의 타입을 저장할 경우, 비교 연산을 지원해야함**
- <map>에 정의되어 있음


### 9. std::map 주요 멤버 함수와 설명
- begin(), end(), rbegin(), rend(): 순방향 및 역방향 반복자 반환
- insert(): (중복되지 않는) 새로운 원소를 삽입 / emplace()
- erase(): 특정 원소를 삭제
- operator[]: 특정 키에 해당하는 원소의 값을 **참조**로 반환. 해당 키의 원소가 없으면 새로운 원소를 기본값으로 생성하여 참조를 반환 / at()
    - [Key] = value
    - 원소 삽입으로도 사용. 해당 키가 없으면 기본 값으로 생성해서 **그 참조를 반환함**

- find(): 특정 키 값을 갖는 원소를 찾아 반복자를 반환. 원소를 끝까지 찾지 못하면 end()에 해당하는 반복자를 반환 / contains()
- clear(): 모든 원소를 삭제
- size(): 원소의 개수를 반환
- empty(): map이 비어 있으면 true를 반환


### 10. std::map 주요 예제
```cpp
#include <iostream>
#include <string>
#include <map>

using namespace std;

int main()
{
    map<string, int> fruits;
    fruits.insert({"Apple", 1000});
    fruits.insert(make_pair("Banana", 1500));
    cout << fruits["Apple"] << endl; // Key에 해당하는 Value로 치환됨
    fruits["orange"]; // orange라는 Key가 생성되고 기본 생성자로 초기화됨
    fruits["orange"] = 3000; // 변경도 가능

    for (const auto& p : fruits)
    {
        cout << p.first << " is " << p.second << " won." << endl;
    }

    if (fruits.find("Apple") != fruits.end())
    {
        cout << "Found" << endl;
    }
    else
    {
        cout << "Not Found" << endl;
    }

    if (fruits.find("Appl") != fruits.end())
    {
        cout << "Found" << endl;
    }
    else
    {
        cout << "Not Found" << endl;
    }

    fruits.erase("Apple");
    for (const auto& p : fruits)
    {
        cout << p.first << " is " << p.second << " won." << endl;
    }

    for (auto [name, price] : fruits) // auto 및 구조적 바인딩 C++ 17부터 지원
    {
        cout << name << " is " << price << "won." << endl; 
    }

    return 0;
}   
```


### 11. 추가 내용
- std::map의 operator[]의 부수 효과
    - 존재하지 않는 키를 조회만 해도, 기본값으로 원소를 생성해버림
    - .at(): 키가 없으면 예외를 던짐
    - .find(): 키가 없으면 end() 반복자 반환

- insert()은 std::pair<iterator, bool>을 반환함
    - auto[it, success] = odds.insert(5);
    - if (!success) cout << "이미 존재하는 값입니다." << endl;
        - first: 삽입된 원소(또는 이미 있던 원소)를 가리키는 반복자
        - second: 삽입 성공 여부 bool

- std::map vs std::unordered_map
    - 내부 구조: 레드블랙트리 vs 해시테이블
    - 시간 복잡도: O(log N) vs 평균 O(1) 최악 O(N)
    - 사용: 데이터 정렬이 필요할 때 vs 정렬보다 빠른 탐색이 중요할 때

- 실무적 조언
    - map에서는 operator[]보다 find()를 쓰는 습관을 기르자!!
    - size() 함수는 C++이후부터 O(1) 안심하고 사용해도 됨!!

- set/map/multi_... vs unordered_set/map/multi_...
    - 전자는 레드블랙트리
    - 후자는 해시테이블
