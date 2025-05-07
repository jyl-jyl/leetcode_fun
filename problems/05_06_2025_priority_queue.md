## Priority Queue
Leetcode 347
> Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

I need to learn more about PR so asked AI to help me get started

### Max-heap PQ (by default)
```
#include <iostream>
#include <queue>
#include <vector>

int main() {
    std::priority_queue<int> pq; // max-heap by default

    pq.push(10);
    pq.push(5);
    pq.push(20);

    while (!pq.empty()) {
        std::cout << pq.top() << " "; // prints the highest priority
        pq.pop();
    }

    return 0;
}
```

### Min-heap PQ
```
#include <iostream>
#include <queue>
#include <vector>
#include <functional>

int main() {
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;

    minHeap.push(10);
    minHeap.push(5);
    minHeap.push(20);

    while (!minHeap.empty()) {
        std::cout << minHeap.top() << " ";
        minHeap.pop();
    }

    return 0;
}
```

### Custom PQ with Struct
```
#include <iostream>
#include <queue>
#include <vector>

struct Task {
    int priority;
    std::string name;

    // define operator<
    bool operator<(const Task& other) const {
        return priority < other.priority; // higher priority comes first
    }
};

int main() {
    std::priority_queue<Task> tasks;

    tasks.push({3, "Write report"});
    tasks.push({5, "Fix bug"});
    tasks.push({1, "Email team"});

    while (!tasks.empty()) {
        std::cout << tasks.top().name << " (priority " << tasks.top().priority << ")\n";
        tasks.pop();
    }

    return 0;
}
```

### Impl for Max_heap PQ

- insert(int) – to add an element.
- extractMax() – to remove and return the element with the highest priority.
- peek() – to return the maximum element without removing it.
- Heap utility methods:
  - heapifyUp() – for maintaining the heap after insertion.
  - heapifyDown() – for maintaining the heap after extraction.

```
#include <iostream>
#include <vector>
#include <stdexcept>

class PriorityQueue {
private:
    std::vector<int> heap;

    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }

    void heapifyUp(int index) {
        while (index > 0 && heap[parent(index)] < heap[index]) {
            std::swap(heap[parent(index)], heap[index]);
            index = parent(index);
        }
    }

    void heapifyDown(int index) {
        int size = heap.size();
        int largest = index;
        int left = leftChild(index);
        int right = rightChild(index);

        if (left < size && heap[left] > heap[largest]) largest = left;
        if (right < size && heap[right] > heap[largest]) largest = right;

        if (largest != index) {
            std::swap(heap[index], heap[largest]);
            heapifyDown(largest);
        }
    }

public:
    void insert(int value) {
        heap.push_back(value);
        heapifyUp(heap.size() - 1);
    }

    int extractMax() {
        if (heap.empty()) throw std::runtime_error("Heap is empty");

        int max = heap[0];
        heap[0] = heap.back();
        heap.pop_back();
        heapifyDown(0);
        return max;
    }

    int peek() const {
        if (heap.empty()) throw std::runtime_error("Heap is empty");
        return heap[0];
    }

    bool isEmpty() const {
        return heap.empty();
    }

    void print() const {
        for (int val : heap) std::cout << val << " ";
        std::cout << std::endl;
    }
};

int main() {
    PriorityQueue pq;
    pq.insert(10);
    pq.insert(30);
    pq.insert(20);
    pq.insert(5);

    std::cout << "Current Heap: ";
    pq.print();

    std::cout << "Max: " << pq.extractMax() << "\n";

    std::cout << "Heap after extractMax: ";
    pq.print();

    return 0;
}
```

### Templated PQ
```
#include <iostream>
#include <vector>
#include <functional>  // for std::less
#include <stdexcept>

template <typename T, typename Container = std::vector<T>, typename Compare = std::less<T>>
class PriorityQueue {
private:
    Container heap;
    Compare comp;

    int parent(int i) const { return (i - 1) / 2; }
    int leftChild(int i) const { return 2 * i + 1; }
    int rightChild(int i) const { return 2 * i + 2; }

    void heapifyUp(int index) {
        while (index > 0 && comp(heap[parent(index)], heap[index])) {
            std::swap(heap[parent(index)], heap[index]);
            index = parent(index);
        }
    }

    void heapifyDown(int index) {
        int size = heap.size();
        int best = index;
        int left = leftChild(index);
        int right = rightChild(index);

        if (left < size && comp(heap[best], heap[left])) best = left;
        if (right < size && comp(heap[best], heap[right])) best = right;

        if (best != index) {
            std::swap(heap[index], heap[best]);
            heapifyDown(best);
        }
    }

public:
    void push(const T& value) {
        heap.push_back(value);
        heapifyUp(heap.size() - 1);
    }

    void pop() {
        if (heap.empty()) throw std::runtime_error("Heap is empty");
        heap[0] = heap.back();
        heap.pop_back();
        heapifyDown(0);
    }

    const T& top() const {
        if (heap.empty()) throw std::runtime_error("Heap is empty");
        return heap[0];
    }

    bool empty() const {
        return heap.empty();
    }

    std::size_t size() const {
        return heap.size();
    }

    void print() const {
        for (const auto& x : heap) std::cout << x << " ";
        std::cout << "\n";
    }
};

```
