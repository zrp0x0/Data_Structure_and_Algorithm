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
