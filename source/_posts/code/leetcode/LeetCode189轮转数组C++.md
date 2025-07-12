---
title: LeetCode189轮转数组C++
categories:
  - 力扣
tags:
  - 编程
  - 代码
  - 数组
  - CPP
mathjax: true
date: 2025-04-23 12:51:49
---

### 题目
给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。
**示例 1:**
```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```
**示例 2:**
```
输入: nums = [-1,-100,3,99], k = 2
输出: [3,99,-1,-100]
```

**提示：**
- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

### 解法一：反转法（O(n) 时间 + O(1) 空间）

#### 思路：
先将整个数组反转，然后分别反转前 `k` 个和后 `n-k` 个元素。
比如： 原始数组：`[1, 2, 3, 4, 5, 6, 7]`  
步骤：
1. 整体反转 → `[7,6,5,4,3,2,1]`
2. 前 `k=3` 个反转 → `[5,6,7,4,3,2,1]`
3. 后 `n-k=4` 个反转 → `[5,6,7,1,2,3,4]`

#### 代码：
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        reverse(nums.begin(), nums.end());
        reverse(nums.begin(), nums.begin() + k);
        reverse(nums.begin() + k, nums.end());
    }
};
```


### 解法二：使用额外数组（O(n) 时间 + O(n) 空间）
#### 思路：
我们想把数组向右轮转 `k` 个位置，也就是说，**每个元素的新位置是原位置加上 `k`，然后对数组长度 `n` 取模**，这样就不会越界。
#### 代码：
```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> temp(n);
        for (int i = 0; i < n; ++i) {
            temp[(i + k) % n] = nums[i];
        }
        nums = temp;
    }
};
```
