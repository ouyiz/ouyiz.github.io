---
title: C++自定义排序方式
date: 2025-03-06 16:00:00
categories: [C++]
tags: [排序, 代码,C++]
mathjax: true
copyright: false
---
> 注意
> 以下内容为Chatgpt生成后整理

在 C++ 中，**自定义比较函数**（通常是一个函数或者函数对象）和**仿函数**（函数对象）都可以用于自定义排序规则或比较方式，但它们的实现方式和使用方法有所不同。

### **1. 自定义比较函数**

自定义比较函数是指一个普通的函数，它用于定义如何比较两个对象。通常，这个函数有两个参数，返回值是一个布尔值，表示这两个对象之间的比较结果。

#### **特点**：

- **形式**：自定义比较函数通常是一个独立的普通函数。
- **使用方式**：它可以作为参数传递给排序算法（如 `std::sort`）或数据结构（如 `std::priority_queue`）等，定义对象的排序或比较规则。
- **返回类型**：一般返回一个 `bool` 类型，表示是否满足某种条件（例如：`a < b`，返回 `true` 表示 `a` 小于 `b`）。

#### **示例**：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// 自定义比较函数
bool compare(int a, int b) {
    return a < b;  // 按升序排序
}

int main() {
    vector<int> v = {3, 1, 4, 1, 5, 9};
    
    sort(v.begin(), v.end(), compare);  // 使用自定义比较函数排序
    
    for (int num : v) {
        cout << num << " ";
    }
    return 0;
}
```

在这个示例中，`compare` 函数定义了一个比较规则，`std::sort` 采用该规则对 `vector<int>` 进行排序。

> 注意
> 在 C++ 的排序函数（比如 `sort`）中，这个 `compare` 函数的作用是告诉排序算法：当 `compare(a, b)` 返回 `true` 时，`a` 会排在 `b` 前面。

#### **优点**：

- 简单直接。
- 易于理解和使用。

#### **缺点**：

- 无法存储状态。如果需要在比较中保存某些信息或行为（如计数、配置等），就不太适合使用普通函数。

---

### **2. 仿函数（函数对象）**

仿函数是通过重载 `operator()` 来创建的对象，使得这个对象能够像函数一样被调用。换句话说，仿函数是一种行为类似于函数的类或结构体。它通常用于需要自定义行为且需要存储状态的场景。

#### **特点**：

- **形式**：仿函数是一个类或结构体，类中重载了 `operator()`，使得对象可以像函数一样调用。
- **使用方式**：和普通函数一样，仿函数也可以作为参数传递给算法或数据结构，但它可以保存状态（例如：比较次数、特定的配置信息等）。
- **返回类型**：和自定义比较函数一样，仿函数通常返回 `bool` 类型，但它可以执行更加复杂的操作。

#### **示例**：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// 定义仿函数（函数对象）
struct Compare {
    bool operator()(int a, int b) {
        return a < b;  // 按升序排序
    }
};

int main() {
    vector<int> v = {3, 1, 4, 1, 5, 9};
    
    // 使用仿函数排序
    sort(v.begin(), v.end(), Compare());
    
    for (int num : v) {
        cout << num << " ";
    }
    return 0;
}
```

在这个示例中，`Compare` 结构体重载了 `operator()`，使得它成为一个仿函数。我们在 `std::sort` 中使用了 `Compare()`，它会像一个函数一样进行排序。

#### **优点**：

- 可以保存状态。仿函数是类，因此可以在类中定义成员变量，保存一些额外的状态信息，提供更强的灵活性。
- 可以定义复杂的比较行为，除了简单的比较，还可以使用成员函数、静态变量等。

#### **缺点**：

- 相比普通函数，仿函数的实现相对复杂，需要定义一个类或结构体并重载 `operator()`。

---

### **3. 自定义比较函数与仿函数的区别**

| 特点       | 自定义比较函数                       | 仿函数（函数对象）                     |
| -------- | ----------------------------- | ----------------------------- |
| **实现形式** | 普通函数                          | 类或结构体，重载 `operator()`         |
| **使用方式** | 作为参数传递给算法或数据结构（如 `std::sort`） | 作为参数传递给算法或数据结构（如 `std::sort`） |
| **存储状态** | 无法存储状态                        | 可以存储状态，通过成员变量保存信息             |
| **灵活性**  | 较简单，适用于简单的比较                  | 更灵活，适合需要额外状态或行为的场景            |
| **适用场景** | 适合简单的比较和排序场景                  | 适合需要动态行为或存储状态的复杂场景            |

### **4.各个容器和算法支持自定义比较器的区别**
#### **1. `std::priority_queue`（优先队列）**

```cpp
priority_queue<T, vector<T>, Compare>
```

- **传入参数**：`Compare(a, b)` 传入的是 **父节点 `a`，子节点 `b`**。
- **返回值含义**：
    - 如果 `Compare(a, b) == true`，表示 **`a` 的优先级低于 `b`**，因此 `a` 会排在 `b` 的下方。
    - **默认是 `std::less<T>`（最大堆），要实现小顶堆，用 `std::greater<T>` 或自定义 `Compare`**。

📌 **示例（小顶堆）：**

```cpp
struct Compare {
    bool operator()(int a, int b) {
        return a > b; // `a` > `b`，`a` 低优先级
    }
};
priority_queue<int, vector<int>, Compare> pq;
```

**行为**：

- `pq.top()` 返回最小的元素。
- **最小堆（小顶堆）：堆顶是最小的元素**。

#### **2. `std::sort`（排序）**

```cpp
sort(begin, end, Compare)
```

- **传入参数**：`Compare(a, b)` 传入的是 **相邻的 `a` 和 `b`**。
- **返回值含义**：
    - 如果 `Compare(a, b) == true`，表示 **`a` 应该排在 `b` 前面**。
    - **默认是 `std::less<T>`（升序），如果 `Compare(a, b) == a > b`，则是降序排序**。

📌 **示例（降序排序）：**

```cpp
struct Compare {
    bool operator()(int a, int b) {
        return a > b; // `a` 放前面，降序
    }
};
vector<int> v = {3, 1, 4, 1, 5};
sort(v.begin(), v.end(), Compare()); // 排序后 v = {5, 4, 3, 1, 1}
```

**行为**：

- **返回 `true` 代表 `a` 应该在 `b` 前面**。
- **影响整个排序规则**。


#### **3. `std::set` / `std::map`（有序容器）**

```cpp
set<T, Compare>
map<Key, Value, Compare>
```

- **传入参数**：`Compare(a, b)` 传入的是 **两个元素**（`set` 的值 或 `map` 的键）。
- **返回值含义**：
    - 如果 `Compare(a, b) == true`，表示 **`a` 应该排在 `b` 前面**。
    - **如果 `Compare(a, b) == false && Compare(b, a) == false`，说明 `a == b`，此时 `set` / `map` 认为元素已存在，不会插入**。
    - **默认是 `std::less<T>`（升序），如果 `Compare(a, b) == a > b`，则是降序存储**。

📌 **示例（降序 `set`）：**

```cpp
struct Compare {
    bool operator()(int a, int b) {
        return a > b; // `a` 放前面，降序存储
    }
};
set<int, Compare> s = {3, 1, 4, 1, 5}; // 存储后 {5, 4, 3, 1}
```

📌 **示例（自定义 `map` 排序）：**

```cpp
struct Compare {
    bool operator()(const string& a, const string& b) {
        return a.length() < b.length(); // 按字符串长度排序
    }
};
map<string, int, Compare> m;
m["apple"] = 1;
m["banana"] = 2;
m["kiwi"] = 3; // 按长度排序，`kiwi` 会在 `apple` 前面
```

**行为**：

- **`set` / `map` 的顺序由 `Compare` 控制**。
- **必须保证 `Compare` 是严格弱序（Strict Weak Ordering），否则行为未定义**。


#### **4. `std::multiset` / `std::multimap`（可重复有序容器）**

```cpp
multiset<T, Compare>
multimap<Key, Value, Compare>
```

- **规则和 `set` / `map` 一样，但允许重复元素**。
- **多个相等的元素会按照 `Compare` 规定的顺序存储**。

📌 **示例（降序 `multiset`）：**

```cpp
multiset<int, greater<int>> ms = {1, 3, 2, 3, 2};
```

**行为**：

- **内部按 `greater<int>` 存储，顺序为 `{3, 3, 2, 2, 1}`**。


#### **5. `std::unordered_set` / `std::unordered_map`**

- **`unordered_set` 和 `unordered_map` 不能使用 `Compare`，因为它们使用哈希函数 (`hash<T>`) 进行存储，而不是比较器。**
- **但可以自定义 `hash` 和 `operator==` 以影响存储方式。**

📌 **示例（自定义哈希）：**

```cpp
struct MyHash {
    size_t operator()(const pair<int, int>& p) const {
        return p.first ^ p.second; // 自定义哈希计算
    }
};
unordered_set<pair<int, int>, MyHash> uset;
```

**行为**：

- **`unordered_set` 允许使用自定义 `hash` 影响存储，但不支持 `Compare` 影响顺序**。


#### **总结**

|STL 容器 / 算法|`Compare(a, b)` 作用|`true` 的含义|默认比较器|
|---|---|---|---|
|`priority_queue<T, vector<T>, Compare>`|传入 (父节点, 子节点)|`a` 优先级低于 `b`（`a` 在 `b` 下面）|`std::less<T>`（大顶堆）|
|`sort(begin, end, Compare)`|传入 (左元素, 右元素)|`a` 在 `b` 前面|`std::less<T>`（升序）|
|`set<T, Compare>`|传入 (元素1, 元素2)|`a` 在 `b` 前面|`std::less<T>`（升序）|
|`map<Key, Value, Compare>`|传入 (键1, 键2)|`a` 在 `b` 前面|`std::less<T>`（升序）|
|`multiset<T, Compare>`|传入 (元素1, 元素2)|`a` 在 `b` 前面|`std::less<T>`（升序）|
|`multimap<Key, Value, Compare>`|传入 (键1, 键2)|`a` 在 `b` 前面|`std::less<T>`（升序）|
|`unordered_set` / `unordered_map`|**不能用 `Compare`**，只能用 `hash<T>`|-|`hash<T>`|

1. **`priority_queue`**：`true` = **`a` 优先级低（更靠下）**。
2. **`sort`** / **`set`** / **`map`**：`true` = **`a` 应该在 `b` 前面**。
