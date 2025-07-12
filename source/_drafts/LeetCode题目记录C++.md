## LeetCode55.跳跃游戏
[55. 跳跃游戏 - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game/description/?envType=study-plan-v2&envId=top-interview-150)
此题并不适合动态规划，需要双重循环。
此题可以使用贪心算法，维护一个变量 `maxPos` 表示能到达的最远位置
```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxPos = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (maxPos >= i)maxPos = max(maxPos, nums[i] + i);
            else if (maxPos >= nums.size() - 1)return true;
            else return false;
        }
        return true;
    }
};
```


## LeetCode45.跳跃游戏 II
笨人此题使用动态规划，but贪心耗时小。
```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, INT_MAX);//到位置i的最小步数
        dp[0] = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 1; (j <= nums[i]) && (i + j < n); j++) {
                dp[i + j] = min(dp[i + j], dp[i] + 1);
            }
        }
        return dp[n - 1];
    }
};
```
贪心思想：每次从当前位置出发，走到“当前能跳的范围”里，挑一个“下一跳能跳得最远的”点。
注意跳到最后一个位置不需要跳了，所以是`i < n-1`。
```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        int jump = 0;//跳跃次数
        int end = 0;//这次跳跃所能达到的最远点
        int farthest = 0;//当前跳跃范围时能跳到的最远点
        for (int i = 0; i < n-1; i++) {
            farthest = max(farthest, i + nums[i]);
            if (i == end) {
                jump++;
                end = farthest;
            }
        }
        return jump;
    }
};
```

## LeetCode274. H-Index
sort一下即可
```C++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(), citations.end());
        int h = 0;
        int n = citations.size();
        for (int i = n - 1; i >= 0; i--) {
            if (n - i >= h && citations[i] >= n - i) {
                h = n - i;
            }
        }
        return h;
    }
};
```


## LeetCode380. Insert Delete GetRandom O(1)
要能够随机返回现有的一项，所以需要vector来辅助。最终使用`vector`与`unordered_map<int, int>`
```C++
class RandomizedSet {
public:
    unordered_map<int, int> valToIdx;
    vector<int> vec;
    RandomizedSet() {

    }

    bool insert(int val) {
        if (valToIdx.count(val))return false;
        vec.push_back(val);
        valToIdx[val] = vec.size() - 1;
        return true;
    }

    bool remove(int val) {
        if (!valToIdx.count(val)) return false;
        int i = valToIdx[val];
        int n = vec.size();
        vec[i] = vec[n - 1];
        valToIdx[vec[i]] = i;
        vec.pop_back();
        valToIdx.erase(val);
        return true;
    }

    int getRandom() {
        int r = rand() % vec.size();
        return vec[r];
    }
};
```

## LeetCode238.除自身以外数组的乘积
不能用除法，那就只能分左右两边，然后相乘
```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> left(nums.size(), 1);
        for (int i = 1; i < nums.size(); i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }
        int right = 1;
        for (int i = nums.size() - 1; i >= 0; i--) {
            left[i] = left[i] * right;
            right = right * nums[i];
        }
        return left;
    }
};
```

## LeetCode134.加油站
贪心
- 计算总油量 `totalTank` = 所有 `gas[i] - cost[i]` 之和，如果总油量 < 0，说明无论从哪里出发都不可能走完一圈。
- 遍历所有加油站，维护当前的油箱余量 `curTank`，如果某一站出发会使得 `curTank` < 0，说明不能从当前起点到这里 —— 应该从下一个站重新开始尝试。
	- 为什么我们在 currTank < 0 时更新起点是合理的：当前从 `start` 开始走，走到了位置 `i`，结果油不够用了（`currTank < 0`）。说明从 `start` 到 `i` 的这段路，不可能作为一段完整路径的前半段。那么就直接放弃从 `start` 到 `i` 之间的任何站点作为起点，因为它们都无法完成。所以考虑从 i+1 重新开始试试看。
	- 为什么最终选中的这个 start 是对的：所有失败的起点都被排除了，一旦我们找到了一个起点，从它出发之后到终点 `n-1`，currTank 一直是 ≥ 0 的；又因为 `totalTank ≥ 0`，说明从这个新起点走完整一圈，油是够的；
```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total = 0;
        int cur = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            int diff = gas[i] - cost[i];
            total += diff;
            cur += diff;
            if (cur < 0) {
                start = i + 1;
                cur = 0;
            }
        }
        return total >= 0 ? start : -1;
    }
};
```


## LeetCode135.分发糖果
需要满足两边的条件，所以：
1. 先从左到右遍历一遍，如果 `ratings[i] > ratings[i - 1]`，就令 `candies[i] = candies[i - 1] + 1`
2. 再从右到左遍历一遍，如果 `ratings[i] > ratings[i + 1]`，就令 `candies[i] = max(candies[i], candies[i + 1] + 1)`
```C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candy(ratings.size(), 1);
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i] > ratings[i - 1]) {
                candy[i] = candy[i - 1] + 1;
            }
        }

        for (int i = ratings.size() - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candy[i] = max(candy[i], candy[i + 1] + 1);
            }
        }
        int total = 0;
        for (int i = 0; i < ratings.size(); i++) {
            total = total + candy[i];
        }
        return total;
    }
};

```

## LeetCode13.罗马数字转整数
质朴的解法
```C++
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> umap = {
            {'I',1},{'V',5},{'X',10},{'L',50},
            {'C',100 },{'D',500},{'M',1000}
        };
        int res = 0;
        int n = s.size();
        for (int i = 0; i < s.size(); i++) {
            if (i < n - 1 && s[i] == 'I' && s[i + 1] == 'V') {
                res = res + 4; i++;
            }
            else if(i < n - 1 && s[i] == 'I' && s[i + 1] == 'X'){
                res = res + 9; i++;
            }
            else if (i < n - 1 && s[i] == 'X' && s[i + 1] == 'L') {
                res = res + 40; i++;
            }
            else if (i < n - 1 && s[i] == 'X' && s[i + 1] == 'C') {
                res = res + 90; i++;
            }
            else if (i < n - 1 && s[i] == 'C' && s[i + 1] == 'D') {
                res = res + 400; i++;
            }
            else if (i < n - 1 && s[i] == 'C' && s[i + 1] == 'M') {
                res = res + 900; i++;
            }
            else {
                res = res + umap[s[i]];
            }

        }
        return res;
    }
};
```

聪明的解法，求自己多动脑
```C++
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> umap = {
            {'I',1},{'V',5},{'X',10},{'L',50},
            {'C',100 },{'D',500},{'M',1000}
        };
        int res = 0;
        int n = s.size();
        for (int i = 0; i < s.size(); i++) {
            if (i < n - 1 && umap[s[i]] < umap[s[i + 1]]) {
                res = res - umap[s[i]];
            }
            else {
                res = res + umap[s[i]];
            }

        }
        return res;
    }
};
```

## LeetCode12.整数转罗马数字
```C++
class Solution {
public:
    string intToRoman(int num) {
        vector<int> values = { 1000,900,500,400,100,90,50,40,10,9,5,4,1 };
        vector<string> romans = { "M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I" };
        int i = 0;
        string res;
        while (num > 0) {
            if (num - values[i] >= 0) {
                num = num - values[i];
                res = res + romans[i];
            }
            else {
                i++;
            }
        }
        return res;
    }
};
```

## LeetCode58.最好一个单词的长度
轻轻松松，也就是两个值，一个只在不为空更新，一个不为空加1为空变0
```C++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int res = 0;
        int cur = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') { cur = 0; }
            else {
                cur++;
                res = cur;
            }
        }
        return res;
    }
};
```

## LeetCode14.最长公共前缀
先排序字符串数组，然后只比较首尾两个字符串的公共前缀即可。因为排好序后，相邻字符串之间变化最剧烈的两个，就是第一个和最后一个。
```C++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        sort(strs.begin(), strs.end());
        string begin = strs.front();
        string end = strs.back();
        int i = 0;
        while (i < begin.size() && i < end.size() && begin[i] == end[i]) {
            i++;
        }
        return begin.substr(0, i);
    }
};

```



## LeetCode151.反转字符串中的单词
首先通过一个指针指向新string的位置来移除空格。
然后翻转整个字符串，再翻转每个单词。
```C++
class Solution {
public:
    void reverse(string& s, int start, int end) {
        while (start < end)
        {
            swap(s[start], s[end]);
            start++;
            end--;
        }
    }
    void removeSpace(string & s) {
        int a = 0;
        for (int i = 0; i < s.size();i++) {
            if (s[i] != ' ') {
                if (a != 0)s[a++] = ' ';//在新单词开始前加空格
                while (i < s.size() && s[i] != ' ') {
                    s[a++] = s[i++];
                }
            }
        }
        s.resize(a);
    }
    string reverseWords(string s) {
        removeSpace(s);
        reverse(s, 0, s.size() - 1);
        int left = 0;
        int right = 0;
        while (left < s.size() && right <= s.size()) {
            if (s[right] == ' ' || right == s.size()) {
                reverse(s, left, right - 1);
                left = right + 1;
                right = right + 1;
            }
            else {
                right++;
            }
        }
        return s;
    }
};
```
`istringstream iss(s);`这一句是使用 C++ 标准库中的` istringstream` 类，把字符串` s `包装成一个“字符串输入流”，让你可以像读文件或读标准输入那样一个单词一个单词地读出字符串中的内容。
```C++
class Solution {
public:
    
    string reverseWords(string s) {
        istringstream iss(s);
        string word;
        stack<string> st;
        while (iss >> word) {
            st.push(word);
        }
        string res;
        while (!st.empty()) {
            res = res + st.top();
            st.pop();
            if (!st.empty())res = res + " ";
        }
        return res;
    }
};
```

## LeetCode6.Z字形变换
就是对s去模拟变换这个过程。使用`vector`存储每一行的值。
```C++
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows == 1 || numRows >= s.size())return s;
        vector<string> rows(numRows);
        int cur = 0;
        bool goingDown = false;
        for (char c : s) {
            rows[cur] += c;
            if (cur == 0 || cur == numRows - 1) {
                goingDown = !goingDown;
            }
            cur += goingDown ? 1 : -1;
        }
        string res;
        for (string row : rows) {
            res = res + row;
        }
        return res;
    }
};
```

## LeetCode 125. 验证回文串
- `isalnum()` -检查是否为数字或字母
- `isalpha()` - 检查是否是字母
- `isdigit()` - 检查是否是数字
- `islower()` - 检查是否是小写字母
- `isupper()` - 检查是否是大写字母
- `tolower()` - 转换为小写
- `toupper()` - 转换为大写
```C++
class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0, right = s.size() - 1;
        
        while (left < right) {
            while(left<right&&!isalnum(s[left]))left++;
            while(left<right&&!isalnum(s[right]))right--;
            if(tolower(s[left])!=tolower(s[right])){
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
};
```

手写版
```C++
bool isAlnum(char c) {
    return (c >= 'a' && c <= 'z') ||  // 小写字母
           (c >= 'A' && c <= 'Z') ||  // 大写字母
           (c >= '0' && c <= '9');     // 数字
}
```

```C++
char toLower(char c) {
        if (c >= 'A' && c <= 'Z') {
            return c + 32;  // 大写转小写
        }
        return c;
    }
```


## LeetCode392.判断子序列
轻轻松松双指针
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int a=0;
        int b=0;
        while(a<s.size()&&b<t.size()){
            if(s[a]==t[b]){
                a++;
                b++;
            }
            else{
                b++;
            }
        }
        if(a==s.size())return true;
        else return false;
    }
};
```

## LeetCode157.两数之和II-输入有序数组
轻轻松松双指针
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int a=0;
        int b=numbers.size()-1;
        while(a<b){
            if(numbers[a]+numbers[b]>target){
                b--;
            }
            else if(numbers[a]+numbers[b]<target){
                a++;
            }
            else{
                return {a+1,b+1};
            }
        }
        return {};
    }
};
```

## LeetCode11.盛最多水的容器
轻轻松松双指针
```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left=0;
        int right=height.size()-1;
        int res=0;
        while(left<right){
            int tmp=(right-left)*min(height[left],height[right]);
            res=max(res,tmp);
            if(height[left]>height[right]){
                right--;
            }
            else{
                left++;
            }
        }
        return res;
    }
};
```


## LeetCode15.三数之和
固定一个数，然后使用双指针。
注意要对a,b,c都进行去重
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        
        for (int a = 0; a < n - 2; a++) {
            if(a>0&&nums[a]==nums[a-1])continue;
            int b=a+1;int c=n-1;
            while(b<c){
                int sum=nums[a]+nums[b]+nums[c];
                if(sum==0){
                    res.push_back({nums[a],nums[b],nums[c]});
                    while(b<c&&nums[b]==nums[b+1])b++;
                    while(b<c&&nums[c]==nums[c-1])c--;
                    b++;
                    c--;
                }
                else if(sum>0){
                    c--;
                }
                else {
                    b++;
                }
            }
        }
        return res;
    }
};
```