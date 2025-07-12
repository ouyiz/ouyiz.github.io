---
title: LeetCode88. 合并两个有序数组C++
categories:
  - 力扣
tags:
  - 代码
  - 编程
  - 数组
  - CPP
mathjax: true
date: 2025-04-18 15:43:45
---
## 题目 
给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意**：最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。


**示例：**

```text
输入：
nums1 = [1,2,3,0,0,0], m = 3  
nums2 = [2,5,6], n = 3

输出：
[1,2,2,3,5,6]
```


## 答案
### 方法1：笨方法
最简单粗暴的方法就是用一个数组来
存放nums1，将nums1当作最终结果数组。

时间复杂度：O(m + n)
空间复杂度：O(m)
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        vector<int> nums(nums1.begin(), nums1.begin() + m);
        int i = 0;
        int j = 0;
        int cur = 0;
        while (i < m && j < n) {
            if (nums[i] < nums2[j]) {
                nums1[cur] = nums[i];
                i++;
            }
            else {
                nums1[cur] = nums2[j];
                j++;
            }
            cur++;
        }
        if (i < m) {
            for (; i < m; i++) {
                nums1[cur] = nums[i];
                cur++;
            }
        }
        else {
            for (; j < n; j++) {
                nums1[cur] = nums2[j];
                cur++;
            }
        }
    }
};
```
### 方法2：从后往前
由于 `nums1` 的末尾预留了足够空间，可以采用**从后往前**的双指针策略，这样就不会覆盖掉还没处理的元素！
- `p1` 指向 `nums1` 的有效部分末尾（`m-1`）。
- `p2` 指向 `nums2` 的末尾（`n-1`）。
- `p` 指向 `nums1` 最后的填充位置（`m + n - 1`）。
依次比较 `nums1[p1]` 和 `nums2[p2]`，将较大的数放到 `nums1[p]`，指针相应后移，直到 `nums2` 全部处理完。
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int cur = m + n - 1;
        while (i >= 0 && j >= 0) {
            if (nums1[i] > nums2[j]) {
                nums1[cur] = nums1[i];
                i--;
            }
            else {
                nums1[cur] = nums2[j];
                j--;
            }
            cur--;
        }
        while (j >= 0) {
            nums1[cur] = nums2[j];
            j--;
            cur--;
        }
    }
};

```

## 经验总结
- 要考虑正向填充和反向填充
- 当想要用额外空间解决问题的时候，想一想可不可以用双指针。
	- 一个指针指向原数组位置
	- 一个指针指向新数组位置。