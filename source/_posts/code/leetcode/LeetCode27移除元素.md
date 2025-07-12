---
title: LeetCode27.移除元素C++
categories:
  - 力扣
tags:
  - 编程
  - 代码
  - 数组
  - CPP
  - 双指针
mathjax: true
date: 2025-04-18 16:02:14
---
## 题目
给你一个数组 `nums` 和一个值 `val`，你需要原地移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

示例：

```text
输入: nums = [3,2,2,3], val = 3
输出: 2, nums = [2,2,_,_]

输入: nums = [0,1,2,2,3,0,4,2], val = 2
输出: 5, nums = [0,1,3,0,4,_,_,_]
```

**提示：**
- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`


## 答案
### 方法1：笨方法
找个数组存不等于val的元素，然后赋值回去。

时间复杂度： `O(n)`
空间复杂度：`O(n)`
```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        vector<int> temp;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                temp.push_back(nums[i]);  // 收集所有有效元素
            }
        }

        for (int i = 0; i < temp.size(); i++) {
            nums[i] = temp[i];  // 重新覆盖原数组
        }

        return temp.size();  // 返回新数组的有效长度
    }
};

```
### 方法2：双指针
1. **双指针法：**
    - `fast`：遍历数组，负责查找有效数字；
    - `slow`：标记新数组的位置，覆盖无效元素。
2. 如果 `nums[fast] != val`，则将 `nums[fast]` 移动到 `nums[slow]`，然后 `slow++`。
3. 遍历完成后，`slow` 的值就是新数组的长度。

时间复杂度： `O(n)`
空间复杂度：`O(1)`
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                nums[k++] = nums[i];
            }
        }
        return k;
    }
};
```


## 经验总结
- 当想要用额外空间解决问题的时候，想一想可不可以用双指针。
	- 一个指针指向原数组位置
	- 一个指针指向新数组位置。