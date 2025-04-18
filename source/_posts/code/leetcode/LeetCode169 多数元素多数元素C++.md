---
title: leetcode169多数元素
date: 2025-03-11 14:12:09
categories:
  - 力扣
tags:
  - 力扣
  - 代码
  - 摩尔投票法
  - CPP
mathjax: true
copyright: false
---
> [!NOTE] 
> 以下内容包含Chatgpt的帮助

# **题目描述**

给定一个大小为 `n` 的数组 `nums`，找出其中出现次数超过 `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 **示例**

**示例 1：**

```cpp
输入: nums = [3,2,3]
输出: 3
```

**示例 2：**

```cpp
输入: nums = [2,2,1,1,1,2,2]
输出: 2
```

 **进阶要求**

- 你能在**时间复杂度 O(n)**、**空间复杂度 O(1)** 的条件下完成算法吗？

---

# **解法 1：哈希表（map 计数）**
**思路**

我们可以使用哈希表（`unordered_map`）统计每个元素出现的次数，然后找到出现次数超过 `⌊ n/2 ⌋` 的元素。

**代码**

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;
class Solution {

public:
int majorityElement(vector<int>& nums) {
    unordered_map<int, int> count;
    int n = nums.size();

    for (int num : nums) {
        count[num]++;
        if (count[num] > n / 2) {
            return num;
        }
    }
    return -1; // 理论上不会执行到这里，因为题目保证一定有多数元素
}

int main() {
    vector<int> nums = {2,2,1,1,1,2,2};
    cout << majorityElement(nums) << endl;  // 输出 2
    return 0;
}
```

**复杂度分析**

- **时间复杂度：O(n)**，遍历数组一次统计出现次数。
- **空间复杂度：O(n)**，哈希表存储了最多 `n` 个不同的数。

---

# **解法 2：排序**

**思路**

- 由于多数元素的出现次数超过 `⌊ n/2 ⌋`，如果我们对数组进行排序，**中间的元素（索引 `n/2` 处）一定是多数元素**。
- 直接返回 `nums[n/2]`。

**代码**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int majorityElement(vector<int>& nums) {
    sort(nums.begin(), nums.end());  // 排序
    return nums[nums.size() / 2];    // 中间元素必定是多数元素
}

int main() {
    vector<int> nums = {2,2,1,1,1,2,2};
    cout << majorityElement(nums) << endl;  // 输出 2
    return 0;
}
```

**复杂度分析**

- **时间复杂度：O(n log n)**（排序的开销）。
- **空间复杂度：O(1)**（原地排序）。

---

# **解法 3：摩尔投票法**

 **思路**

- 由于多数元素出现的次数超过 `⌊ n/2 ⌋`，如果我们用一个 `count` 计数器，每次遇到相同的元素就加一，不同的元素就减一：
    - `count == 0` 时，我们假设当前元素是多数元素，并将 `count` 设为 1。
    - 继续遍历，最后剩下的 `candidate` 就是多数元素。

**代码**

```cpp
#include <iostream>
#include <vector>
using namespace std;

int majorityElement(vector<int>& nums) {
    int candidate = nums[0], count = 0;
    
    for (int num : nums) {
        if (count == 0) {
            candidate = num;  // 假设当前元素是多数元素
        }
        count += (num == candidate) ? 1 : -1;
    }
    
    return candidate;
}

int main() {
    vector<int> nums = {2,2,1,1,1,2,2};
    cout << majorityElement(nums) << endl;  // 输出 2
    return 0;
}
```

**复杂度分析**

- **时间复杂度：O(n)**，遍历数组一次即可确定多数元素。
- **空间复杂度：O(1)**，只使用了额外的变量 `candidate` 和 `count`。

## **摩尔投票法的直觉**

- 假设 `x` 是多数元素，`y` 是非多数元素。
- **多数元素的数量 > 其他所有元素的总和**，所以最终 `x` **一定会占据主导地位**。

**数学证明**

 假设多数元素是`M`，它出现了 `k > n/2` 次

假设整个数组长度为 `n`，其他所有元素的数量是 `n - k < k`。

1. 最坏情况下，`M` 与其他元素交错排列
    
    ```
    M, X, M, X, M, X, M, X, M, ...
    ```
    
    - 由于 `M` 的数量比 `X` 多，在投票过程中 `M` 仍然会剩余。
        
    - 假设 `X` 们全部抵消 `M`，但 `M` 仍然有 `k - (n - k) = 2k - n` 个存活，而 `X` 被完全抵消。
        
    - 因为 `k > n/2`，所以 `2k - n > 0`，即 `M` 仍然剩下，最终成为 `candidate`。
        
2. 无论 `M` 如何分布，它都不会被完全抵消
    
    - 设 `M` 的出现次数为 `x`，所有其他元素的出现次数总和为 `y`。
        
    - 由于 `x > y`，在遍历的过程中，`M` 可能会被 `y` 抵消掉 `y` 次，但 `M` 仍然剩余 `x - y` 次。
        
    - 因此，最终 `M` 仍然是最多的，且一定会成为 `candidate`。