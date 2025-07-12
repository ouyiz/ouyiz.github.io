---
title: LeetCode双指针专题-一个原数组，一个新数组
categories:
  - 力扣
tags:
  - 编程
  - 代码
  - 双指针
  - CPP
mathjax: true
date: 2025-04-22 17:01:55
---
# 相关题目
## LeetCode27.移除元素

### 题目
给你一个数组 `nums` 和一个值 `val`，你需要原地移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为
`k`，要通过此题，您需要执行以下操作：

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


### 解法
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
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast++) {
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
        }
        return slow;
    }
};
```






## LeetCode26.删除有序数组中的重复项
### 题目

给你一个 **非严格递增排列** 的数组 `nums` ，请你原地删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。


**示例 1：**
```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2，并且原数组 nums 的前两个元素被修改为 1 和 2。无需考虑数组中超出新长度后面的元素。
```

**示例 2：**
```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4,_,_,_,_,_]
```


**提示：**
- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非严格递增** 排列

### 解法
考虑到题目中提及是有序数组，所有可以使用下述方法
1. **双指针法：**
    - `fast`：遍历数组，查找新数组的元素；
    - `slow`：指向新数组的结尾元素。
2. 如果 `nums[fast] != nums[slow]`，就说明找到了新的元素，`slow++`，然后将将 `nums[fast]` 移动到 `nums[slow]`
3. 遍历完成后，`slow + 1` 的值就是新数组的长度。

时间复杂度： `O(n)`
空间复杂度：`O(1)`
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slow = 0;//新数组的末尾
        for (int fast = 1; fast < nums.size(); fast++) {
            if (nums[fast] != nums[slow]) {//找到新元素
                slow++;
                nums[slow] = nums[fast];
            }
        }
        return slow + 1;
    }
};
```


## LeetCode80.删除有序数组中的重复项
### 题目
给你一个有序数组 `nums` ，请你原地删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

 **示例 1：**
```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新的长度 5，并且前五个元素为：1, 1, 2, 2, 3。
函数返回的值和 nums 中元素的前五个值都正确即可，后面剩下的元素可以忽略。
```

**示例 2：**
```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
```

**提示：**
- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按升序排列

### 解法
考虑到题目中提及是有序数组，所有可以使用下述方法
1. **双指针法：**
    - `fast`：遍历数组，查找新数组的元素；
    - `slow`：指向新数组的结尾元素。
2. 找到新元素的两种情况
	- `nums[slow] != nums[fast]`，此时不等于新数组的结尾元素，符合
	- `nums[slow] == nums[fast]&& nums[slow] != nums[slow - 1]`，此时等于新数组的结尾元素，那这个元素只能数新数组中出现两次的最后一次

时间复杂度： `O(n)`
空间复杂度：`O(1)`
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() < 2)return nums.size();
        int slow = 1;
        for (int fast = 2; fast < nums.size(); fast++) {
            if ((nums[slow] != nums[fast]) || 
                (nums[slow] == nums[fast] && nums[slow] != nums[slow - 1])) {
                nums[++slow] = nums[fast];
            }
        }
        return slow + 1;
    }
};

```


# 经验总结
- 当想要用额外空间解决问题的时候，想一想可不可以用双指针。
	- 一个指针指向原数组位置
	- 一个指针指向新数组位置