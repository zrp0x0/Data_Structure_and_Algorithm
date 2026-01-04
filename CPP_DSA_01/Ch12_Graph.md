# S01. 그래프


### 1. 그래프(graph)
- 객체 간의 연결 관계를 표현할 수 있는 비선형 자료 구조
- 쾨니히스베르크의 다리 문제(한붓그리기, 오일러 경로)가 그래프 이론의 시초로 알려져 있음
    - 임의의 지점에서 다리를 한 번씩만 건너서 초기 위치로 돌아오는 경우의 수
    - 오일러가 수학적으로 가능한지 불가능한지 구하는 것을 증명함

- 그래프의 예: 지도에서 도시들의 연결, 지하철 노선도, SNS 친구 관계도 등


### 2. 그래프의 구성 요소
- 그래프 G = (V, E)
- 정점(vertex, node): V
    - 그래프를 구성하는 기본 단위. 노드
    - 자료를 저장하거나 상태를 표현

- 엣지(edge, link): E
    - 정점과 정점을 잇는 선. 간선
    - **방향을 가질 수 있음**
    - **가중치(weight)를 가질 수 있음**


### 3. 그래프 용어
- 가중치(weight)
    - 엣지에 부여된 수치. 비용(cost)

- 인접 정점(adjacent vertex)
    - 엣지로 연결되어 있는 정점

- 차수(degree)
    - 정점에 연결된 다른 정점의 개수

- 경로(path)
    - 특정 정점에서 다른 정점으로 이동하는 방법을 인접 정점의 나열로 표현한 것
    - 1 -> 4 -> 8 -> 2 -> 5

- 사이클(cycle)
    - 시작 정점과 마지막 정점이 같은 단순 경로(simple path)
    - 트리: 사이클이 없는 그래프

- 방향 그래프(directed graph)
    - 엣지에 방향이 있는 그래프
    - 유향 그래프, 네트워크(network) vs 무향 그래프(양방향)
    - 방향 그래프에서 엣지는 (u, v)는 정점 u에서 정점 v로 이동하는 엣지를 나타냄
        - (u, v) != (v, u)

- 가중치 그래프(weighted graph)
    - 엣지에 가중치가 부여되어 있는 그래프

- 서브 그래프(sub graph)
    - 주어진 그래프에서 정점과 간선 일부를 제외하여 만든 그래프. 부분 그래프


### 4. 그래프 표현 방법
- 인접 행렬(adjacency matrix)
    - 정점 개수가 N인 경우, N * N 크기의 2차원 행렬로 정점의 인접 관계를 표현하는 방법
    - adj[u][v]: 노드 u에서 노드 v로 가는 간선이 있으면 1, 아니면 0
    - **그래프 정점 개수가 적고, 엣지가 많을 때 유리. **공간 복잡도는 O(N^2)**
    - 무방향 그래프는 대칭
    - 유방향 그래프는 갈 수 있는 것만 1

```cpp
vector<vector<int>> adj_matrix = {
    {0, 1, 0, 1, 1, 0},
    {0, 1, 0, 1, 1, 0},
    {0, 1, 0, 1, 1, 0},
    {0, 1, 0, 1, 1, 0},
    {0, 1, 0, 1, 1, 0},
    {0, 1, 0, 1, 1, 0},
    {0, 1, 0, 1, 1, 0} // 정확하진 않지만 이런 느낌
};
```

- 인접 리스트(adjacency list)
    - 각 정점에 인접한 정점들을 연결 리스트로 표현
    - 보통 정점 개수에 해당하는 배열의 각 원소에 연결 리스트가 속해 있는 형태로 표현
    - 공간 복잡도는 O(N + M) (N: 정점 개수, M: 엣지 개수)
        - 엣지의 개수는 방향을 생각해야함 (양방향 2)

```cpp
vector<vector<int>> adj_list = {
    {1, 3, 4},
    {0, 2, 4},
    {1, 5},
    {0, 4},
    {0, 1, 3},
    {2}
}; // 벡터의 리스트로 표현하는 것은 비효율적이기 때문에 벡테의 벡터로 구성함
```

- 엣지 리스트(edge list)
    - 모든 엣지 (u, v)를 리스트 또는 배열로 표현
    - 공간 복잡도는 O(M) (M: 엣지 개수)
    
```cpp
vector<vector<int>> edge_list = {
    {0, 1}, {0, 3}, 
    ...
    ...
};
```


### 5. 인접 행렬을 인접 리스트로 변환하는 예제 프로그램
```cpp

#include <iostream>
#include <vector>

using namespace std;

vector<vector<int>> invert_adj_list(const vector<vector<int>>& adj_matrix)
{
    vector<vector<int>> adj_list(adj_matrix.size());

    for (int u = 0; u < adj_matrix.size(); u++)
    {
        for (int v = 0; v < adj_matrix[u].size(); v++)
        {
            if (adj_matrix[u][v] == 1)
            {
                adj_list[u].push_back(v);
            }
        }
    }

    return adj_list;
}

int main()
{
    vector<vector<int>> adj_matrix = {
        {0, 1, 0, 1, 1, 0},
        {1, 0, 1, 0, 1, 0},
        {0, 1, 0, 0, 0, 1},
        {1, 0, 0, 0, 1, 0},
        {1, 1, 0, 1, 0, 0},
        {0, 0, 1, 0, 0, 0}
    };

    /*
    1 3 4 
    0 2 4 
    1 5 
    0 4 
    0 1 3 
    2 
    */

    vector<vector<int>> adj_list = invert_adj_list(adj_matrix);

    for (const auto& lst : adj_list)
    {
        for (const auto& n : lst)
        {
            cout << n << " ";
        }
        cout << endl;
    }

    return 0;
}
```


### 6. 추가 내용
- 차수의 세분화
    - 방향 그래프일 경우 두 가지로 나누어서 생각해야함
        - 진입 차수(In-degree): 해당 정점으로 들어오는 엣지의 수
        - 진출 차수(Out-degree): 해당 정점으로 나가는 엣지의 수
    - 이유: 방향 그래프를 이용한 알고리즘(위상 정렬)에서는 진입 차수가 0인 노드를 찾는 것이 매우 중요함

- 사이클과 트리의 관계의 엄밀성
    - 사이클이 없으며
    - 모든 정점이 연결된 그래프
        - 정점이 연결이 안되어 있는 그래프도 있음

- vector<list> vs vector<vector>
    - 캐시 지역성이 vector<vector>가 훨씬 좋음

- 암시적 그래프
    - 미로 찾기 문제에서 N*M 격자판 자체가 그래프 (상하좌우가 엣지)
    - 이럴 경우 굳이 리스트를 따로 만들지 않고 배열을 사용해 이동 가능한 정점을 계산함



---
# S02. 그래프 순회: DFS와 BFS


### 1. 그래프 순회(graph traversal)
- 하나의 정점에서 시작해서 모든 정점들을 한번씩 방문하는 작업
- 많은 문제들이 그래프 순회를 이용하여 해결될 수 있음
    - 두 정점 사이의 연결 경로가 있는지 검사, 최단 거리 검사 등

- 대표적 그래프 순회 방법
    - 깊이 우선 탐색(Depth First Search)
    - 너비 우선 탐색(Breadth First Search)



### 2. 깊이 우선 탐색
- 현재 정점과 인접한 정점 중 하나를 선택해서 이동하는 과정을 반복하고,
- 더 이상 이동할 정점이 없을 경우 뒤쪽으로 백트래킹(backtracking)하여 다시 이동할 경로를 탐색
- 시작 정점으로부터 거리가 멀어지는(깊이가 깊어지는) 방식으로 정점을 탐색
- 보통 재귀 또는 스택을 이용하여 구현
- 보통 **재귀 또는 스택**을 이용하여 구현

- ex. 0 - 1 - 2 - 5 - <2> - <1> - 4 - 3 / <>: 백트래킹

- 한가지 방문 순서만 있는 것이 아님!!!


### 3. 너비 우선 탐색
- 현재 정점과 인접한 모든 정점을 방문한 후, 다시 이들 정점과 인접한 모든 정점을 찾아 방문하는 방식
- 시작 정점으로부터 가까운 정점을 먼저 방문하고, 멀리 떨어져 있는 정점을 나중에 방문
- 보통 **큐**를 이용하여 구현

- ex. (0 - 1 - 3 - 4) - 2 - 5
    - 0 - (1 3 4)
    - 1 - (2 4)
    - 2 - (5)


### 4. DFS 구현하기 - 재귀 이용 방법
```cpp
const int N = 6;
bool gVisited[N] = {};

void dfs_recursion(const vector<vector<int>>& adj_list, int s)
{
    // 1. 이미 s를 방문했으면 함수 종료
    if (gVisited[s])
        return;

    // 2. 그렇지 않으면 s를 방문
    gVisited[s] = true;
    cout << s << " ";

    // 3. s와 인접한 모든 노드에 대해 dfs_recursion() 함수를 재귀 호출
    for (int v : adj_list[s])
        dfs_recursion(adj_list, v);
}

int main()
{
    vector<vector<int>> adj_list = { {1, 3, 4}, {0, 2, 4}, {1, 5}, {0, 4}, {0, 1, 2}, {2} }
    dfs_recursion(adj_list, 0);
    cout << endl;
}
```


### 5. DFS 구현하기 - 스택 이용 방법
```cpp
vector<int> dfs(const vector<vector<int>>& adj_list, int s)
{
    vector<bool> visited(adj_list.size(), false); // 방문 여부 기록
    vector<int> visit_order; // 각각의 정점 방문 순서를 기록

    // 1. 비어 있는 스택을 생성하고 s를 삽입
    stack<int> stk;
    stk.push(s);

    while (!stk.empty())
    {
        // 2. 스택에서 정점 번호를 꺼내서 변수 v로 설정
        int v = stk.top();
        stk.pop();

        // 3. 이미 v를 방문했으면 스택의 다음 정점 번호를 꺼냄
        if (visited[v])
            continue;

        // 4. 그렇지 않으면 v를 방문
        visited[v] = true;
        visit_order.push_back(v);

        // 5. v와 인접한 노드 중에서 아직 방문하지 않은 정점 번호를 스택에 삽입
        for (int a : adj_list[v])
        {
            if (!visited[a])
                stk.push(a);
        }
    }

    return visit_order;
}
```


### 6. BFS 구현하기 - 큐 이용 방법
```cpp
vector<int> bfs(const vector<vector<int>>& adj_list, int s)
{
    vector<bool> visited(adj_list.size(), false); // 방문 여부 기록
    vector<int> visit_order; // 각각의 정점 방문 순서를 기록

    // 1. 비어 있는 스택을 생성하고 s를 삽입
    queue<int> q;
    q.push(s);

    while (!q.empty())
    {
        // 2. 큐에서 정점 번호를 꺼내서 변수 v로 설정
        int v = q.front();
        q.pop();

        // 3. 이미 v를 방문했으면 스택의 다음 정점 번호를 꺼냄
        if (visited[v])
            continue;

        // 4. 그렇지 않으면 v를 방문
        visited[v] = true;
        visit_order.push_back(v);

        // 5. v와 인접한 노드 중에서 아직 방문하지 않은 정점 번호를 스택에 삽입
        for (int a : adj_list[v])
        {
            if (!visited[a])
                q.push(a);
        }
    }

    return visit_order;
}
```


### 7. 전체 예제
```cpp

#include <iostream>
#include <vector>
#include <stack>
#include <queue>
using namespace std;

vector<int> dfs(const vector<vector<int>>& adj_list, int s)
{
    vector<bool> visited(adj_list.size(), false); // 방문 여부 기록
    vector<int> visit_order; // 각각의 정점 방문 순서를 기록

    // 1. 비어 있는 스택을 생성하고 s를 삽입
    stack<int> stk;
    stk.push(s);

    while (!stk.empty())
    {
        // 2. 스택에서 정점 번호를 꺼내서 변수 v로 설정
        int v = stk.top();
        stk.pop();

        // 3. 이미 v를 방문했으면 스택의 다음 정점 번호를 꺼냄
        if (visited[v])
            continue;

        // 4. 그렇지 않으면 v를 방문
        visited[v] = true;
        visit_order.push_back(v);

        // 5. v와 인접한 노드 중에서 아직 방문하지 않은 정점 번호를 스택에 삽입
        for (int a : adj_list[v])
        {
            if (!visited[a])
                stk.push(a);
        }
    }

    return visit_order;
}


vector<int> bfs(const vector<vector<int>>& adj_list, int s)
{
    vector<bool> visited(adj_list.size(), false); // 방문 여부 기록
    vector<int> visit_order; // 각각의 정점 방문 순서를 기록

    // 1. 비어 있는 스택을 생성하고 s를 삽입
    queue<int> q;
    q.push(s);

    while (!q.empty())
    {
        // 2. 큐에서 정점 번호를 꺼내서 변수 v로 설정
        int v = q.front();
        q.pop();

        // 3. 이미 v를 방문했으면 스택의 다음 정점 번호를 꺼냄
        if (visited[v])
            continue;

        // 4. 그렇지 않으면 v를 방문
        visited[v] = true;
        visit_order.push_back(v);

        // 5. v와 인접한 노드 중에서 아직 방문하지 않은 정점 번호를 스택에 삽입
        for (int a : adj_list[v])
        {
            if (!visited[a])
                q.push(a);
        }
    }

    return visit_order;
}

void print_vector(const vector<int>& v)
{
    for (int n : v)
    {
        cout << n << " ";
    }
    cout << endl;
}

int main()
{
    vector<vector<int>> adj_list = 
    {
        {1, 3, 4},
        {0, 2, 4},
        {1, 5},
        {0, 4},
        {0, 1, 3},
        {2}
    };

    auto dfs_order = dfs(adj_list, 0);
    auto bfs_order = bfs(adj_list, 0);

    print_vector(dfs_order);
    print_vector(bfs_order);

    return 0;
}
```


### 8. 추가 내용
- BFS와 최단 거리
    - BFS는 가중치가 없는 그래프에서 시작 정점과 특정 정점까지의 최단 경로를 보장함
    - BFS는 가까운 곳부터 탐색하므로 처음 발견한 경로가 반드시 가장 짧은 경로임

- 시간 복잡도
    - DFS / BFS: O(V + E)
    - 모든 정점을 한 번씩 방문하고, 각 정점에서 연결된 모든 간선을 확인하기 때문임

- 효율적인 구현
    - BFS
    ```cpp
    // 5. v와 인접한 노드 중에서 아직 방문하지 않은 정점 번호를 스택에 삽입
        for (int a : adj_list[v])
        {
            if (!visited[a])
            {
                visited[a] = true;
                q.push(a);
            }
        }
    ```
    - 꺼내는 시점에서 방문 처리를 하는 것이 메모리/시간적 이점

- 항상 말하지만 재귀는 그래프 깊이가 깊어지면 사용하면 스택 오버플로우가 일어남

- 사용처
    - 최단 경로 찾기: BFS
    - 경로의 특징을 저장해야할 때: DFS
    - 그래프 규모가 매우 클 때: DFS
    - 모든 정점 방문: 둘 다 무관 / DFS 구현이 쉬워 선호하는 편
