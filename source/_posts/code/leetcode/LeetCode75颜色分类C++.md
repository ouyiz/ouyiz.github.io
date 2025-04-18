---
title: LeetCode75颜色分类
date: 2025-03-15 21:14:39
categories:
  - 力扣
tags:
  - 力扣
  - 代码
  - CPP
mathjax: true
---

# **问题描述**

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**原地**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。

**示例**

**输入:**

```cpp
nums = [2, 0, 2, 1, 1, 0]
```

**输出:**

```cpp
[0, 0, 1, 1, 2, 2]
```


# **解法**
使用三个指针：
- index0：0~(index0-1)存放的是0，也就是下一个0要放在index0
- index2：(index2+1)~n-1存放的是2，也就是下一个2要放在index2
- cur：当前遍历的元素位置

那么有三种情况需要考虑：
- ``nums[cur]``为0，将``nums[cur]与nums[index0]``交换，此时index0要加1。cur也要加1，因为在交换前，index要么等于cur，要么``nums[index]``为1，对于这两种情况，都需要加1来寻找下一个更新index0与index2的机会。
- ``nums[cur]``为1，直接cur+1，因为index0与index2都无法更新。
- ``nums[cur]``为2，将``nums[cur]与nums[index2]``交换，此时index2要减1。不需要将cur加1，因为我们不知道交换器``nums[index2]``的值是什么情况。

现在来思考while循环的终止条件：
- 两种可能``cur<index2``或者``cur<=index2``。因为index2是下一个要放2的位置，所以我们并不知道``nums[index2]``的值，所以应该选择对index2位置的值进行处理，也就是``cur<=index2``。

# **代码实现**

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int index0 = 0, index2 = n - 1, cur = 0;
        while (cur <= index2) {
            if (nums[cur] == 0) {
                swap(nums[cur], nums[index0]);
                index0++;
                cur++;
            }
            else if (nums[cur] == 2) {
                swap(nums[cur], nums[index2]);
                index2--;
            }
            else cur++;
        }
    }
};```

## **时间复杂度**

- 每个元素最多被交换一次，因此遍历数组的时间复杂度是 **O(n)**，其中 `n` 是数组的长度。

## **空间复杂度**

- 只使用了常数额外空间，因此空间复杂度是 **O(1)**。
