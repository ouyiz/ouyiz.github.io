---
title: LeetCode买卖股票专题C++
categories:
  - 力扣
tags:
  - 编程
  - 代码
  - 服务器
  - CPP
mathjax: true
date: 2025-04-27 13:30:42
---
## LeetCode 121买卖股票的最佳时机
### 题目
给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。
你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1:**
```
输入: prices = [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天买入（价格=1），在第 5 天卖出（价格=6），利润 = 6-1 = 5。
     注意不能在第 1 天买入然后第 2 天卖出。
```

**示例 2:**
```
输入: prices = [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成，最大利润为 0。
```

**提示：**
- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`


### 解法一：一次遍历，维护最小价格
**题目重点：**
最多只能买一支股票

**思路**
所以我们从左到右遍历，记录“当前最小的买入价格”，然后计算每一天“如果今天卖出，能赚多少”。每次更新最大利润即可。
也就是需要两个变量：
- 一个记录当前最低买入价格
- 一个记录当前最大利润

**代码**
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int min_price = prices[0];
        int max_profit = 0;
        for (int i = 1; i < prices.size(); i++) {
            max_profit = max(max_profit, prices[i] - min_price);
            min_price = min(min_price, prices[i]);
        }
        return max_profit;
    }
};
```

**时间和空间复杂度**
- 时间复杂度：`O(n)` —— 只遍历一次数组
- 空间复杂度：`O(1)` —— 只使用了两个变量

### 解法二：动态规划
**题目重点：**
最多只能买一支股票

**思路：**
1.确定dp数组以及下标的含义
- `dp[i][0]`：第 i 天 **不持有** 股票的最大收益。
- `dp[i][1]`：第 i 天 **持有** 股票的最大收益。
2.确定递推公式
- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);`
    要么今天什么都不做，要么今天卖出
- `dp[i][1] = max(dp[i-1][1], -prices[i]);` 
    要么今天什么都不做，要么今天买入
3.dp数组如何初始化
- `vector<vector<int>> dp(n, vector<int>(2, 0));`表示当天收益为0
- `dp[0][0] = 0`，第0天没持股
- `dp[0][1] = -prices[0]`，第0天买了股票
4.确定遍历顺序
- ` for (int i = 1; i < prices.size(); i++)`

**代码：**
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // 初始化 dp 数组
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0)); 
        dp[0][1] = 0;           // 第一天不持有股票，最大利润为 0
        dp[0][1] = -prices[0];  // 第一天持有股票，最大利润为负值（买入）
        for (int i = 1; i < n; ++i) {
            // 不持有股票的最大利润：要么前一天就不持有，要么今天卖出
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
            // 持有股票的最大利润：要么前一天就持有，要么今天买入
            dp[i][1] = max(dp[i-1][1], -prices[i]);
           
        }
        return dp[n-1][0];
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n)
- 空间复杂度：O(n)
## LeetCode122买卖股票的最佳时机II
### 题目
给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。
在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。(你可以在同一天买入并卖出一支股票，系统认为这是一笔完整交易，合法且有效)
返回 _你能获得的 **最大** 利润_ 。

**示例 1：**
```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天买入（价格=1），第 3 天卖出（价格=5），利润=4。
      接着在第 4 天买入（价格=3），第 5 天卖出（价格=6），利润=3。
      总利润 = 4 + 3 = 7。
```

**示例 2：**
```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天买入，在第 5 天卖出，利润=4。
```

**示例 3：**
```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下，无法完成任何交易，利润为 0。
```


### 解法一：贪心算法
**题目重点：**
可以多次买卖，甚至当天买卖，但手里只能有一支股票

**思路：** 
只要今天的价格比昨天高，就在昨天买入、今天卖出。即：只要有利润就卖出。
- 我们不关心买入卖出是哪一天，只要能赚钱就赚。
- 实际上，这相当于把所有的上升区间的利润都累加起来。
<img src="file-20250424212231625.png" width=300px>

**代码：**
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] > prices[i - 1]) {
                res = res + prices[i] - prices[i - 1];
            }
        }
        return res;
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n)
- 空间复杂度：O(1)



### 解法二：动态规划，每天持有/不持有两种状态
**题目重点：**
可以多次买卖，甚至当天买卖，但手里只能有一支股票

**思路：**
1.确定dp数组以及下标的含义
- `dp[i][0]`：第 i 天 **不持有** 股票的最大收益。
- `dp[i][1]`：第 i 天 **持有** 股票的最大收益。
2.确定递推公式
- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])`  
    要么今天什么都不做，要么今天卖出
- `dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])`  
    要么今天什么都不做，要么今天买入
3.dp数组如何初始化
- `vector<vector<int>> dp(n, vector<int>(2, 0));`表示当天收益为0
- `dp[0][0] = 0`，第0天没持股
- `dp[0][1] = -prices[0]`，第0天买了股票
4.确定遍历顺序
- ` for (int i = 1; i < prices.size(); i++)`

**代码：**
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        //dp[i][0]：第 i 天 不持有股票的最大收益
        //dp[i][1]：第 i 天 持有股票的最大收益。
        vector<vector<int>> dp(prices.size(), vector<int>(2, 0));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        // 最后一天不持股为最大利润
        return dp[prices.size() - 1][0];
    }
};
```
也可以使用下面的代码
两种可能：
	`dp0=dp0`：此时`dp0`已经是前一天的`dp0`
	`dp0=dp1+prices[i]`：此时当天进行了卖出操作，但是可以当天买入卖出，所以合理
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int dp0 = 0; //不持有股票
        int dp1 = -prices[0]; //持有股票
        for (int i = 1; i < prices.size(); i++) {
            dp0 = max(dp0, dp1 + prices[i]);
            dp1 = max(dp1, dp0 - prices[i]);
        }
        return dp0;
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n)
- 空间复杂度：O(n)

### 解法三：动态规划 + 状态压缩（滚动数组）
**题目重点：**
可以多次买卖，甚至当天买卖，但手里只能有一支股票

**思路：**
只保留前一天的两个状态，不用数组，节省空间。
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int dp0 = 0;
        int dp1 = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            int new_dp0 = max(dp0, dp1 + prices[i]);
            int new_dp1 = max(dp1, dp0 - prices[i]);
            dp0 = new_dp0;
            dp1 = new_dp1;
        }
        // 最后一天不持股为最大利润
        return dp0;
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n) 
- 空间复杂度：O(1)

## LeetCode123买卖股票的最佳时机III
### 题目
给定一个数组 `prices`，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。
设计一个算法来计算你所能获取的 **最大利润**。你最多可以完成 **两笔交易**。
**注意：**  你 **不能同时参与多笔交易**（也就是说，你必须在再次购买前卖出股票）。

**示例 1：**
```
输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（价格 = 0）买入，在第 6 天（价格 = 3）卖出，利润 = 3 - 0 = 3。  
     随后，在第 7 天（价格 = 1）买入，在第 8 天（价格 = 4）卖出，利润 = 4 - 1 = 3。  
     总利润 = 3 + 3 = 6。
```
 **示例 2：**
```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（价格 = 1）买入，第 5 天（价格 = 5）卖出，利润 = 5 - 1 = 4。  
     注意：最多两笔交易，但一次交易已经获得最大利润。
```
 **示例 3：**
```
输入：prices = [7,6,4,3,1]
输出：0
解释：没有交易获取利润，返回 0。
```
**示例 4：**
```
输入：prices = [1]
输出：0
```

**提示：**
- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 105`

### 解法：动态规划
**题目重点：**
可以买卖两次，且手里最多持有一支股票

**思路：**
1.确定dp数组以及下标的含义
- `dp[i][0]`：第 i 天 **第一次买入** 后的最大利润
- `dp[i][1]`：第 i 天 **第一次卖出** 后的最大利润
- `dp[i][2]`：第 i 天 **第二次买入** 后的最大利润
- `dp[i][3]`：第 i 天 **第二次卖出** 后的最大利润
2.确定递推公式
`dp[i][0] = max(dp[i-1][0], -prices[i])`：第一次买入
`dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i])`：第一次卖出
`dp[i][2] = max(dp[i-1][2], dp[i-1][1] - prices[i]) `：第二次买入
`dp[i][3] = max(dp[i-1][3], dp[i-1][2] + prices[i]) `：第二次卖出

3.dp数组如何初始化
- `dp[0][0] = -prices[0]` ：第一天第一次买入
- `dp[0][1] = 0`  ：第一天第一次卖出（无交易）
- `dp[0][2] = -prices[0]`   ：第一天第二次买入
- `dp[0][3] = 0` ： 第一天第二次卖出

4.确定遍历顺序
`for (int i = 1; i < prices.size(); i++) `

**代码：**
这里之间参考`LeetCode122买卖股票的最佳时机II`的解法三，对动态规划数组进行状态压缩。
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int first_buy = -prices[0];
        int first_sell = 0;
        int second_buy = -prices[0];
        int second_sell = 0;
        for (int i = 1; i < prices.size(); i++) {
            first_buy = max(- prices[i], first_buy);
            first_sell = max(first_sell, first_buy + prices[i]);
            second_buy = max(second_buy, first_sell - prices[i]);
            second_sell = max(second_sell, second_buy + prices[i]);
        }
        return max(second_sell, first_sell);
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n) 
- 空间复杂度：O(1)
## LeetCode188买卖股票的最佳时机IV
### 题目
给定一个整数 `k` 和一个数组 `prices`，其中 `prices[i]` 是一支股票第 `i` 天的价格。
设计一个算法来计算你所能获得的最大利润。你最多可以完成 `k` 笔交易。
**注意**：你不能同时参与多笔交易（你必须在再次购买前卖出股票）。

示例 1：
```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天买入，第 2 天卖出，利润 = 4 - 2 = 2
```

 示例 2：
```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：
    第 2 天买入，第 3 天卖出，利润 = 6 - 2 = 4；
    第 5 天买入，第 6 天卖出，利润 = 3 - 0 = 3；
    总利润 = 4 + 3 = 7。
```

**提示：**
- `1 <= k <= 100`
- `1 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`
### 解法：动态规划 
**题目重点：**
可以买卖K次，且手里最多持有一支股票

**思路**
1）特殊情况优化：
如果 `k >= prices.size() / 2`，说明交易次数不再受限制 —— 相当于题目 122，可以无限交易：
```cpp
if (k >= prices.size() / 2) {
    int profit = 0;
    for (int i = 1; i < prices.size(); ++i) {
        if (prices[i] > prices[i - 1])
            profit += prices[i] - prices[i - 1];
    }
    return profit;
}
```

2）正常动态规划：
参考`LeetCode123买卖股票的最佳时机III`压缩后的解法，这道题的不同之处在于变量多了。
1.确定dp数组以及下标的含义
- `dp[j][0]`：**最多进行了 j 次交易，当前**手上**没有股票**的最大利润；
- `dp[j][1]`：**最多进行了 j 次交易，当前**手上**持有股票**的最大利润。
2.确定递推公式
对于第 `i` 天、第 `j` 次交易，有：
- `dp[j][0] = max(dp[j][0], dp[j][1] + prices[i])`
    - 今天**不操作** or **卖出股票**
- `dp[j][1] = max(dp[j][1], dp[j-1][0] - prices[i])`
    - 今天**不操作** or **买入股票**
3.dp数组如何初始化
初始化第 0 天的情况（即 `i = 0`）：
- 对于所有 `j`：
    - `dp[j][0] = 0`：不持股初始利润为 0
    - `dp[j][1] = -prices[0]`：持股初始利润为负第 0 天价格
解释：
- 可以认为在第 0 天就可以买入（花钱），但不可能在第 0 天前卖出（所以 `dp[j-1][0]` 初始化为 0 是合理的）
4.确定遍历顺序
- 外层：`for (int i = 1; i < prices.size(); ++i)` 遍历每天
- 内层：`for (int j = 1; j <= k; ++j)` 从第 1 次交易开始往上转移（必须从 1 开始，因为 `dp[j-1][0]` 要存在）

**代码：**
```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if (k >= n / 2) {
            int res = 0;
            for (int i = 1; i < n; i++) {
                if (prices[i] > prices[i - 1]) {
                    res = res + prices[i] - prices[i - 1];
                }
            }
            return res;
        }
        vector<vector<int>> dp(k + 1, vector<int>(2, 0));
        for (int j = 0; j <= k; j++)dp[j][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= k; j++) {
                dp[j][0] = max(dp[j][0], dp[j][1] + prices[i]);//卖出
                dp[j][1] = max(dp[j][1], dp[j - 1][0] - prices[i]);//买入
            }
        }
        return dp[k][0];
    }
};
```

**时间与空间复杂度：**
- **时间复杂度：** O(n*k)
- **空间复杂度：** O(k)


## LeetCode714买卖股票的最佳时机含手续费
### 题目
给定一个整数数组 `prices`，其中 `prices[i]`表示第 `i` 天的股票价格 ；整数 `fee` 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

**示例 1：**
```
输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
解释：  
- 在价格为 1 时买入  
- 在价格为 8 时卖出，利润为 8 - 1 - 2 = 5  
- 在价格为 4 时买入  
- 在价格为 9 时卖出，利润为 9 - 4 - 2 = 3  
总利润 = 5 + 3 = 8
```

**示例 2：**
```
输入：prices = [1,3,7,5,10,3], fee = 3
输出：6
```


### 解法一：贪心算法
和之前无手续费的版本（LeetCode 122）思路很像。

**题目重点：**
可以买卖多次，甚至当天买卖，但手里只能有一支股票，有手续费。

**思路：** 
- 原来在 LeetCode 122（无手续费）里，遇到每次涨价就买卖就行了。
- 但是加了手续费以后，每次交易（卖出）就要扣掉一次 `fee`，所以不能贪心地频繁买卖了。需要做到 尽量延迟卖出，一口气吃最大的上涨收益，摊平手续费的影响。
- 维护两个变量：
	- `持有状态 hold`：表示截至到目前为止，持有一支股票后，最大可能的收益。
	- `不持有状态 cash`：表示截至到目前为止，不持有股票的最大收益。
- 贪心逻辑：每天看卖了更赚钱还是不动更赚钱，买了更赚钱还是不动更赚钱。
	- 卖出（如果卖了赚得多，就卖出，同时付手续费）。
	- 买入（如果买了未来有希望赚更多，就买入）。
- 更新 cash：`cash = max(cash, hold + prices[i] - fee);`
	- 我可以选择继续不买不卖，那就保持昨天的 `cash`。
	- 也可以选择把手里的股票卖掉，那就拿到卖股票的钱 `prices[i]`，加上之前持有股票的利润 `hold`，然后扣掉手续费 `fee`。
- 更新 hold：`hold = max(hold, cash - prices[i]);`
	- 我可以选择继续持有股票，那就保持昨天的 `hold`。
	- 也可以选择今天买一支新股票，那就从现在的 cash 里扣掉买股票的钱 `prices[i]`。

**代码：**
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int cash = 0;//不持有股票
        int hold = -prices[0];//持有股票
        for (int i = 1; i < prices.size(); i++) {
            cash = max(cash, hold + prices[i] - fee);//更新不持有股票，卖出
            hold = max(hold, cash - prices[i]);//更新持有股票，买入
        }
        return cash;
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n)
- 空间复杂度：O(1)


### 解法二：动态规划，每天持有/不持有两种状态
和之前无手续费的版本（LeetCode 122）思路很像。

**题目重点：**
可以买卖多次，甚至当天买卖，但手里只能有一支股票，有手续费。

**思路：**
1.确定dp数组以及下标的含义
- `dp[i][0]`：第 i 天 **不持有** 股票的最大收益。
- `dp[i][1]`：第 i 天 **持有** 股票的最大收益。
2.确定递推公式
- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]-fee)`  
    - 要么继承昨天就不持股的状态；
	- 要么昨天持股，今天卖出，同时扣掉手续费 `fee`。
- `dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])`  
    - 要么继承昨天就持股的状态；
	- 要么昨天不持股，今天买入，花了 `prices[i]` 的钱。
3.dp数组如何初始化
- `vector<vector<int>> dp(n, vector<int>(2, 0));`表示当天收益为0
- `dp[0][0] = 0`，第0天没持股
- `dp[0][1] = -prices[0]`，第0天买了股票
4.确定遍历顺序
- ` for (int i = 1; i < prices.size(); i++)`

**代码：**
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        dp[0][0] = 0; //不持有股票
        dp[0][1] = -prices[0]; //持有股票
        for (int i = 1; i < prices.size(); i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[n - 1][0];
    }
};

```

**时间和空间复杂度**
- 时间复杂度：O(n)
- 空间复杂度：O(n)

### 解法三：动态规划 + 状态压缩（滚动数组）
和之前无手续费的版本（LeetCode 122）思路很像。

**题目重点：**
可以买卖多次，甚至当天买卖，但手里只能有一支股票，有手续费。

**思路：**
只保留前一天的两个状态，不用数组，节省空间。
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        int dp0 = 0; //不持有股票
        int dp1 = -prices[0]; //持有股票
        for (int i = 1; i < prices.size(); i++) {
            
            dp0 = max(dp0, dp1 + prices[i] - fee);
            dp1 = max(dp1, dp0 - prices[i]);
        }
        return dp0;
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n) 
- 空间复杂度：O(1)



## LeetCode309买卖股票的最佳时机含冷冻期
### 题目
给定一个整数数组`prices`，其中第  `prices[i]` 表示第 `_i_` 天的股票价格 。​
设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:
- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例1：**
```text
输入: prices = [1,2,3,0,2]
输出: 3
解释: 
第 1 天买入股票 (价格 = 1)
第 2 天卖出股票 (价格 = 2)，利润为 1
第 3 天冷冻期 (不能买)
第 4 天买入股票 (价格 = 0)
第 5 天卖出股票 (价格 = 2)，利润为 2
总利润 = 1 + 2 = 3
```

**提示：**
- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

### 解法一：动态规划
**题目重点：**
可以买卖多次，甚至当天买卖，但手里只能有一支股票，有冷冻期。

**思路：**
1.确定 dp 数组和下标含义
定义：
- `dp[i][0]`：第 i 天持有股票的最大利润。
- `dp[i][1]`：第 i 天不持有股票，且今天卖出（下一天处于冷冻期，不能买入）
- `dp[i][2]`：第 i 天不持有股票，且今天没有卖出（）下一天不再冷冻期
2.确定递推公式
- `dp[i][0] = max(dp[i-1][0], dp[i-1][2] - prices[i])`
    - 今天持有股票 = （昨天就持有）或（昨天没股票且不是冷冻，今天买入）
- `dp[i][1] = dp[i-1][0] + prices[i]`
    - 今天卖出股票（进入冷冻期）= 昨天持有股票 + 今天卖掉赚钱
- `dp[i][2] = max(dp[i-1][1], dp[i-1][2])`
    - 今天啥也没干 = （昨天是冷冻期过来的）或（昨天就是空闲状态）
3.初始化
第 0 天：
- `dp[0][0] = -prices[0]`：买入股票
- `dp[0][1] = 0`：不可能卖股票，所以是 0
- `dp[0][2] = 0`：没买没卖，也是 0
4.确定遍历顺序
自然是按照天数 `i = 1 -> n-1` 顺序遍历。

**代码：**
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(3, 0));
        dp[0][0] = -prices[0]; //持有股票
        dp[0][1] = 0; //不持有股票且此天卖出（下一天冷冻期）
        dp[0][2] = 0; //不持有股票且此天不卖出（下一天不在冷冻期）
        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][2] - prices[i]);
            dp[i][1] = dp[i - 1][0] + prices[i];
            dp[i][2] = max(dp[i - 1][1], dp[i - 1][2]);
        }
        return max(dp[n - 1][1], dp[n - 1][2]);
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n) 
- 空间复杂度：O(n)
### 解法二：动态规划 + 状态压缩（滚动数组）
**题目重点：**
可以买卖多次，甚至当天买卖，但手里只能有一支股票，有冷冻期。

**代码：**
此处只可这样写，不可`dp0 = max(dp0, dp2 - prices[i]);dp1 = dp0 + prices[i];dp2 = max(dp2, dp1);`
- 因为这样在更新`dp2 = max(dp2, dp1)`时是错的，如果`dp2=dp1`，那就是有冷冻期的。
```C+++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int dp0 = -prices[0]; //持有股票
        int dp1 = 0; //不持有股票且此天卖出（下一天冷冻期）
        int dp2 = 0; //不持有股票且此天不卖出（下一天不在冷冻期）
        for (int i = 1; i < n; i++) {
            int new_dp0 = max(dp0, dp2 - prices[i]);
            int new_dp1 = dp0 + prices[i];
            int new_dp2 = max(dp2, dp1);
            dp0 = new_dp0;
            dp1 = new_dp1;
            dp2 = new_dp2;
        }
        return max(dp1, dp2);
    }
};
```

**时间和空间复杂度**
- 时间复杂度：O(n) 
- 空间复杂度：O(1)

## 总结
 ### **1. 买卖股票的最佳时机（LeetCode 121）**
- **特点**：只能买卖一次。
- **解法**：
  - **一次遍历**：维护当前最低价格 `min_price` 和最大利润 `max_profit`。
  - **动态规划**：定义 `dp[i][0]`（不持股）和 `dp[i][1]`（持股）的状态转移。

**2. 买卖股票的最佳时机 II（LeetCode 122）**
- **特点**：无限次交易，无手续费。
- **解法**：
  - **贪心**：所有上涨日都买卖，累加利润。
  - **动态规划**：`dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])`（卖出）；`dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])`（买入）。

 **3. 买卖股票的最佳时机 III（LeetCode 123）**
- **特点**：最多完成 **两笔** 交易。
- **解法**：
  - **动态规划**：扩展状态为 `dp[i][0]`（第一次买入）、`dp[i][1]`（第一次卖出）、`dp[i][2]`（第二次买入）、`dp[i][3]`（第二次卖出）。
  - **状态转移**：
    - 第一次买入：`dp[i][0] = max(-prices[i], dp[i-1][0])`。
    - 第一次卖出：`dp[i][1] = max(dp[i-1][0] + prices[i], dp[i-1][1])`。
    - 第二次买入：`dp[i][2] = max(dp[i-1][1] - prices[i], dp[i-1][2])`。
    - 第二次卖出：`dp[i][3] = max(dp[i-1][2] + prices[i], dp[i-1][3])`。


 **4. 买卖股票的最佳时机 IV（LeetCode 188）**
- **特点**：最多完成 **k 笔** 交易。
- **解法**：
  - **动态规划**：状态 `dp[j][0]`（第 j 次交易后不持股）和 `dp[j][1]`（第 j 次交易后持股）。
  - **状态转移**：
    - `dp[j][0] = max(dp[j][0], dp[j][1] + prices[i])`（卖出）。
    - `dp[j][1] = max(dp[j][1], dp[j-1][0] - prices[i])`（买入）。
  - **优化**：若 `k >= n/2`，退化为无限交易问题（贪心）。


 **5. 买卖股票的最佳时机含手续费（LeetCode 714）**
- **特点**：无限次交易，每笔交易付手续费。
- **解法**：
  - **贪心**：延迟卖出以摊平手续费，更新 `cash`（不持股）和 `hold`（持股）：
    - `cash = max(cash, hold + prices[i] - fee)`。
    - `hold = max(hold, cash - prices[i])`。
  - **动态规划**：与无限交易类似，卖出时扣除手续费。


**6. 买卖股票的最佳时机含冷冻期（LeetCode 309）**
- **特点**：卖出后需等待一天才能买入。
- **解法**：
  - **动态规划**：分三种状态：
    - `dp[i][0]`：持股（来自前一天的持股或非冷冻期买入）。
    - `dp[i][1]`：不持股且当天卖出（下一天冷冻）。
    - `dp[i][2]`：不持股且非卖出（下一天可买入）。
  - **状态转移**：
    - `dp[i][0] = max(dp[i-1][0], dp[i-1][2] - prices[i])`。
    - `dp[i][1] = dp[i-1][0] + prices[i]`。
    - `dp[i][2] = max(dp[i-1][1], dp[i-1][2])`。

 **通用技巧**
1. **状态定义**：通常用 `dp[i][k][0/1]` 表示第 i 天、第 k 次交易、是否持股的利润。
2. **初始条件**：`dp[0][k][1] = -prices[0]`（第一天买入）。
3. **空间优化**：多数问题可通过滚动变量（如 `cash`、`hold`）将空间复杂度降至 O(1)。
4. **边界处理**：注意 `k` 的取值（如 `k >= n/2` 时退化为贪心）。
