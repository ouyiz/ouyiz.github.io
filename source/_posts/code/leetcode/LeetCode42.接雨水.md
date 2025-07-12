
## 题目：
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**
![](data:image/png;base64,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)

输入：`height = [0,1,0,2,1,0,1,3,2,1,2,1]  `
输出：6  
解释：上面是由数组 `[0,1,0,2,1,0,1,3,2,1,2,1]` 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

**示例 2：**
输入：`height = [4,2,0,3,2,5]  `
输出：9

提示：
- n == height.length
- 1 <= n <= 2 * 104
- 0 <= height[i] <= 105



## 解法一：暴力

逐个计算每一列能存下的水量。

遍历整个数组。针对数组中的每个元素`arr[i]`，都分别向左、向右遍历一遍数组，找到`arr[i]`左侧和右侧的最大值，计为`leftMax`和`rightMax`，如果`leftMax <= arr[i]`或者`rightMax <= arr[i]`，说明当前这一列存不出水。否则当前列能存储的水量为`min(leftMax, rightMax) - arr[i]`。

所以数组的i位置能存储的水量`res[i] = max{0, min{max(arr[0...i-1]), max(arr[i+1...n])} - arr[i] }`。

再思考一下，如果`max(arr[0...i-1]) > arr[i]`，那么数组`0`到`i-1`位置的最大值和`0`到`i`位置的最大值一定是相等的；如果`max(arr[0...i-1]) <= arr[i]`，那么数组`0`到`i`位置的最大值一定等于`arr[i]`，所以为了避免和`0`之间取`max`的计算，上述公式可以化简为下面的形式。
`res[i] = min{max{arr[0... i]} , max{arr[i . . . n − 1]}} − arr[i]`
```C++
int trap(vector<int>& height) {
    int sum = 0;
    int n = height.size();
    for (int i=0; i<n; i++) {
        int leftMax = 0;
        for (int j=0; j<=i; j++) {
            if (height[j] > leftMax) {
                leftMax = height[j];
            }
        }
        int rightMax = 0;
        for (int j=i; j<n; j++) {
            if (height[j] > rightMax) {
                rightMax = height[j];
            }
        }
        sum += min(leftMax, rightMax) - height[i];
    }
    return sum;

}
```
## 解法二：双指针

从暴力解法中我们可以看到，要求数组`i`位置可以存储的水量，需要先求出`0`到`i`位置的最大值`max(arr[0...i])`，再求出`i`到`n-1`位置的最大值`max(arr[i...n-1])`，两个值中取最小与`arr[i]`做差。

暴力解法之所以时间复杂度比较差，是因为对于数组中的每一个元素，都需要再遍历一遍数组才能得到它左右两侧的最大值。

所以我们可以通过预处理数组得到`leftMax[]`和`rightMax[]`两个数组，`leftMax[i]`代表数组`0`到`i`位置的最大值，`leftMax[i] = max(leftMax[i-1], arr[i])`；`rightMax[i]`代表数组`i`位置到`n-1`位置的最大值，`rightMax[i] = max(rightMax[i+1], arr[i])`。

这样我们就得到了如下的算法流程。

首先遍历数组，从左向右得到数组`leftMax[]`，再从右向左得到`rightMax[]`。然后再遍历一遍数组，对于数组的每一个位置`i`，通过`leftMax[i]`，`rightMax[i]`和`arr[i]`得到结果，将结果汇总得到的值就是最终答案。
```C++
int trap(vector<int>& height) {
    int n = height.size();
    vector<int> leftMax(n, 0);
    leftMax[0] = height[0];
    for (int i=1; i<n; i++) {
        leftMax[i] = max(leftMax[i-1], height[i]);
    }

    vector<int> rightMax(n, 0);
    rightMax[n-1] = height[n-1];
    for (int i=n-2; i>=0; i--) {
        rightMax[i] = max(rightMax[i+1], height[i]);
    }

    int res = 0;
    for (int i=0; i<n; i++) {
        res += min(leftMax[i], rightMax[i]) - height[i];
    }
    return res;

}
```
## 解法三：双指针进阶

解法二中的双指针需要遍历三次数组，第一次得出数组`leftMax[]`，第二次得出数组`rightMax[]`，第三次才是根据`leftMax[i]`，`rightMax[i]`和`height[i]`得出结果。那我们能不能想办法把这三次遍历合并成一次呢？

我们设置两个指针，`left`指向数组的`0`位置，`right`指针指向数组的`n-1`位置。再使用两个变量`leftMax`和`rightMax`，`leftMax`的含义是数组`0...left`位置的最大值，`rightMax`的含义是数组`right...n-1`位置的最大值，这几个变量设置好后就有以下几种情况。

- `leftMax < rightMax`，此时可以使用`leftMax`来结算`height[left]`位置的储水量。它的右侧可能还会有比`rightMax`更高的元素，但不会影响`left`位置的储水量。因为这种情况下`left`位置左侧的最大值是影响该位置储水量的瓶颈，此时`res[left] = leftMax - height[left]`。
- `leftMax > rightMax`，此时可以使用`rightMax`来结算`right`位置的储水量。同样的，它的左侧可能还会有比`leftMax`更高的元素，但都不影响`right`位置的储水量。因为这种情况下`right`位置右侧的最大值是影响该位置储水量的瓶颈。此时`res[right] = rightMax - height[right]`。
- `leftMax == rightMax`，此时既可以结算左侧，也可以结算右侧，或者左右两侧可以同时结算储水量。`res[left] = leftMax - height[left]`，`res[right] = rightMax - height[right]`。但是要注意如果结算前`left == right`，此时只能结算一侧。

不断重复上述流程，哪侧结算就将哪侧的指针相应移动，并在移动的过程中更新`leftMax`和`rightMax`，直到两个指针会合，结算完最后一个位置的水量为止。

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int leftMax = 0;
        int rightMax = 0;
        int res = 0;
        while (left <= right) {
            leftMax = max(leftMax, height[left]);
            rightMax = max(rightMax, height[right]);
            if (leftMax < rightMax) {
                res += leftMax - height[left++];
            }
            else if (leftMax >= rightMax) {
                res += rightMax - height[right--];
            }
        }
        return res;
    }
};
```
