# S01. 트리와 이진 트리


### 1. 트리(tree)
- **계층적인 구조**를 나타내는 자료 구조
- 보통 뿌리(root)가 위에, 나뭇가지(branch)와 잎(leaf)이 아래로 자라는 형태로 표현
- 트리 사용 예: 회사 조직도, 대학 교과 과정의 수강 트리, 파일 시스템 디렉터리 구조


### 2. 트리의 구성 요소
- 노드(node, vertex)
    - 트리의 데이터를 저장하는 원소 단위. 정점

- 엣지(edge, link)
    - 노드와 노드를 연결하는 선. 간선
    - 트리의 엣지는 부모-자식 계층 관계만을 나타냄
    - 노드의 개수가 N이면 항상 N-1개의 엣지가 존재


### 3. 트리 용어
- 루트(root): 최상위 노드. 단 한 개만 존재
- 부모(parent)-자식(child): 해당 노드의 바로 위의 노드(부모) / 바로 아래의 노드(자식)
- 리프(leaf): 자식 노드가 없는 노드
    - 단말(terminal)
    - 외부 노드(external node)
- 내부 노드(internal node): 자식이 하나라도 있는 노드
- 형제(sibling): 부모가 같은 노드들
- 조상(ancestor)-후손(descendant): 부모, 부모의 부모, 부모의 부모의 부모 등을 통틀어서 조상이라고 표현 / 후손은 자식들
- 서브트리(subtree): 부트리, 부분트리, 특정 노드를 루트 노드로 잡고 그 하위 노드들을 모아놓은 트리

- 차수(degree): 자식 노드의 개수
    - 노드의 차수: 특정 노드가 가진 자식 노드의 수
    - 트리의 차수: 해당 트리 내에 있는 노드들의 차수 중 최대값 / 이진 트리의 차수는 항상 2이하

- 레벨(level): 루트부터 아래로 0, 1, 2, ...
- 높이(height): 특정 노드에서 가장 멀리 있는 리프 노드까지 가는 엣지의 수 / 리프 노드의 높이는 0
- 깊이(depth): 루트 노드에서 특정 노드에 도달하기 위해 거치는 엣지의 수 / 루트 노드의 깊이는 0


### 4. 이진 트리(binary tree)
- 모든 노드가 최대 두 개의 자식을 갖는 트리
    - 모든 노드의 차수가 2 이하인 트리
    - 모든 노드가 2개의 서브트리를 가지고 있는 트리(서브트리는 빈 트리(empty tree)일 수도 있음)
- 각각의 자식 노드의 부모의 왼쪽 자식(left child)또는 오른쪽 자식(right child)으로 지정됨


### 5. 이진 트리 용어
- 정 이진 트리(proper binary tree, full binary tree)
    - 모든 노드가 0개 또는 2개의 자식 노드를 갖는 이진 트리(즉, 리프 노드를 제외한 노드들은 모두 자식이 2개임)

- 포화 이진 트리(perfect binary tree)
    - 트리의 모든 레벨에서 노드가 모두 채워져 이진 트리
    - 높이가 h이면 노드 개수가 2^(h+1) - 1  

- 완전 이진 트리(complete binary tree)
    - 마지막 레벨을 제외한 모든 레벨에는 노드가 완전히 채워져있고, 마지막 레벨에는 노드가 왼쪽부터 채워져있는 이진 트리
    - 힙 자료 구조의 기초가 됨

- 균형 이진 트리(balanced binary tree)
    - 모든 노드의 서브 트리 간의 높이 차이가 1 
    - AVL 트리, 레드-블랙 트리
    
- 편향 트리(skewed tree)
    - 리프 노드를 제외한 모든 노드가 하나의 자식 노드만 갖는 트리
    - 여러 가지 알고리즘을 사용할 경우, 효율이 좋지 않아 균형이 있는 이진 트리를 사용하는 것이 일반적임


### 5. 이진 트리 구현 방법
- 배열
    - 완전 이진 트리의 경우, 배열을 이용하여 효율적으로 표현 가능 -> 10장 힙(우선 순위 큐)에서 설명

- 연결 리스트
    - 저장할 데이터와 왼쪽 자식과 오른쪽 자식 노드를 가리키는 포인터 2개를 갖는 구조체를 사용
    - Node [[data] [left] [right]]
    ```cpp
    struct Node
    {
        char data;
        Node* left;
        Node* right;

        Node(char d) : data(d), left(nullptr), right(nullptr) {}
    }
    ```

    ```cpp
    int main()
    {
        Node* root = new Node('A');

        root->left = new Node('B');
        root->right = new Node('C');

        root->left->left = new Node('D');
        root->left->right = new Node('E');

        root->right->right = new Node('F');

        // The tree is not deleted!
        // - 다음 시간에 배울 것임
    }
    ```


### 6. 추가 내용
- delete nullptr은 C++에서 안전하게 허용됨

- 이진 트리의 수학적 성질
    - 레벨 i에서의 최대 노드 수: 2^i / 전체 노드 개수가 아님 주의!!
    - 노드가 n개인 이진 트리의 엣지 수는 n-1
    - 포화 이진 트리의 높이가 h일 때 리프 노드의 수는 2^h
    - **리프 노드의 수(n0)와 자식이 2개인 노드의 수(n2)의 관계는 n0 = n2 + 1**



---
# S02. 이진 트리의 순회


### 1. 트리 순회(traversal)
- 정해진 순서에 의해 트리의 모든 노드를 한 번씩 방문하는 작업


### 2. 전형적인 트리 순회 방법
- 전위 순회(preorder traversal)
- 중위 순회(inorder traversal)
- 후위 순회(postorder traversal)
- 레벨 순서 순회(level order traversal)

- 전위 / 중위 / 후위: 깊이 우선 탐색(depth first traversal)
- 레벨 순서: 너비 우선 탐색(breadth first traversal)


### 3. 전위 순회(preorder traversal)
- 1. 현재 노드(상위 노드)
- 2. 왼쪽 **서브 트리**
- 3. 오른쪽 **서브 트리**
- 위 과정 반복

```cpp
void preorder(Node* node)
{
    if (node) // 기저 조건: node == nullptr;
    {
        std::cout << node->data << ", ";
        preorder(node->left);
        preorder(node->right);
    }
}
```


### 4. 중위 순회(inorder traversal)
- 1. 왼쪽 **서브 트리**
- 2. 현재 노드(상위 노드)
- 3. 오른쪽 **서브 트리**
- 위 과정 반복

```cpp
void inorder(Node* node)
{
    if (node) // 기저 조건: node == nullptr
    {
        inorder(node->left);
        std::cout << node->data << ", ";
        inorder(node->right);
    }
}
```


### 5. 후위 순회(postorder traversal)
- 1. 왼쪽 **서브 트리**
- 2. 오른쪽 **서브 트리**
- 3. 현재 노드(상위 노드)
- 위 과정 반복

```cpp
void postorder(Node* node)
{
    if (node)
    {
        postorder(node->left);
        postorder(node->right);
        std::cout << node->data << ", ";
    }
}
```


### 6. 레벨 순서 순회(level order traversal)
- 낮은 레벨에 있는 노드를 모두 방문한 후, 큰 레벨로 이동하여 방문을 반복
- **큐를 사용하여 구현**

```cpp
void levelorder(Node* node)
{
    std::queue<Node*> q;
    q.push(node);

    while (!q.empty())
    {
        auto curr = q.front();
        q.pop();

        std::cout << curr->data << ", ";
        if (curr->left)
            q.push(curr->left);

        if (curr->right)
            q.push(curr->right);
    }
}
```


### 7. 트리 순회 테스트 코드
```cpp
#include <iostream>
#include <queue>

using std::cout;
using std::endl;
using std::queue;

struct Node
{
    char data;
    Node* left;
    Node* right;

    Node(char data) : data(data), left(nullptr), right(nullptr) {}
};


void preorder(Node* node)
{
    if (node)
    {
        cout << node->data << " ";
        preorder(node->left);
        preorder(node->right);
    }
}

void inorder(Node* node)
{
    if (node)
    {
        inorder(node->left);
        cout << node->data << " ";
        inorder(node->right);
    }
}

void postorder(Node* node)
{
    if (node)
    {
        postorder(node->left);
        postorder(node->right);
        cout << node->data << " ";
    }
}

void levelorder(Node* node)
{
    queue<Node*> q;
    q.push(node);

    while (!q.empty())
    {
        Node* curr = q.front();
        q.pop();

        cout << curr->data << " ";

        if (curr->left)
            q.push(curr->left);

        if (curr->right)
            q.push(curr->right);
    }
}


int main()
{
    /*
       A
     B   C
    D E   F
    */

    Node* A = new Node('A');
    A->left = new Node('B');
    A->right = new Node('C');
    A->left->left = new Node('D');
    A->left->right = new Node('E');
    A->right->right = new Node('F');

    preorder(A);
    cout << endl;
    inorder(A);
    cout << endl;
    postorder(A);
    cout << endl;
    levelorder(A);
    cout << endl;

    return 0;
}
```


### 8. 이진 트리의 순회
- 이진 트리 삭제하기
    - 각각의 노드에서 왼쪽 자식 노드와 오른쪽 자식 노드를 먼저 삭제하고, 
    - 자기 자신을 삭제하는 코드를 재귀적으로 수행

    - 후위 순회 방식
        - 1. 왼쪽 서브 트리 삭제
        - 2. 오른쪽 서브 트리 삭제
        - 3. 현재 노드(상위 노드) 삭제

        ```cpp
        void delete_tree(Node* node)
        {
            if (node)
            {
                delete_tree(node->left);
                delete_tree(node->right);
                delete node;
            }
        }
        ```

```cpp
#include <iostream>
#include <queue>

using std::cout;
using std::endl;
using std::queue;

struct Node
{
    char data;
    Node* left;
    Node* right;

    Node(char data) : data(data), left(nullptr), right(nullptr) {}
};


void preorder(Node* node)
{
    if (node)
    {
        cout << node->data << " ";
        preorder(node->left);
        preorder(node->right);
    }
}

void inorder(Node* node)
{
    if (node)
    {
        inorder(node->left);
        cout << node->data << " ";
        inorder(node->right);
    }
}

void postorder(Node* node)
{
    if (node)
    {
        postorder(node->left);
        postorder(node->right);
        cout << node->data << " ";
    }
}

void levelorder(Node* node)
{
    queue<Node*> q;
    q.push(node);

    while (!q.empty())
    {
        Node* curr = q.front();
        q.pop();

        cout << curr->data << " ";

        if (curr->left)
            q.push(curr->left);

        if (curr->right)
            q.push(curr->right);
    }
}

void delete_tree(Node* node)
{
    if (node)
    {
        delete_tree(node->left);
        delete_tree(node->right);
        cout << "delete node " << node->data << endl;
        delete node;
    }
}


int main()
{
    /*
       A
     B   C
    D E   F
    */

    Node* A = new Node('A');
    A->left = new Node('B');
    A->right = new Node('C');
    A->left->left = new Node('D');
    A->left->right = new Node('E');
    A->right->right = new Node('F');

    preorder(A);
    cout << endl;
    inorder(A);
    cout << endl;
    postorder(A);
    cout << endl;
    levelorder(A);
    cout << endl;

    delete_tree(A);

    return 0;
}
```


### 9. 추가 내용
- 순회 방식별 응용
    - 전위
        - 트리의 복사, 트리 구조를 그대로 출력할 때 사용

    - 중위
        - 이진 탐색 트리에서 오름차순 데이터 추출

    - 후위
        - 트리 삭제, 디렉토리 용량 계산, 수식 트리 계산

    - 레벨
        - 최단 경로 탐색, 네트워크 라우티 분석

- 알고리즘 복잡도 분석
    - 시간 복잡도: O(N)
    - 공간 복잡도:
        - DFS: O(H): 트리의 높이
        - BFS: O(W): 트리의 최대 너비

- 수식 트리(Expression Tree)
    - (A + B) * C
    - 전위 표기법
        - * + A B C

    - 중위 표기법
        - A + B * C

    - 후위 표기법
        - A B + C *

- 재귀를 사용하지 않고 DFS 구현
    - std::stack을 사용하여 직접 구현할 수 있음 (함수 호출 스택을 명시적 스택으로 대체)
    ```cpp
    #include <iostream>
    #include <stack>

    using std::cout;
    using std::endl;
    using std::stack;

    struct Node
    {
        char data;
        Node* left;
        Node* right;

        Node(char data) : data(data), left(nullptr), right(nullptr) {}
    };

    // void preorder(Node* node)
    // {
    //     if (node)
    //     {
    //         cout << node->data << " ";
    //         preorder(node->left);
    //         preorder(node->right);
    //     }
    // }

    void preorder(Node* node)
    {
        stack<Node*> stk;
        stk.push(node);

        while (!stk.empty())
        {
            Node* curr = stk.top();
            stk.pop();

            cout << curr->data << " ";

            if (curr->right)
                stk.push(curr->right);
            
            if (curr->left)
                stk.push(curr->left);
        }
    }

    void inorder(Node* node)
    {
        stack<Node*> stk;
        Node* curr = node;

        while (curr != nullptr || !stk.empty())
        {
            while (curr != nullptr)
            {
                stk.push(curr);
                curr = curr->left;
            }
            
            curr = stk.top();
            stk.pop();

            cout << curr->data << " ";

            curr = curr->right;
        }
    }

    void postorder(Node* node)
    {
        stack<Node*> s1;
        stack<Node*> s2;

        s1.push(node);

        while (!s1.empty())
        {
            Node* curr = s1.top();
            s1.pop();
            s2.push(curr);

            if (curr->left)
                s1.push(curr->left);

            if (curr->right)
                s1.push(curr->right);
        }

        while (!s2.empty())
        {
            cout << s2.top()->data << " ";
            s2.pop();
        }
    }

    void delete_tree(Node* node)
    {
        if (node)
        {
            delete_tree(node->left);
            delete_tree(node->right);
            cout << "delete node " << node->data << endl;
            delete node;
        }
    }


    int main()
    {
        /*
        A
        B   C
        D E   F
        G H   
        */

        Node* A = new Node('A');
        A->left = new Node('B');
        A->right = new Node('C');
        A->left->left = new Node('D');
        A->left->right = new Node('E');
        A->right->right = new Node('F');

        A->left->right->left = new Node('G');
        A->left->right->right = new Node('H');

        preorder(A);
        cout << endl;

        inorder(A);
        cout << endl;

        postorder(A);
        cout << endl;

        delete_tree(A);

        return 0;
    }
    ```
