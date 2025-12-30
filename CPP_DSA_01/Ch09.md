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
