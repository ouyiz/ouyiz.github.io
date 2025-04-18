---
title: LeetCode背包专题C++
categories:
  - 力扣
tags:
  - 编程
  - 代码
  - 动态规划
  - CPP
mathjax: true
date: 2025-04-17 18:46:27
---

> 注意
> 以下内容理论部分有参考《代码随想录》，推荐看了代码随想录得背包问题相关内容后再刷代码。
> [代码随想录](https://www.programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)

![](file-20250410173038988.png)
# 0-1背包问题
## 理论基础
### 问题描述
一个正在抢劫商店的小偷发现了$n$个商品，第$i(范围为[0,n-1])$个商品价值`value[i]`美元，重`weight[i]`磅，`value[i]`和`weight[i]`都是整数。这个小偷希望拿走价值尽量高的商品，但他的背包最多能容纳$W$磅重的商品，$W$是一个整数。他应该拿哪些商品呢？
（我们称这个问题是0-1背包问题，因为对每个商品，小偷要么把它完整拿走，要么把它留下；他不能只拿走一个商品的一部分，或者把一个商品拿走多次。）

### 求解思路
#### 二维数组
**1.确定dp数组以及下标的含义**
`dp[i][j]` 表示从下标为`[0-i]`的物品里任意取，放进容量为`j`的背包，价值总和最大是多少。

**2.确定递推公式**
对于物体`i`只有两种情况
- 不放物品`i`：背包容量为`j`，里面不放物品`i`的最大价值是`dp[i - 1][j]`。
- 放物品`i`：背包空出物品`i`的容量后，背包容量为`j - weight[i]`，`dp[i - 1][j - weight[i]] `为背包容量为`j - weight[i]`且不放物品i的最大价值，那么`dp[i - 1][j - weight[i]] + value[i] `，就是背包放物品i得到的最大价值。
递归公式： `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);`

**3.dp数组如何初始化**
对于`i`：
	由递推公式的`dp[i-1][j]`知，需要考虑`dp[0][j]`的情况。
	当 `j < weight[0]`的时候，`dp[0][j] `应该是 0，因为背包容量比编号0的物品重量还小。
	当`j >= weight[0]`时，`dp[0][j] `应该是`value[0]`，因为背包容量放足够放编号0物品。

对于`j`：
	如果背包容量j为0的话，即`dp[i][0]`，无论是选取哪些物品，背包价值总和一定为0。

故初始化代码：
```C++
for (int i = 1; i < weight.size(); i++) {  
    dp[i][0] = 0;
}
for (int j = weight[0]; j <= bagweight; j++) {
    dp[0][j] = value[0];
}
```

**4.确定遍历顺序**
两种情况：
- 先`i`后`j`，先物品后背包重量
```C++
// weight数组的大小 就是物品个数
for(int i = 1; i < weight.size(); i++) { // 遍历物品
    for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    }
}
```
- 先`j`后`i`，先背包重量后物品
```C++
// weight数组的大小 就是物品个数
for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    }
}
```

#### 一维数组
**1.确定dp数组以及下标的含义**
`dp[j]`表示：容量为`j`的背包，所背的物品价值可以最大为`dp[j]`。

**2.确定递推公式**
对于物体``i``只有两种情况
- 不放物品`i`：背包容量为`j`，里面不放物品`i`的最大价值是`dp[j]`。
- 放物品`i`：背包空出物品`i`的容量后，背包容量为`j - weight[i]`，`dp[j - weight[i]] `为背包容量为`j - weight[i]`且不放物品i的最大价值，那么`dp[j - weight[i]] + value[i] `，就是背包放物品i得到的最大价值。
递归公式： `dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);`

**3.dp数组如何初始化**
对于`j`：
	如果背包容量j为0的话，即`dp[0]`，无论是选取哪些物品，背包价值总和一定为0。

故初始化代码：
```C++
dp[0]=0;
```

**4.确定遍历顺序**
两种情况：
- 先`i`后`j`，先物品后背包重量
	这里第二层循环背包重量是从大到小，为了保证物品`i`只被放入一次。
	因为如果是从小到大，假设对于物体$i_0$，它可能在$j=j_0$的时候满足条件，进入过一次背包，结果在$j=j_1(j_1>j_0)$的时候又满足条件，放进背包，这就不是01背包问题了。
```C++
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

- 先`j`后`i`，先背包重量后物品
	不可以！！！
	首先，要和上面一样，背包重量是从大到小。
	但是，这样的话，背包只能放一个物品。因为背包重量是从大到小，那` dp[j - weight[i]] + value[i]`其实用于是`0+value[i]`，也就是只能放一个物品。
	```C++
//不可以这样写！！！！
for(int j = bagWeight; j >= 0; j--) { // 遍历物品
    for(int i = 0; i < weight.size(); i++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

## LeetCode题目

### Leetcode 416. 分割等和子集

#### 题目描述

给你一个 **只包含正整数** 的非空数组 `nums`，判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

- **示例 1：**
    ```
    输入：nums = [1, 5, 11, 5]
    输出：true
    解释：数组可以分成 [1, 5, 5] 和 [11]，两个子集和相等。
    ```
- **示例 2：**
    ```
    输入：nums = [1, 2, 3, 5]
    输出：false
    ```


#### 思路
**转化为01背包问题：**
这个问题可以变成一个 01 背包问题：从 `nums` 中选出一部分数字，使得它们的和恰好是 `sum / 2`。
- 如果数组的总和是奇数，那就肯定不能划分。
- 如果是偶数，就尝试用动态规划找出是否存在子集和为 `sum / 2`。
需要找到一些数，使数的和为sum/2;

**动态规划步骤：**
1.确定dp数组以及下标的含义
`dp[j]`表示和是否可以组成`j`
2.确定递推公式
`if(dp[j-nums[i]]==true)dp[j]=true`
3.dp数组如何初始化
`dp[0]=true`
4.确定遍历顺序
```C++
for(int i=0;i<nums.size();i++)
   for(int j=sum/2;j>=nums[i];j--)
```

#### 代码

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            sum = sum + nums[i];
        }
        if (sum % 2 == 1)return false;
        int n = sum / 2;
        vector<bool> dp(n + 1, false);
        dp[0] = true;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = n; j >= nums[i]; j--) {
                if (dp[j - nums[i]] == true)dp[j] = true;
            }
        }
        return dp[n];
    }
};
```


### Leetcode 1049. 最后一块石头的重量 II

#### 题目描述

有一堆石头，用整数数组 `stones` 表示。其中 `stones[i]` 表示第 `i` 块石头的重量。

每一回合，从中选出 **任意两块** 石头，然后将它们一起粉碎。假设石头的重量分别为 `x` 和 `y`，且 `x <= y`。那么粉碎的可能结果如下：
- 如果两块石头重量相同，那么它们都会被完全粉碎；
- 如果两块石头重量不同，那么较重的石头会剩下重量为 `较重 − 较轻` 的新石头。

最后，最多剩下一块石头，返回这块石头的最小可能重量。如果没有石头剩下，就返回 `0`。

**示例：**

```
输入：stones = [2,7,4,1,8,1]
输出：1
解释：
    可能的一种方案是：
    - (2,4) -> 2
    - (1,1) -> 0
    - (2,7) -> 5
    - (5,8) -> 3
    - (3,0) -> 3
    最小的可能重量为 1
```

**提示：**
- `1 <= stones.length <= 30`
- `1 <= stones[i] <= 100`

#### 思路
**转化为01背包问题：**
- 设总重量为 `sum`，这道题的目的其实时尽可能将石头平均分成两堆，一堆小于等于`n/2`，一堆大于等于`n/2`。
- 所以本质就是一个 **01 背包问题**，目标是找到一组石头，它们的重量和 `s` 尽可能接近 `sum / 2`。

**动态规划步骤：**
1.确定dp数组以及下标的含义
`dp[j]`表示容量为 `j` 的背包最多能装多少重量的石头
2.确定递推公式
`dp[j] = max(dp[j], dp[j - stone] + stone)`
3.dp数组如何初始化
`vector<int> dp(target + 1, 0);`
4.确定遍历顺序
```C++
for(int i=0;i<nums.size();i++)
   for(int j=sum/2;j>=nums[i];j--)
```
#### 代码 

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = 0;
        for (int i = 0; i < stones.size(); i++) {
            sum = sum + stones[i];
        }
        int target = sum / 2;
        vector<int> dp(target + 1, 0);
        for (int i = 0; i < stones.size(); i++) {
            for (int j = target; j >= stones[i]; j--) {
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }
        return sum - 2 * dp[target];
    }
};
```


### LeetCode494. 目标和

#### 题目描述

给你一个整数数组 `nums` 和一个整数 `target`。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：
- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

 **示例 1：**
```
输入：nums = [1,1,1,1,1], target = 3  
输出：5  
解释：总共有 5 种方法让最终结果为 3：  
-1 +1 +1 +1 +1 = 3  
+1 -1 +1 +1 +1 = 3  
+1 +1 -1 +1 +1 = 3  
+1 +1 +1 -1 +1 = 3  
+1 +1 +1 +1 -1 = 3
```

示例 2：
```
输入：nums = [1], target = 1  
输出：1
```



#### 思路
**转化为01背包问题：**
设数组中所有数的和为 `sum`，我们要在这些数前加 + 或 -，使得总和为 `target`。
设加正号的那些数之和为 `P`，加负号的和为 `N`，则有：
```
P - N = target  
P + N = sum
```
解这个方程组得：
```
2P = target + sum  
=> P = (target + sum) / 2
```
也就是说问题就变成了：
**从数组中选出若干个数，使得它们的和为 (target + sum) / 2**，求这样的选法有多少种。

**动态规划步骤：**
1.确定dp数组以及下标的含义
`dp[i]`表示使得和为i的选法种数
2.确定递推公式
`dp[j] = dp[j] + dp[j - nums[0]];``
3.dp数组如何初始化
`vector<int> dp(weight + 1, 0);`
` dp[0] = 1;`
4.确定遍历顺序
```C++
for (int i = 0; i < nums.size(); i++) 
    for (int j = weight; j >= nums[0]; j--) 
```


#### 代码

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++)sum += nums[i];
        if ((target + sum) % 2 == 1||abs(target)>sum)return 0;
        int weight = (target + sum) / 2;
        vector<int> dp(weight + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = weight; j >= nums[i]; j--) {
               dp[j] = dp[j] + dp[j - nums[i]];
            }
        }
        return dp[weight];
    }
};
```


### LeetCode 474. 一和零

#### 题目描述

给你一个二进制字符串数组 `strs` 和两个整数 `m` 和 `n`。
请你找出并返回 **strs 的最大子集的大小**，其中 **最多有 `m` 个 0 和 `n` 个 1**。
如果 `x` 的所有元素也是 `y` 的元素，集合 `x` 是集合 `y` 的子集。

**示例 1：**

```
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3  
输出：4  
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"}，它们的大小是 4。
```

**示例 2：**

```
输入：strs = ["10", "0", "1"], m = 1, n = 1  
输出：2  
解释：最多可选 {"0", "1"}
```


#### 思路
**转化为01背包问题：**
- 背包容量是：`m` 个 0 和 `n` 个 1
- 物品是：每个字符串，它的“重量”是包含的 0 和 1 的个数
- “价值”是：选择这个字符串后的子集大小

**动态规划步骤：**
1.确定dp数组（dp table）以及下标的含义
`Dp[i][j]`表示最多由`i`个0和`j`个1时最大子集的长度。
2.确定递推公式
`Dp[i][j]=max(dp[i][j],Dp[i-m0][j-m1]+1);`
3.dp数组如何初始化
`vector<int> dp(m+1,vector<int>(n+1,0));`
4.确定遍历顺序
```C++
for(int k=0;k<strs.size();k++)
	for(int i=m;i>=m0;i++;)
		for(int j=n;j>=m1;j++)
```

#### 代码

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> strs01(strs.size(), vector<int>(2, 0));
        for (int i = 0; i < strs.size(); i++) {
            for (int j = 0; j < strs[i].size(); j++) {
                if (strs[i][j] == '0')strs01[i][0]++;
                if (strs[i][j] == '1')strs01[i][1]++;
            }
        }
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int k = 0; k < strs.size(); k++) {
            for (int i = m; i >= strs01[k][0]; i--) {
                for (int j = n; j >= strs01[k][1]; j--) {
                    dp[i][j] = max(dp[i][j], dp[i - strs01[k][0]][j - strs01[k][1]] + 1);

                }
            }
        }
        return dp[m][n];
    }
};
```

### 题目总结
#### 1. 确定 `dp` 数组含义
- 定义一个一维或二维 `dp` 数组。
- 下标 `dp[i]` 表示容量为 `i` 时，背包能达到的最大值、最小值、组合数或可行性等状态。
    - **Leetcode 416. 分割等和子集**：`dp[i]` 表示是否能通过某些物品的组合，恰好凑成重量为 `i` 的背包。
    - **Leetcode 1049. 最后一块石头的重量 II**：`dp[j]`表示容量为 `j` 的背包最多能装多少重量的石头
    - **Leetcode 494. 目标和**：`dp[i]` 表示能够通过某些数值组合，达到 `i` 这个目标和的情况数。
    - **Leetcode 474. 一和零**：`dp[i][j]` 表示能够使用 `i` 个零和 `j` 个一组成某个目标的子集数量。

#### 2. 确定递推公式
- **0-1背包**的核心特征是：**每个物品只能选择一次**。
- 递推公式常见形式：
    ```cpp
    dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);  // 求最大值
    dp[j] = min(dp[j], dp[j - weight[i]] + value[i]);  // 求最小值
    dp[j] = dp[j] + dp[j - weight[i]];  // 求组合数/方案数
    dp[j] = true if dp[j - weight[i]];  // 判断可行性
    ```

#### 3. 初始化
根据问题类型，初始化方式有所不同：

| 问题类型  | 初始化方式                       |
| ----- | --------------------------- |
| 求最小值  | `dp[0] = 0`, 其他为 `INT_MAX`  |
| 求最大值  | `dp[0] = 0`, 其他为 `INT_MIN`  |
| 判断可行性 | `dp[0] = true`, 其他为 `false` |
| 求方案数  | `dp[0] = 1`, 其他为 `0`        |


#### 4. 遍历顺序
- **物品在外层，容量在内层**：从后往前遍历，避免重复计算。
    ```cpp
    for (int i = 0; i < n; i++) {
        for (int j = capacity; j >= weight[i]; j--) {
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    ```
 
# 完全背包问题
## 理论基础
### 问题描述
有`N`件物品和一个最多能背重量为`W`的背包。第i件物品的重量是`weight[i]`，得到的价值是`value[i]` 。**每件物品都有无限个（也就是可以放入背包多次）**，求解将哪些物品装入背包里物品价值总和最大。

**完全背包和01背包问题唯一不同的地方就是，每种物品有无限件**。

### 求解思路
#### 二维数组
**1.确定dp数组以及下标的含义**
`dp[i][j] `表示从下标为`[0-i]`的物品，每个物品可以取无限次，放进容量为`j`的背包，价值总和最大是多少。

**2.确定递推公式**
对于物体`i`只有两种情况
- 不放物品`i`：背包容量为`j`，里面不放物品`i`的最大价值是`dp[i - 1][j]`。
- 放物品`i`：背包空出物品`i`的容量后，背包容量为`j - weight[i]`，`dp[i][j - weight[i]] `为背包容量为`j - weight[i]`且不放物品i的最大价值，那么`dp[i][j - weight[i]] + value[i] `，就是背包放物品i得到的最大价值。
递归公式： `dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);`
也可以是：`dp[i][j] = max(dp[i][j], dp[i][j - weight[i]] + value[i]);`

==注意：01背包中是 `dp[i - 1][j - weight[i]] + value[i])`==

**3.dp数组如何初始化**
对于`i`：
	不需要考虑
对于`j`：
	如果背包容量j为0的话，即`dp[i][0]`，无论是选取哪些物品，背包价值总和一定为0。
```C++
// 初始化 dp
vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
for (int j = weight[0]; j <= bagWeight; j++) {
    dp[0][j] = dp[0][j - weight[0]] + value[0]; 
}
```

**4.确定遍历顺序**
 先遍历物品再遍历背包：
 ```C++
for (int i = 1; i < n; i++) { // 遍历物品
    for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);
    }
}
```
先遍历背包再遍历物品：
```C++
for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
    for (int i = 1; i < n; i++) { // 遍历物品
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i][j - weight[i]] + value[i]);
    }
}
```

#### 一维数组
**1.确定dp数组以及下标的含义**
`dp[j]`表示：容量为`j`的背包，所背的物品价值可以最大为`dp[j]`。

**2.确定递推公式**
对于物体``i``只有两种情况
- 不放物品`i`：背包容量为`j`，里面不放物品`i`的最大价值是`dp[j]`。
- 放物品`i`：背包空出物品`i`的容量后，背包容量为`j - weight[i]`，`dp[j - weight[i]] `为背包容量为`j - weight[i]`且不放物品i的最大价值，那么`dp[j - weight[i]] + value[i] `，就是背包放物品i得到的最大价值。
递归公式： `dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);`

**3.dp数组如何初始化**
对于`j`：
	如果背包容量j为0的话，即`dp[0]`，无论是选取哪些物品，背包价值总和一定为0。

故初始化代码：
```C++
  vector<int> dp(bagWeight + 1, 0);
```

**4.确定遍历顺序**
先遍历背包再遍历物品：
```C++
for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        if (j - weight[i] >= 0) dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
    cout << endl;
}
```

先遍历物品再遍历背包：
```C++
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
        if (j - weight[i] >= 0) dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

注：
对于纯完全背包问题，其for循环的先后循环是可以颠倒的！
但如果题目稍稍有点变化，就会体现在遍历顺序上。
如果问装满背包有几种方式的话？ 那么两个for循环的先后顺序就有很大区别了，而leetcode上的题目都是这种稍有变化的类型。

## LeetCode题目
### LeetCode518.零钱兑换II
#### 题目描述
给你一个整数数组 `coins` 表示不同面额的硬币，另给一个整数 `amount` 表示总金额。
请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 `0` 。
假设每一种面额的硬币有无限个。 
题目数据保证结果符合 32 位带符号整数。

**示例 1：**
```
输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```


**示例 2：**
```
输入: amount = 3, coins = [2]
输出: 0
```

#### 思路
**转化为完全背包问题：**
- 每种硬币数量无限，目标是找到组合数，使它们的和恰好等于 `amount`。
- 硬币顺序不同视为同一种组合
所以本质上是完全背包问题（物品：硬币，容量：目标金额）。
- 这里和完全背包不同的是，这道题求的是方案数，而完全背包问题求得是最大价值。
- ==对于完全背包问题，物品顺序没有影响，所以遍历顺序也没有影响。==
- ==对于这道题，因为求的是方案数，所以遍历顺序有影响。
- ==因为硬币顺序不同是同一组合，此时只能先遍历物品再遍历容量。==


**动态规划解题步骤：**
1.确定 `dp` 数组及含义
`dp[j]` 表示：  凑出金额 j 所有组合的方案数。
2.确定递推公式
对于每一个硬币 `coin`：
```C++
dp[j] = dp[j] + dp[j - coin]
```
含义：
- 如果不选 `coin`，方案数是 `dp[j]`（原来的）。
- 如果选 `coin`，剩下的金额是 `j - coin`，方案数是 `dp[j - coin]`。
3.dp数组如何初始化
必须初始化：
```C++
dp[0] = 1
```
意思是：金额为 `0` 时，不选任何硬币也算一种组合。
其他：
```C++
dp[i] = 0
```
表示还未计算。
4.确定遍历顺序
- 外层：遍历每一个硬币（保证组合顺序不重复，避免算重）。
- 内层：遍历每一个金额 `j`，从小到大更新（因为完全背包，允许硬币重复选）。
```C++
for (int i = 0; i < coins.size(); i++) 
            for (int j = coins[i]; j <= amount; j++) 
```

#### 代码
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        //防溢出
        vector<unsigned int> dp(amount + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < coins.size(); i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] = dp[j] + dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
};
```

### LeetCode377.组合总和IV
#### 题目
给你一个由 **不同** 整数组成的数组 `nums` ，和一个目标整数 `target` 。请你从 `nums` 中找出并返回总和为 `target` 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

**示例 1：**
```
输入：nums = [1, 2, 3], target = 4  
输出：7

解释：
所有可行的组合有：
(1, 1, 1, 1)  
(1, 1, 2)  
(1, 2, 1)  
(1, 3)  
(2, 1, 1)  
(2, 2)  
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```
**示例 2：**
```
输入：nums = [9], target = 3  
输出：0

解释：  
9 > 3，无法组成 target = 3，结果为0。
```
**提示：**
- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- `nums` 中的所有元素互不相同。
- `1 <= target <= 1000`

#### 思路
**转化为完全背包问题：**
- 每个数字可以重复使用，目标是找到组合数，使它们的和恰好等于 `target`。
- 数字顺序不同视为不同组合（重要）。
所以本质上是**完全背包问题**（物品：数字，容量：`target`）。
- 这里和完全背包不同的是，这道题求的是最少的硬币个数，而完全背包问题求得是最大价值。
- 对于完全背包问题，物品顺序没有影响，所以遍历顺序也没有影响。
- ==对于这道题，因为求的是方案数，所以遍历顺序有影响。
- ==因为数字顺序不同是不同组合，此时只能先遍历容量再遍历物品。==


**动态规划解题步骤：**
1.确定 `dp` 数组及含义
`dp[j]` 表示：  凑成总和` j `的排列组合个数。
2.确定递推公式
对于每个目标总和 `j`，遍历 `nums` 中每一个数：
```C++
dp[j] = dp[j] + dp[j - nums[i]]
```
含义：
- 如果不选 `nums[i]`，方案数是 `dp[j]`（原来的）。
- 如果选 `nums[i]`，剩下的是 `j - nums[i]`，方案数是 `dp[j - coin]`。
3.dp数组如何初始化
必须初始化：
```C++
dp[0] = 1
```
意思是：什么都不选，只有一种组合。
其他：
```C++
dp[i] = 0
```
表示还未计算。
4.确定遍历顺序
- - 外层遍历**容量 `j`**，从 `0` 到 `target`；
- 内层遍历**物品 `num`**，考虑每个可用的数字。
```C++
for (int j = 0; j <= target; j++) {
    for (int i = 0; i < nums.size(); i++) {
        if (j >= nums[i]) {
            dp[j] += dp[j - nums[i]];
        }
    }
}
```

#### 代码
```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned int> dp(target + 1, 0);
        dp[0] = 1;
        for (int j = 0; j <= target; j++) {
            for (int i = 0; i < nums.size(); i++) {
                if (j >= nums[i])dp[j] = dp[j] + dp[j - nums[i]];
            }
        }
        return dp[target];
    }
};
```

### LeetCode322.零钱兑换
#### 题目描述：
给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。
计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。
你可以认为每种硬币的数量是无限的。

**示例 1：**
```
输入：coins = [1, 2, 5], amount = 11
输出：3
解释：11 = 5 + 5 + 1
```
**示例 2：**
```
输入：coins = [2], amount = 3
输出：-1
```
**示例 3：**
```
输入：coins = [1], amount = 0
输出：0
```
**提示：**
- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

#### 思路
**转化为完全背包问题：**
- 每个银币可以重复使用，目标是找到最少的硬币个数，使它们的和恰好等于 `amount`。
- 物品顺序没有影响
所以本质上是**完全背包问题**（物品：硬币，容量：目标金额）。
- 这里和完全背包不同的是，这道题求的是最少的硬币个数，而完全背包问题求得是最大价值。
- 对于完全背包问题，物品顺序没有影响，所以遍历顺序也没有影响。
- ==对于这道题，因为求的是最少的硬币个数，所以遍历顺序没有影响。==


**动态规划解题步骤：**
1.确定 `dp` 数组及含义
`dp[j]` 表示：  组成金额 `j` 所需的**最少硬币数**。
2.确定递推公式
对于每一个硬币 `coins[i]`，可以选择用或不用：
```C++
dp[j] = min(dp[j], dp[j - coins[i]] + 1)
```
3.dp数组如何初始化
```C++
//因为是求最小值，先把 dp数组全初始化为一个最大值，表示尚未计算
vector<int> dp(amount + 1, amount + 1); 
//凑出金额0，硬币数量为0
dp[0] = 0
```
4.确定遍历顺序
都可以：
- 外层遍历每个硬币，内层遍历从小到大遍历金额。
- 内层遍历每个硬币，外层遍历从小到大遍历金额。
```C++
for (int i = 0; i < coins.size(); i++) {   // 遍历物品（硬币）
            for (int j = coins[i]; j <= amount; j++) {  // 遍历背包容量
                dp[j] = min(dp[j], dp[j - coins[i]] + 1);
            }
        }
```

#### 代码：

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int i = 0; i < coins.size(); i++) {
            for (int j = coins[i]; j <= amount; j++) {
                dp[j] = min(dp[j], dp[j - coins[i]] + 1);
            }
        }
        if (dp[amount] == amount + 1) return -1;
        else return dp[amount];
    }
};
```


### LeetCode279.完全平方数
#### 题目
给你一个整数 `n` ，返回 _和为 `n` 的完全平方数的最少数量_ 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

示例1：
```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

示例2：
```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

提示：
- `1 <= n <= 104`

#### 思路
**转化为完全背包问题：**
- 每个数字1², 2², 3², 4²,...，可以重复使用，目标是最少数量的完全平方数，组成总和为 `n`。
- 物品顺序没有影响
所以本质上是**完全背包问题**（物品：数字，容量：n）。
- 这里和完全背包不同的是，这道题求的是最少数量，而完全背包问题求得是最大价值。
- 对于完全背包问题，物品顺序没有影响，所以遍历顺序也没有影响。
- ==对于这道题，因为求的是最少的数量，所以遍历顺序没有影响。==


**动态规划解题步骤：**
1.确定 `dp` 数组及含义
`dp[j]` 表示：  和为 `j` 时，最少需要多少个完全平方数
2.确定递推公式
对于每一个完全平方数 `i*i`，可以选择放或不放：
```C++
dp[j] = min(dp[j], dp[j - i*i] + 1);
```
3.dp数组如何初始化
```C++
//因为是求最小值，先把 dp数组全初始化为一个最大值，表示尚未计算
vector<int> dp(n + 1, n + 1); 
//和为0，不需要任何数
dp[0] = 0
```
4.确定遍历顺序
都可以：
- 外层遍历每个数字，内层遍历从小到大遍历容量。
- 内层遍历每个容量，外层遍历从小到大遍历数字。
```C++
for (int i = 1; i * i <= n; i++) {        // 遍历物品，完全平方数
    for (int j = i * i; j <= n; j++) {    // 遍历背包容量
        dp[j] = min(dp[j], dp[j - i * i] + 1);
    }
}
```

#### 代码

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, n + 1);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) {
            for (int j = i * i; j <= n; j++) {
                dp[j] = min(dp[j], dp[j - i * i] + 1);
            }
        }
        return dp[n];
    }
};
```

### LeetCode139.单词拆分

#### 题目
给你一个字符串 `s` 和一个字符串列表 `wordDict` 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 `s` 则返回 `true`。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

示例1：
```txt
输入：s = "leetcode", wordDict = ["leet", "code"]
输出：true
解释：返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```
示例2：
```txt
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```
示例3：
```txt
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```
提示：
- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` 和 `wordDict[i]` 仅由小写英文字母组成
- `wordDict` 中的所有字符串 **互不相同**

#### 思路
**转化为完全背包问题：**
- 给定一个字符串 s（背包容量），字典单词 wordDict（物品集合，每个单词可以重复使用），判断是否能拼成 s。
- 数值顺序不同视为相同同组合（重要）。
所以本质上是**完全背包问题**（物品：单词，容量：`s`）。
- 这里和完全背包不同的是，这道题求的是能否组成，而完全背包问题求得是最大价值。
- 对于完全背包问题，物品顺序没有影响，所以遍历顺序也没有影响。
- ==对于这道题，因为求的是能否组成，所以遍历顺序没有影响。==


**动态规划解题步骤：**
1.确定 `dp` 数组及含义
`dp[i]`：表示 `s[0, i-1]`（前i个字符）能否由字典中的单词拼接而成。  
`dp[i] = true`，表示可以拆分。`false` 表示不行。
2.确定递推公式
对于每个 `word`，如果 `s` 在位置 `i - word.size()` 到 `i` 这个子串等于 `word`，且 `dp[i - word.size()] == true`，则说明 `dp[i] = true`。
```C++
if (dp[i - word.size()] && s.substr(i - word.size(), word.size()) == word) {
    dp[i] = true;
}
```
3.dp数组如何初始化
```C++
vector<bool> dp(s.size() + 1, false);
dp[0] = true
```
5.确定遍历顺序
都可以：
- 外层遍历背包容量（从1到`s.size()`），内层遍历`wordDict`（推荐）
- 内层遍历背包容量（从1到`s.size()`），外层遍历`wordDict`
```C++
for (int i = 1; i <= s.size(); i++) {
    for (const string& word : wordDict) {
        if (i >= word.size() && dp[i - word.size()] && s.substr(i - word.size(), word.size()) == word) {
            dp[i] = true;
            break;
        }
    }
}
```


#### 代码

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for (int i = 1; i <= s.size(); i++) {
            for (int j = 0; j < wordDict.size(); j++) {
                if (i >= wordDict[j].size() &&
                    dp[i - wordDict[j].size()]
                    && s.substr(i - wordDict[j].size(), wordDict[j].size()) == wordDict[j]) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.size()];
    }
};

```

### 题目总结

#### 1.确定 `dp` 数组含义
- 定义一个一维/二维 `dp` 数组。
- 下标 `dp[i]` 表示 容量为 i 时，背包能达到的最大/最小/可能性等状态。
- 示例：
	- **Leetcode 518 零钱兑换 II**：`dp[i]` 表示总金额为 `i` 时的组合数（不同的顺序视为相同的情况）。
	- **Leetcode377 组合总和IV：**`dp[i]` 表示总和` i `的排列个数（不同的顺序视为不同的情况）。
    - **Leetcode 322 零钱兑换**：`dp[i]` 表示金额为 `i` 时所需最少硬币数。
    - **Leetcode 279 完全平方数：**`dp[i]`: 和为` i` 所需的最少平方数个数
    - **Leetcode 139 单词拆分**：`dp[i]` 表示前 `i` 个字符能否被字典中的单词拼出。

#### 2.确定递推公式
- **完全背包**的核心特征是：**每个物品可以选多次**。
- 所以公式一般形如：
    ```cpp
    dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);    // 求最大值
    dp[j] = min(dp[j], dp[j - weight[i]] + value[i]);    // 求最小值
    dp[j] =dp[j] + dp[j - coins[i]];                           // 求组合数/方案数
    dp[j] = true if dp[j - word.size()] && s.substr(...) // 判断可行性
    ```


#### 3.初始化
根据问题不同，初始化有所不同：

| 问题类型   | 初始化方式                      |
| ------ | -------------------------- |
| 求最小值   | `dp[0] = 0`, 其他为 `INT_MAX` |
| 求最大值   | `dp[0] = 0`, 其他为 `INT_MIN` |
| 判断是否可行 | `dp[0] = true`,其他为`False`  |
| 求组合方案数 | `dp[0] = 1`,其他为`0`         |


####  4.遍历顺序
- **物品在外层，容量在内层**：适用于求组合数。
    ```cpp
    for (int i = 0; i < n; i++) {
        for (int j = coins[i]; j <= amount; j++) {
            dp[j] += dp[j - coins[i]];
        }
    }
    ```
- **容量在外层，物品在内层**：适用于求排列数或状态是否可达。
    ```cpp
    for (int j = 1; j <= amount; j++) {
        for (int i = 0; i < coins.size(); i++) {
            if (j >= coins[i]) dp[j] += dp[j - coins[i]];
        }
    }
    ```
    
