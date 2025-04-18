---
title: LeetCode300最长递增子序列C++（两种解法）
categories:
  - 力扣
tags:
  - 动态规划
  - 贪心
  - CPP
  - 编程
  - 代码
mathjax: true
date: 2025-04-05 17:16:46
---

## 题目

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的某些元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。


**示例 1：**
```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```
**示例 2：**
```
输入：nums = [0,1,0,3,2,3]
输出：4
```
 **示例 3：**
```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

 **提示：**
- `1 <= nums.length <= 2500`
- `-10⁴ <= nums[i] <= 10⁴`

**进阶：**

你能将算法的时间复杂度降低到 O(n log n) 吗？

## 解法一：动态规划
这道题的目标是找出一个数组中最长的递增子序列的长度，子序列的意思是可以不连续，只要顺序保持不变。

**动态规划的方法：**
1. 用数组`dp`，其中 `dp[i]` 表示以第 `i` 个数结尾的最长递增子序列的长度。
2. 初始时，每个位置的 `dp[i]` 都设为 1，表示最小长度是它自己。
3. 然后我们遍历数组，对于每个 `nums[i]`，再往前找所有的 `nums[j]（j < i）`，如果 `nums[j]` 小于 `nums[i]`，就说明 `nums[i]` 可以接在 `nums[j]` 的后面，这时候就可以更新 `dp[i] = max(dp[i], dp[j] + 1)`。
4. 最终，我们从 dp 数组中找出最大的值，就是整个数组的最长递增子序列的长度。

**时间复杂度：**
时间复杂度是 O(n²)，因为有两层循环。
``` c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        int res = 1;
        vector<int> dp(n, 1);
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                    res = max(res, dp[i]);
                }
            }
        }
        return res;
    }
};
```

##  解法二：贪心 + 二分查找
**解法：**
可以使用 **贪心 + 二分查找** 来实现，将问题转换为：
- 我们维护一个数组 `sub`，`sub[i]` 表示长度为 `i+1` 的递增子序列结尾的最小元素；
- 对于每个数字 `num`，我们使用 **二分查找** 找到 `sub` 中第一个 ≥ `num` 的位置：
    - 如果找不到，说明 `num` 比所有元素都大，直接 `push_back`；
    - 如果找到了，就替换掉原位置的值，让结尾更小，以便构造更优的子序列。
**时间复杂度：**
- 每个元素最多执行一次二分查找：**O(log n)**
- 总共 `n` 个元素：**O(n log n)**
**示例：**
```
输入：nums = [10, 9, 2, 5, 3, 7, 101, 18]
过程如下：
1. 初始：tail = []
2. num=10，tail为空，加入：tail = [10]
3. num=9，小于10，用9替换10：tail = [9]
4. num=2，小于9，用2替换9：tail = [2]
5. num=5，大于2，加入：tail = [2,5]
6. num=3，替换5：tail = [2,3]
7. num=7，加入：tail = [2,3,7]
8. num=101，加入：tail = [2,3,7,101]
9. num=18，替换101：tail = [2,3,7,18]
最终 tail 的长度是 4，表示最长递增子序列的长度是 4。
```

**代码：**
二分查找部分类似LeetCode35搜索插入位置
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> sub;
        for (int i = 0; i < nums.size(); i++) {
            int left = 0, right = sub.size() - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (sub[mid] < nums[i]) {
                    left = mid + 1;
                }
                else {
                    right = mid - 1;
                }
            }
            if (left == sub.size()) {
                sub.push_back(nums[i]);
            }
            else {
                sub[left] = nums[i];
            }
        }
        return sub.size();
    }
};
```