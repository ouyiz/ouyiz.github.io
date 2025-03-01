---
title: 堆及C++实现应用
date: 2025-03-01 15:16:00
categories: [数据结构]
tags: [堆, 代码,C++]
copyright: false
---
堆（Heap）是一种特殊的树形数据结构，通常用来实现优先队列、排序算法等功能。堆是一种完全二叉树，它满足特定的顺序性质。我们可以分为最大堆和最小堆两种。
### 1. 堆的定义

堆是一个完全二叉树，其节点满足堆的性质：

- **最大堆（Max Heap）**：每个父节点的值大于或等于其子节点的值，即根节点是树中的最大值。
- **最小堆（Min Heap）**：每个父节点的值小于或等于其子节点的值，即根节点是树中的最小值。

堆的特点：
- **完全二叉树**：除了最底层外，其他层的节点都是满的，最底层的节点从左到右排列。完全二叉树的结构保证了堆的高效性。
- **堆的顺序性质**：堆中任意节点的值与其子节点的值有顺序关系，最大堆的父节点值大于子节点，而最小堆则相反。

### 2. 堆的基本操作

堆主要支持以下几种基本操作：

1. **插入元素（Insert）**：
    - 在堆的末尾插入一个新元素，并通过“上浮”操作将其放到正确的位置，以保持堆的顺序性质。
    - 上浮操作：如果新插入的元素比父节点大（最大堆）或小（最小堆），则与父节点交换位置，直到堆顺序性质被恢复。交换不会破环堆得性质，不需要堆化，因为父节点得值大于所有得子节点。
2. **删除根节点（Delete）**：
    - 删除堆的根节点，并将堆的最后一个元素放到根位置。然后通过“下沉”操作将其放到正确的位置，以恢复堆的顺序性质。
    - 下沉操作：将根节点与其子节点中较大的（最大堆）或较小的（最小堆）子节点交换，直到堆顺序性质被恢复。需要使用堆化。
3. **堆化（Heapify）**：
    - 堆化是将一个无序数组转换成一个堆结构。它的过程是从数组的最后一个非叶子节点开始，逐步执行“下沉”操作，直到堆顺序性质得到恢复。
4. **获取最大（最小）元素**：
    - 在最大堆中，根节点存储着最大元素；在最小堆中，根节点存储着最小元素。这个操作的时间复杂度是O(1)。
### 3. 堆的应用

堆广泛应用于以下场景：

- **优先队列**：堆可以用来实现优先队列，保证插入和删除操作都能按照优先级进行。
- **堆排序（Heap Sort）**：利用堆的性质，可以对元素进行排序。首先将元素插入堆中，然后逐个删除根节点，即得到一个有序序列。
### 4. 堆的时间复杂度

- **插入操作**：O(log n)，插入元素后最多需要上浮h层，所有为O(log n)
- **删除根节点操作**：O(log n)，删除根节点后需要执行下沉操作，也需要log(n)次比较和交换。
- **堆化操作**：O(n)，从最后一个非叶子节点开始执行下沉操作，整体的时间复杂度是O(n)。

> [!NOTE] 堆得高度为什么是O(log n)
> 对一棵完全二叉树，
> **第 0 层**（根节点）有 1 个结点。
> **第 1 层** 有 2个结点。
> **第 2 层** 有 4个结点。
> **第 h 层** 最多有 $2^h$ 个结点。
> 因此，**最多的总结点数**：1+2+4+⋯+$2^h$=$2^h$+1−1
> 设堆的结点总数为 n，则：n≤$2^{h+1}$-1
> 	n+1≤$2^{h+1}$
> 	$\log_2 (n+1)$≤h+1
> 	h≥ $\log_2 {(n+1)}-1$
> 	h=O(log n)
>

### 5. 堆的实现

堆通常用数组来实现，因为完全二叉树的结构适合用数组来表示：

- 数组中下标为 `i` 的元素的左子节点在下标 `2*i + 1`，右子节点在下标 `2*i + 2`，父节点则在下标 `(i-1) / 2`。
**代码：**

```cpp
class MaxHeap {
private:
    vector<int> heap;  // 使用vector存储堆元素

    // 堆化操作，从下标i开始，调整堆。（加上left(i)与right(i)都是最大堆）
    void heapify(int i, int n) {
        int largest = i;           // 假设当前节点是最大值
        int left = 2 * i + 1;      // 左子节点下标
        int right = 2 * i + 2;     // 右子节点下标

        // 如果左子节点比当前节点大，更新largest
        if (left < n && heap[left] > heap[largest]) {
            largest = left;
        }

        // 如果右子节点比当前节点大，更新largest
        if (right < n && heap[right] > heap[largest]) {
            largest = right;
        }

        // 如果largest不等于i，说明需要交换
        if (largest != i) {
            swap(heap[i], heap[largest]);
            heapify(largest, n);  // 递归调整下去
        }
    }

public:
    // 插入元素
    void insert(int val) {
        heap.push_back(val);  // 将新元素插入到堆的末尾
        int i = heap.size() - 1;
        // 上浮操作：如果新插入的元素比父节点大，则交换
        while (i > 0 && heap[(i - 1) / 2] < heap[i]) {
            swap(heap[i], heap[(i - 1) / 2]);
            i = (i - 1) / 2;  // 更新i为父节点
        }
    }

    // 删除堆顶元素（根节点）
    void deleteRoot() {
        if (heap.size() == 0) {
            cout << "Heap is empty!" << endl;
            return;
        }
        // 将堆的最后一个元素移动到根节点
        heap[0] = heap.back();
        heap.pop_back();
        // 调整堆，使其重新满足堆的性质
        heapify(0, heap.size());
    }

    // 建堆操作，调整整个数组为一个堆，以为最后一个结点的父节点为(n-2)/2,也就是n/2-1
    void buildHeap() {
        int n = heap.size();
        // 从最后一个非叶子节点开始，进行堆化操作
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(i, n);
        }
    }

    // 打印堆的元素
    void printHeap() {
        for (int i : heap) {
            cout << i << " ";
        }
        cout << endl;
    }

    // 获取堆的大小
    int size() {
        return heap.size();
    }

    // 获取堆顶元素
    int getRoot() {
        if (heap.size() > 0) {
            return heap[0];
        }
        cout << "Heap is empty!" << endl;
        return -1;  // 返回一个无效值
    }
};

int main() {
    MaxHeap maxHeap;

    // 插入元素
    maxHeap.insert(10);
    maxHeap.insert(20);
    maxHeap.insert(5);
    maxHeap.insert(30);
    maxHeap.insert(15);
    
    cout << "Heap after insertion: ";
    maxHeap.printHeap();

    // 删除堆顶元素
    maxHeap.deleteRoot();
    cout << "Heap after deleting root: ";
    maxHeap.printHeap();

    // 建堆操作
    vector<int> arr = {3, 1, 5, 2, 4, 6};
    MaxHeap customHeap;
    for (int val : arr) {
        customHeap.insert(val);
    }
    customHeap.buildHeap();
    cout << "Heap after building from array: ";
    customHeap.printHeap();

    return 0;
}
```


### 6.priority_queue
`priority_queue` 是 C++ 标准库中的 **优先队列**，底层通常由 **堆（heap）** 实现。默认情况下，它是一个 **最大堆**（大顶堆），即 **每次取出的元素都是当前最大的**。如果想要最小堆（小顶堆），需要进行一些特殊处理。

###### **1.`priority_queue` 的基本用法**
 **头文件**
`priority_queue` 需要包含 `<queue>` 头文件：
```cpp
#include <queue>
```

**定义 `priority_queue`**
```cpp
std::priority_queue<int> pq;  // 默认是最大堆（大顶堆）
```

```cpp
template <class T, 
          class Container = std::vector<T>, 
          class Compare = std::less<typename Container::value_type>>
class priority_queue;
```

|**参数**|**作用**|
|---|---|
|`T`|队列中存储的元素类型|
|`Container`|存储数据的底层容器，默认是 `std::vector<T>`|
|`Compare`|比较器，默认是 `std::less<T>`（大顶堆）|

**常用操作**

| 操作        | 说明       |
| --------- | -------- |
| `push(x)` | 插入元素 `x` |
| `pop()`   | 删除堆顶元素   |
| `top()`   | 返回堆顶元素   |
| `empty()` | 判断是否为空   |
| `size()`  | 返回元素个数   |

示例：
```cpp
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<int> pq;
    pq.push(3);
    pq.push(5);
    pq.push(1);
    pq.push(4);

    std::cout << "堆顶元素: " << pq.top() << std::endl;  // 输出 5（最大值）

    pq.pop();  // 删除 5

    std::cout << "新的堆顶元素: " << pq.top() << std::endl;  // 输出 4

    return 0;
}
```
输出：
```
堆顶元素: 5
新的堆顶元素: 4
```

---

###### **2. 小顶堆（最小堆）**

默认情况下，`priority_queue` 是 **最大堆**，如果需要 **最小堆**，可以使用 **`std::greater<T>`**：

```cpp
std::priority_queue<int, std::vector<int>, std::greater<int>> pq;
```

示例：

```cpp
#include <iostream>
#include <queue>
#include <vector>

int main() {
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
    minHeap.push(3);
    minHeap.push(5);
    minHeap.push(1);
    minHeap.push(4);

    std::cout << "堆顶元素: " << minHeap.top() << std::endl;  // 输出 1（最小值）

    minHeap.pop();

    std::cout << "新的堆顶元素: " << minHeap.top() << std::endl;  // 输出 3

    return 0;
}
```
**输出**

```
堆顶元素: 1
新的堆顶元素: 3
```

> [!NOTE] **`std::greater<T>
> `std::greater<T>` 是 C++ 标准库 `<functional>` 头文件中的一个**函数对象（仿函数）**，用于 **比较两个值的大小**，它的作用是**定义"大于"（>`）的比较规则**。

---

###### **3. 存储 `pair`（结构体、对象）**

有时，我们需要让 `priority_queue` 存储 **更复杂的数据类型**，例如 `pair<int, int>`，可以通过 **自定义比较方式** 来决定排序规则。

**(1)按第一个元素降序（默认）**

```cpp
std::priority_queue<std::pair<int, int>> pq;
```
这样 `pair<int, int>` **会按第一个元素降序排列**，也就是最大堆。
示例：
```cpp
#include <iostream>
#include <queue>

int main() {
    std::priority_queue<std::pair<int, int>> pq;
    pq.push({3, 100});
    pq.push({5, 200});
    pq.push({1, 300});
    pq.push({4, 400});

    while (!pq.empty()) {
        auto p = pq.top();
        std::cout << "(" << p.first << ", " << p.second << ")" << std::endl;
        pq.pop();
    }
    
    return 0;
}
```
**输出**

```
(5, 200)
(4, 400)
(3, 100)
(1, 300)
```

---

**(2) 按第一个元素升序（小顶堆）**

如果希望 **按 `pair` 的第一个元素升序排列**，可以使用 `std::greater<>`：
```cpp
std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;
```

示例：
```cpp
#include <iostream>
#include <queue>
#include <vector>

int main() {
    std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;
    pq.push({3, 100});
    pq.push({5, 200});
    pq.push({1, 300});
    pq.push({4, 400});

    while (!pq.empty()) {
        auto p = pq.top();
        std::cout << "(" << p.first << ", " << p.second << ")" << std::endl;
        pq.pop();
    }

    return 0;
}
```

 **输出**

```
(1, 300)
(3, 100)
(4, 400)
(5, 200)
```

---

###### **4. 自定义比较函数**

如果需要更复杂的排序规则（如按照 `pair.second` 排序），可以使用 **lambda 表达式或函数对象**。

 **(1) 使用 `lambda`**

```cpp
auto cmp = [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
    return a.second > b.second;  // 按 second 升序排列
};
std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(cmp)> pq(cmp);
```

示例：

```cpp
#include <iostream>
#include <queue>
#include <vector>

int main() {
    auto cmp = [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
        return a.second > b.second;  // 按 second 升序排列
    };

    std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(cmp)> pq(cmp);

    pq.push({1, 300});
    pq.push({2, 100});
    pq.push({3, 200});

    while (!pq.empty()) {
        auto p = pq.top();
        std::cout << "(" << p.first << ", " << p.second << ")" << std::endl;
        pq.pop();
    }

    return 0;
}
```

**输出**

```
(2, 100)
(3, 200)
(1, 300)
```

---

**(2) 使用 `struct`**

```cpp
struct Compare {
    bool operator()(const std::pair<int, int>& a, const std::pair<int, int>& b) {
        return a.second > b.second;  // 按 second 升序排列
    }
};

std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, Compare> pq;
```

---

###### **5. `priority_queue` 时间复杂度**

| 操作           | 时间复杂度    |
| ------------ | -------- |
| 插入 (`push`)  | O(log⁡n) |
| 取堆顶 (`top`)  | O(1)     |
| 删除堆顶 (`pop`) | O(log⁡n) |
