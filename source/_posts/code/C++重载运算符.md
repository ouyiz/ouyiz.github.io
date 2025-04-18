---
title: C++重载运算符
date: 2025-03-10 22:00:00
categories: [C++]
tags: [重载,代码,C++,运算符]
mathjax: true
copyright: false
---
> [!NOTE] 
> 以下内容为Chatgpt生成后整理


# **1. 基本概念**
### **(1)什么是运算符重载？**

**运算符重载**（Operator Overloading）允许我们为已有的运算符（如 `+`、`-`、`*`、`==` 等）定义新的行为，以便这些运算符能够处理我们自定义的类型（如类）。换句话说，运算符重载让运算符能够支持自定义类的对象，使得这些对象在运算符使用时像内置数据类型一样工作。
### **(2)为什么需要运算符重载？**

运算符重载能够让我们：

- 提高代码的可读性和可维护性。
- 让自定义类的对象可以使用常见的运算符，方便直接参与运算。

例如，考虑两个 `Complex` 类对象相加，如果没有运算符重载，我们可能需要调用复杂的成员函数来完成加法操作，而运算符重载可以让加法操作变得简洁直观。

- **C++ 允许重载的大部分运算符**
- **不能重载的运算符**：
    - `::`（作用域解析符）
    - `.*`（成员指针访问运算符）
    - `.`（成员访问运算符）
    - `sizeof`（求大小）
    - `typeid`（运行时类型识别）

---

# **2. 重载运算符的方式**

- **作为成员函数**（`operator+`、`operator==` 等）
- **作为友元函数**（`operator<<`、`operator>>` 等）
### (1)作为成员函数

```cpp
class 类名 {
public:
    返回类型 operator 运算符(参数列表) {
        // 运算符重载的实现
    }
};
````
### (2)作为非成员函数（友元函数）

```cpp
class 类名 {
    friend 返回类型 operator 运算符(参数列表);
};
```

---

# **3. 常见运算符重载示例**

## **(1) 重载 `+`（加法运算符）**

 **1）为什么要重载 `+`？**

默认情况下，C++ **仅支持内置类型的加法**：

```cpp
int a = 2, b = 3;
int c = a + b; // ✅ 可以相加
```

但如果你定义了一个**类**，默认情况下 **对象不能相加**：

```cpp
class Vector {
public:
    int x, y;
};

int main() {
    Vector v1{1, 2}, v2{3, 4};
    Vector v3 = v1 + v2; // ❌ 错误，编译失败！
}
```

📌 **原因**：

- C++ **不知道如何相加** `v1` 和 `v2`。
- 需要 **重载 `operator+`**，告诉 C++ 该怎么加。


**2) `operator+` 的基本语法**

```cpp
class ClassName {
public:
    返回类型 operator+(const ClassName& other) const {
        // 执行加法逻辑
    }
};
```

📌 **说明**：

- `operator+` **必须是成员函数或友元函数**。
- `const` **保证不会修改原对象**（推荐）。
- `other` **是另一个参与相加的对象**。
- **返回新对象**，而不是修改 `this`。



**3) 示例：为 `Vector` 类重载 `+`**

假设 `Vector` 表示二维向量 `(x, y)`，我们希望：

```cpp
Vector v1(1, 2);
Vector v2(3, 4);
Vector v3 = v1 + v2; // 结果 (4,6)
```

我们可以这样实现：

```cpp
#include <iostream>
using namespace std;

class Vector {
public:
    int x, y;

    // 构造函数
    Vector(int x = 0, int y = 0) : x(x), y(y) {}

    // 重载 `+`
    Vector operator+(const Vector& other) const {
        return Vector(x + other.x, y + other.y);
    }

    // 打印函数
    void print() const {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Vector v1(1, 2), v2(3, 4);
    Vector v3 = v1 + v2; // ✅ 使用 `+` 运算符
    v3.print(); // 输出: (4, 6)
}
```

📌 **关键点**：

- `Vector(int x, int y)` **构造函数** 用于创建对象。
- `operator+` **返回一个新的 `Vector`**，表示相加的结果。
- **不修改原对象**，保持对象不可变性（`const`）。



**4) `operator+` 的其他写法**

 （1）使用友元函数

如果 `operator+` 需要访问 `private` 成员，就可以**用友元函数**：

```cpp
class Vector {
private:
    int x, y;

public:
    Vector(int x = 0, int y = 0) : x(x), y(y) {}

    // 友元函数重载 `+`
    friend Vector operator+(const Vector& v1, const Vector& v2);

    void print() const {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

// 友元函数实现
Vector operator+(const Vector& v1, const Vector& v2) {
    return Vector(v1.x + v2.x, v1.y + v2.y);
}

int main() {
    Vector v1(1, 2), v2(3, 4);
    Vector v3 = v1 + v2;
    v3.print(); // 输出: (4, 6)
}
```

📌 **区别**：

- 友元函数 **不是类的成员**，但可以访问 `private` 成员。
- 适用于 **操作两个不同类的对象**（如 `Matrix + Vector`）。


（2）支持 `+=` 运算

为了让 `+=` 也能使用，我们可以重载 `operator+=`：

```cpp
class Vector {
public:
    int x, y;

    Vector(int x = 0, int y = 0) : x(x), y(y) {}

    // `+` 返回新对象
    Vector operator+(const Vector& other) const {
        return Vector(x + other.x, y + other.y);
    }

    // `+=` 直接修改当前对象
    Vector& operator+=(const Vector& other) {
        x += other.x;
        y += other.y;
        return *this; // 返回自身
    }
};

int main() {
    Vector v1(1, 2), v2(3, 4);
    
    v1 += v2; // ✅ 使用 `+=`
    cout << v1.x << ", " << v1.y << endl; // 输出: 4, 6
}
```

📌 **区别**：

- `operator+` **返回新对象**，不修改 `this`。
- `operator+=` **修改当前对象**，返回 `*this` 以支持 **链式调用**。

---
## **(2) 重载 `==`（比较运算符）**

```cpp
class Person {
public:
    string name;
    int age;

    Person(string name, int age) : name(name), age(age) {}

    // 重载 `==`
    bool operator==(const Person& other) const {
        return (name == other.name && age == other.age);
    }
};

int main() {
    Person p1("Alice", 25), p2("Alice", 25);
    if (p1 == p2) {
        cout << "相等" << endl;
    } else {
        cout << "不相等" << endl;
    }
}
```

📌 **注意**：

- `operator==` **必须返回 `bool`**。

---

## **(3) 重载 `<<`（流插入运算符，打印对象）**

流插入运算符 `<<` 不能作为类成员函数 **（因为 `cout` 在左侧，不属于 `Person` 类）**，必须定义为**友元函数**：

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    string name;
    int age;

    Person(string name, int age) : name(name), age(age) {}

    // 重载 `<<` 运算符（必须是友元函数）
    friend ostream& operator<<(ostream& os, const Person& p) {
        os << "Name: " << p.name << ", Age: " << p.age;
        return os;
    }
};

int main() {
    Person p("Alice", 25);
    cout << p << endl; // 自动调用 operator<<
}
```

📌 **注意**：

- `<<` **必须返回 `ostream&`，否则 `cout << p << "hello";` 不能连续输出**。

---

## **(4) 重载 `[]`（索引运算符）**

如果你希望 `obj[i]` 访问对象内部数据，可以重载 `operator[]`：

```cpp
#include <iostream>
using namespace std;

class Array {
private:
    int data[5];

public:
    Array() { for (int i = 0; i < 5; i++) data[i] = i * 10; }

    // 重载 `[]`
    int& operator[](int index) {
        return data[index]; // 允许修改
    }
};

int main() {
    Array arr;
    cout << arr[2] << endl; // 输出 20
    arr[2] = 99;
    cout << arr[2] << endl; // 输出 99
}
```

📌 **注意**：

- `operator[]` 返回 `int&` **可以修改数组内容**。

---

## **(5) 重载 `()`（函数调用运算符）**

#### **1. 为什么要重载 `()`？**

通常，我们调用函数的方式如下：

```cpp
int add(int a, int b) {
    return a + b;
}

int main() {
    cout << add(2, 3); // 输出 5
}
```

但如果你想创建一个**行为类似函数的对象**，可以**重载 `operator()`**，这样对象本身就能像函数一样使用：

```cpp
class Adder {
public:
    int operator()(int a, int b) {
        return a + b;
    }
};

int main() {
    Adder add;
    cout << add(2, 3); // 输出 5
}
```

这里 `Adder` **是一个对象，但它可以像函数一样调用！**

#### **2. 基本语法**

```cpp
class ClassName {
public:
    返回类型 operator()(参数列表) {
        // 函数体
    }
};
```

**说明：**

- `operator()` 让对象变成 **可调用对象**，类似普通函数。
- 可以有 **任意数量的参数**。
- 可以有 **返回值**，与普通函数一样。


#### **3. 代码示例**
**示例 1：最简单的 `()` 重载**

```cpp
#include <iostream>
using namespace std;

class Multiply {
public:
    int operator()(int a, int b) {
        return a * b;
    }
};

int main() {
    Multiply multiply;
    cout << multiply(3, 4) << endl; // 输出 12
}
```

📌 **理解**：

- `Multiply` 类有一个 `operator()`，执行乘法。
- `multiply(3, 4)` **看起来像函数调用**，但 `multiply` 实际上是一个对象。


**示例 2：用于 STL `std::sort` 的比较器**

STL 算法（如 `sort`）经常使用 `operator()` 作为**自定义排序规则**：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Compare {
public:
    bool operator()(int a, int b) {
        return a > b; // 降序排序
    }
};

int main() {
    vector<int> nums = {3, 1, 4, 1, 5, 9};
    
    sort(nums.begin(), nums.end(), Compare()); // 传入一个仿函数对象

    for (int n : nums) cout << n << " "; // 输出: 9 5 4 3 1 1
}
```

📌 **理解**：

- `Compare` **定义了 `operator()`**，用于比较两个数的大小。
- `sort` **不需要函数指针，而是直接传入 `Compare()` 对象**。


**示例 3：存储状态的函数对象**

函数对象可以**保存状态**，而普通函数**不能**：

```cpp
#include <iostream>
using namespace std;

class Counter {
private:
    int count;
public:
    Counter() : count(0) {}

    int operator()() {
        return ++count;
    }
};

int main() {
    Counter counter;
    cout << counter() << endl; // 输出 1
    cout << counter() << endl; // 输出 2
    cout << counter() << endl; // 输出 3
}
```

📌 **理解**：

- `Counter` 对象**记住了之前的 `count` 值**，每次调用 `()` 都会递增 `count`。
- **普通函数不能存储状态**，但**函数对象可以！**


**示例 4：多参数版本**

`operator()` 可以有多个参数：

```cpp
class Power {
public:
    double operator()(double base, int exponent) {
        double result = 1;
        while (exponent--) result *= base;
        return result;
    }
};

int main() {
    Power power;
    cout << power(2, 3) << endl; // 输出 8
}
```

📌 **理解**：

- `Power` 计算 `base^exponent`。
- **多个参数** 和普通函数一样！


#### **4. `operator()` 的高级用法**

**(1) 作为 Lambda 表达式的替代**

`operator()` **和 Lambda 很像**，它可以代替 Lambda 表达式：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class LambdaLike {
public:
    bool operator()(int x) {
        return x % 2 == 0; // 仅返回偶数
    }
};

int main() {
    vector<int> nums = {1, 2, 3, 4, 5, 6};

    nums.erase(remove_if(nums.begin(), nums.end(), LambdaLike()), nums.end());

    for (int n : nums) cout << n << " "; // 输出: 2 4 6
}
```

📌 **理解**：

- `LambdaLike` **像 Lambda 一样**，筛选偶数。


**(2) 在 `map` 中用作自定义比较器**

```cpp
#include <iostream>
#include <map>
using namespace std;

class ReverseCompare {
public:
    bool operator()(int a, int b) {
        return a > b; // 使 `map` 变成降序
    }
};

int main() {
    map<int, string, ReverseCompare> myMap;
    myMap[3] = "Three";
    myMap[1] = "One";
    myMap[2] = "Two";

    for (const auto& pair : myMap) {
        cout << pair.first << ": " << pair.second << endl;
    }
}
// 输出:
// 3: Three
// 2: Two
// 1: One
```

📌 **理解**：

- `operator()` 让 `map` **按键降序排序**。


#### **5. `operator()` 的总结**

|作用|适用场景|例子|
|---|---|---|
|让对象像函数一样调用|计算、操作类|`Multiply multiply; multiply(2,3);`|
|作为 STL 自定义规则|`sort()`、`map`|`sort(v.begin(), v.end(), Compare());`|
|代替 Lambda|`remove_if`|`nums.erase(remove_if(nums.begin(), nums.end(), LambdaLike()), nums.end());`|
|具有状态的可调用对象|记忆历史调用|`Counter counter; counter();`|

---

## **(6) 重载 `->`（箭头运算符）**

如果你的类包含指针对象，可以重载 `->` 使其像指针一样访问：

```cpp
#include <iostream>
using namespace std;

class PtrWrapper {
private:
    int* ptr;

public:
    PtrWrapper(int val) { ptr = new int(val); }
    ~PtrWrapper() { delete ptr; }

    // 重载 `->`
    int* operator->() { return ptr; }
};

int main() {
    PtrWrapper p(42);
    cout << *(p.operator->()) << endl; // 输出 42
}
```

📌 **注意**：

- 适用于 **智能指针**，如 `std::unique_ptr<T>`。

---

## **(7) 重载 `=`（赋值运算符）**

必须处理**深拷贝**问题：

```cpp
class String {
private:
    char* data;

public:
    String(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }

    ~String() { delete[] data; }

    // 重载 `=`
    String& operator=(const String& other) {
        if (this == &other) return *this; // 避免自赋值
        delete[] data;
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
        return *this;
    }
};
```

📌 **注意**：

- `operator=` **要检查自赋值，防止内存泄漏**。

---

# **总结**

|运算符|作用|典型应用|备注|
|---|---|---|---|
|`operator+`|加法|`a + b`|返回新对象|
|`operator-`|减法|`a - b`|返回新对象|
|`operator==`|比较|`a == b`|返回 `bool`|
|`operator<<`|输出|`cout << obj`|需要友元函数|
|`operator[]`|索引|`obj[i]`|允许修改数据|
|`operator()`|函数调用|`obj(x, y)`|仿函数|
|`operator->`|指针访问|`obj->method()`|适用于指针包装类|
|`operator=`|赋值|`a = b`|处理深拷贝|
