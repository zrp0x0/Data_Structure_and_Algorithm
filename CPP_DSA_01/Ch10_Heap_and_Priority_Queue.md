# S01. 힙 01


### 1. 힙(heap)이란?
- 더미, 무더기, 퇴적물 등을 쌓아올리다
- 자료 구조에서 힙은 **완전 이진 트리**의 한 형태로서 힙 속성을 만족하는 자료 구조.
- **이진 힙** / 항상 이진 트리여야하는 건 아님
- **힙 속성**
    - **최대 힙 속성(max heap property)**: 부모 노드의 키 값은 항상 자식 노드의 키 값보다 크거나 같음
    - **최소 힙 속성(min heap property)**: 부모 노드의 키 값은 항상 자식 노드의 키 값보다 작거나 같음


### 2. 힙의 특징
- 루트 노드는 항상 최댓값 또는 최솟값을 갖음
- 부모-자식 사이의 크기 관계만 있고, 왼쪽 자식-오른쪽 자식 사이의 크기 관계는 없음
- 완전 이진 트리이기 때문에 트리의 높이는 h = [log2 N]
- 힙의 응용 분야: **힙 정렬**, **우선 순위 큐**, **다익스트라 알고리즘** 등



### 3. 힙 연산의 시간 복잡도
- 최댓값 또는 최솟값 참조: O(1)
- 원소 삽입: O(log N)
- 원소 삭제: O(log N)


### 4. 힙과 이진 탐색 트리
- 트리의 형태
    - 완전 이진 트리 / 이진 트리

- 원소 중복 여부
    - 중복 가능 / 중복되지 않음

- 원소의 정렬 여부
    - 정렬되지 않음 / 정렬됨(중위 탐색)

- 빠른 원소 탐색
    - 미지원(순차 탐색, O(N)) / 지원(이진 탐색, O(log N))

- 원소의 삽입 또는 삭제
    - O(log N) / O(log N), O(N)

- 최대값/최솟값 참조
    - O(1) / O(log N), O(N)

- 물리적 구조
    - 배열 기간 / 연결리스트 기반

- 캐시 효율성
    - 높음(연속된 메모리 사용) / 낮음(포인터로 인한 메모리 파편화)
    - 같은 O(log N)이라고 힙이 더 빠르게 동작하는 경우가 많음

- 사용 목적
    - 최대값/최솟값의 빠른 추출 / 데이터의 정렬 상태 유지 및 검색


### 5. 힙 구현
- 힙은 완전 이진 트리이므로 각 노드에 인덱스를 붙이고, 배열을 이용하여 쉽게 표현할 수 있음
- 구현의 편의상 루트 노드의 인덱스를 1부터 시작(배열의 0번 인덱스는 무시)
- 노드로부터 부모 노드/자식 노드 인덱스 구하기
    - 2 / 2 = 1 (부모 노드)
    - 2 * 2 = 4 (자식 노드)
    - 2 * 2 + 1 = 5 (자식 노드)

    - 3 / 2 = 1 (부모 노드)
    - 3 * 2 = 6 (자식 노드)
    - 3 * 2 + 1 = 7 (자식 노드)

    - 5 / 2 = 2 (부모 노드)
    - 5 * 2 = 10 (자식 노드)
    - 5 * 2 + 1 = 11 (자식 노드)

    - 부모 노드: n / 2
    - 왼쪽 자식 노드: n * 2
    - 오른쪽 자식 노드: n * 2 + 1


### 6. 최대 힙 구현한 C++ 클래스 설계
```cpp
class MaxHeap
{
private:
    vector<int> vec;

public:
    MaxHeap() : vec(1) {}

    void push(int value);
    void pop();
    int top() const { return vec[1]; }
    int size() const {return vec.size() - 1; }
    bool empty() { return vec.size() == 1; }

private:
    int parent(int i) { return i / 2; }
    int left(int i) { return i * 2; }
    int right(int i) {return i * 2 + 1; }
}
```


### 7. 힙에 원소 삽입
- 1. 힙의 맨 마지막에 새로운 원소 값을 갖는 노드를 추가함
- 2. 새로 삽입한 노드와 부모 노드를 서로 비교하여 힙 속성을 만족하지 않으면 현재 노드와 부모 노드를 교환함
- 3. 부모 노드에서 이 작업을 반복함(heapify_up)

```cpp

void push(int value)
{
    vec.push_back(value);
    heapify_up(vec.size() - 1);
}

void heapify_up(int i)
{
    if (i > 1 && vec[i] > vec[parent(i)]) // i > 1: 루트 노드면 검사할 필요가 없음
    {
        swap(vec[i], vec[parent(i)]);
        heapify_up(parent(i));
    }
}
```

- 재귀 오버헤드
```cpp
void heapify_up(int i)
{
    while (i > 1 && vec[i] > vec[parent(i)])
    {
        swap(vec[i], vec[parent(i)]);
        i = parent(i);
    }
}
```


### 8. 추가 내용
- 힙 생성
    - 데이터가 이미 N개 있는 배열을 힙으로 만들 때, push를 N번하면 NlogN이 걸림
    - 바닥에서 위로 올라가는 방식(heapify_down)을 활용하면 O(N)만에 가능
        - std::make_heap이 이 방식을 사용 

- d-ary heap
    - 이진 힙은 자식이 2개 고정
    - 네트워크 라우팅이나 외부 메모리 알고리즘에서는 자식을 d개로 늘린 d-ary Heap 사용
        - 자식이 많아지면 트리 높이가 낮아져서 삽입 속도가 빨라지지만 삭제 시 자식들 비교하는 비용이 커짐




---
# S02. 힙 02


### 1. 힙에서 원소 삭제
- 힙의 맨 마지막 노드 값을 루트 노드로 복사하고 맨 마지막 노드를 삭제함
- 루트 노드와 두 자식 노드를 비교하여 힙 속성을 만족하지 않으면 두 자식 노드 중에서 더 큰(최대 힙) 노드와 서로 교환함
- 교환한 자식 노드에서 이 작업을 반복(heapify_down)

```cpp
void pop()
{
    vec[1] = vec.back();
    vec.pop_back();
    heapify_down(1);
}

void heapify_down(int i)
{
    int largest = i;

    if (left(i) < vec.size() && vec[left(i)] > vec[largest])
        largest = left(i);

    if (right(i) < vec.size() && vec[right(i)] > vec[largest])
        largest = right(i);

    if (largest != i) // 변경이 된 경우를 말함
    {
        swap(vec[i], vec[largest]);
        heapify_down(largest);
    }
}
```


### 2. Max Heap 구현 예제
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

class MaxHeap
{
private:
	vector<int> vec;

public:
	MaxHeap() : vec(1) {}

	void push(int value)
	{
		vec.push_back(value);
		heapify_up(vec.size() - 1);
	}

	void pop()
	{
		vec[1] = vec.back();
		vec.pop_back();
		heapify_down(1);
	}

	int top() const { return vec[1]; }
	void clear()
	{
		vec.clear();
		vec.push_back(0);
	}
	int size() { return vec.size() - 1; }
	bool empty() { return vec.size() == 1; }

    void print()
    {
        for (int i = 1; i < vec.size(); i++)
        {
            cout << vec[i] << " ";
        }
        cout << endl;
    }

	void heapify_up(int i)
	{
		while (i > 1 && vec[i] > vec[parent(i)])
		{
			std::swap(vec[i], vec[parent(i)]);
			i = parent(i);
		}
	}

	void heapify_down(int i)
	{
		int largest = i;

		if (left(i) < vec.size() && vec[left(i)] > vec[largest])
			largest = left(i);

		if (right(i) < vec.size() && vec[right(i)] > vec[largest])
			largest = right(i);

		if (largest != i)
		{
			std::swap(vec[i], vec[largest]);
			heapify_down(largest);
		}
	}

private:
	int parent(int i)
	{
		return i / 2;
	}

	int left(int i)
	{
		return i * 2;
	}

	int right(int i)
	{
		return i * 2 + 1;
	}
};

int main()
{
    MaxHeap heap;
    heap.push(10);
    heap.push(5);
    heap.push(15);
    heap.push(7);
    heap.push(3);
    heap.push(9);
    heap.push(14);
    heap.push(8);
    heap.push(2);
    heap.push(4);

    heap.print();

    while (!heap.empty())
    {
        cout << heap.top() << " ";
        heap.pop();
    }
    cout << endl;

	return 0;
}
```


### 3. 추가 내용
- C++ STL Heap (std::priority_queue)
    - std::push_heap: 새로운 원소가 끝에 추가된 컨테이너를 힙으로 재정렬
    - std::pop_heap: 루트를 끝으로 보내고 나머지로 힙 재건
    - std::sort_heap: 힙을 정렬된 상태로 만듦

- 힙 정렬
    - 배열을 힙으로 변환
    - 루트를 뒤로 보내기
    - 남은 원소 힙 재건
        - 시간 복잡도: O(NlogN): 최선, 평균, 최악
            - 힙 빌드: O(N)
            - heapify_down: O(log N)
        - 공간 복잡도: O(1)

    - **주로 최악의 시나리오**가 매우매우 중요한 시스템에서 사용함



---
# S03. 우선순위 큐와 std::priority_queue


### 1. 우선순위 큐(priority queue)
- 각각의 데이터에 우선순위(priority)가 정의되어 있어서, 입력 순서에 상관없이 우선순위가 가장 높은 데이터가 먼저 출력되는 형태의 자료 구조
- 우선순위의 예: 응급 자동차, 네트워크 트래픽 우선 순위, 운영체제에서 프로그램 우선 순위
- 일반 선입선출 큐: 큐에 머무른 시간을 우선순위로 설정하면 일반 큐와 같이 동작


### 2. 우선순위 큐 구현 방법
- 순서 없는 배열
    - 삽입: O(1)
    - 삭제: O(n)

- 순서 없는 연결 리스트
    - 삽입: O(1)
    - 삭제: O(n)

- 정렬된 배열
    - 삽입: O(n)
    - 삭제: O(1)

- 정렬된 연결 리스트
    - 삽입: O(n)
    - 삭제: O(1)

- 힙
    - 삽입: O(log N)
    - 삭제: O(log N)

- STL에서 제공하는 std::priority_queue 컨테이너 사용


### 3. std::priority_queue
```cpp
template<class template
    class Container = std::vector<T>,
    class Compare = std::less<T>>
class priority_queue
```

- 우선순위 큐의 기능을 제공하는 **컨테이너 어댑터**
- 삽입 순서에 상관없이 우선순위가 가장 높은 큰(기본적으로 값이 가장 큰) 원소가 먼저 출력됨
- 사용자 정의 타입을 저장할 경우, **비교 연산**을 지원해야 함
- <queue>에 정의되어 있음
- 주요 멤버 함수
    - push(e): e를 추가 O(log N)
    - pop(): 최상위 원소를 제거 O(log N)
    - top(): 최상위 원소를 참조 O(1)


### 4. std::priority_queue 사용 예제
```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>
using namespace std;

template <typename T>
void print_queue(T q)
{
    while (!q.empty())
    {
        cout << q.top() << " ";
        q.pop();
    }
    cout << endl;
}

int main()
{
    vector<int> vec {4, 2, 3, 5, 1};
    priority_queue<int> pq1;
    for (int n : vec)
    {
        pq1.push(n);
    }
    print_queue(pq1);

    priority_queue<int, vector<int>, greater<int>> pq2(vec.begin(), vec.end());
    print_queue(pq2);

    return 0;
}
```

```cpp
#include <iostream>
#include <queue>
#include <string>

using namespace std;

struct Student 
{
    Student(string name, int score) : name(name), score(score) {}

    string name;
    int score;

    bool operator<(const Student& st) const
    {
        return this->score < st.score;
    }
};

int main()
{
    priority_queue<Student> students;
    students.push(Student("Amelia", 80));
    students.push(Student("Sophia", 40));
    students.push(Student("Olivia", 90));
    students.push(Student("George", 70));

    while (!students.empty())
    {
        auto& s = students.top();
        cout << s.name << " " << s.score << endl;
        students.pop();
    }

    return 0;
}
```

```cpp
#include <iostream>
#include <queue>
#include <string>

using namespace std;

struct Student 
{
    Student(string name, int score) : name(name), score(score) {}

    string name;
    int score;

    bool operator<(const Student& st) const
    {
        return this->score < st.score;
    }
    
    bool operator>(const Student& st) const
    {
        return this->score > st.score;
    }
};

int main()
{
    auto cmp = [](const Student& s1, const Student& s2)
    {
        return s1 > s2;
    };

    priority_queue<Student, vector<Student>, decltype(cmp)> students(cmp);
    students.push(Student("Amelia", 80));
    students.push(Student("Sophia", 40));
    students.push(Student("Olivia", 90));
    students.push(Student("George", 70));

    while (!students.empty())
    {
        auto& s = students.top();
        cout << s.name << " " << s.score << endl;
        students.pop();
    }

    return 0;
}
```


### 5. 추가 내용
- 질문
    - 순서 없는 배열
    - 정렬된 배열이 무슨말?

    - 왜 less가 기본값인데 큰 순서대로 나오는거지?

    - 우선순위 큐를 사용하는 목적 이유?
        - 정렬을 하면 되지 않나?
        - 정렬된 배열을 사용는거랑 무슨 상관이 있지?    
        - 시간 복잡도 사용이랑 
        - 보통 어느 때 사용을 하는지

    - Compare에는 람다를 써도 되는 건가? 함수 포인터도?

    - pq<int, vector<int>, > 
        - 여기서 int랑 vector<int>가 무엇을 의미하는 걸까?
        - 굳이 명시를 해야하는가?
