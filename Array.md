---
title: 数组
categories:
  - 算法
  - 数组
tags:
  - 数组
author:
  - Jungle
date: 2022-05-02 
---

# 数组

## 前缀和

### 303-[区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/description/)

给定一个整数数组  `nums`，处理以下类型的多个查询:

1. 计算索引 `left` 和 `right` （包含 `left` 和 `right`）之间的 `nums` 元素的 **和** ，其中 `left <= right`

实现 `NumArray` 类：

- `NumArray(int[] nums)` 使用数组 `nums` 初始化对象
- `int sumRange(int left, int right)` 返回数组 `nums` 中索引 `left` 和 `right` 之间的元素的 **总和** ，包含 `left` 和 `right` 两点（也就是 `nums[left] + nums[left + 1] + ... + nums[right]` )

 

**示例 1：**

```
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

 

**提示：**

- `1 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`
- `0 <= i <= j < nums.length`
- 最多调用 `104` 次 `sumRange` 方法



> 思路: 前缀和
>
> 不要在`sumRange`里使用`for`循环, 通过前缀和技巧将时间复杂度降为O(1).

```java
class NumArray {

    /** preSum[i] 记录 num[0...i-1]的和 */
    int[] preSum;

    public NumArray(int[] nums) {
        preSum = new int[nums.length+1];
        for (int i = 1; i < preSum.length; i++) {
            preSum[i] = preSum[i-1] + nums[i-1];
        }
    }
    
    public int sumRange(int left, int right) {
        return preSum[right+1] - preSum[left];
    }
}
```

---



### 304-二维区域和检索-矩阵不可变

给定一个二维矩阵 `matrix`，以下类型的多个请求：

- 计算其子矩形范围内元素的总和，该子矩阵的 **左上角** 为 `(row1, col1)` ，**右下角** 为 `(row2, col2)` 。

实现 `NumMatrix` 类：

- `NumMatrix(int[][] matrix)` 给定整数矩阵 `matrix` 进行初始化
- `int sumRegion(int row1, int col1, int row2, int col2)` 返回 **左上角** `(row1, col1)` 、**右下角** `(row2, col2)` 所描述的子矩阵的元素 **总和** 。



**示例 1：**

<img src="D:\Data\Blog\hexo\source\_posts\Array\304-1" alt="304-1" style="zoom:80%; margin:0px;" />

```
输入: 
["NumMatrix","sumRegion","sumRegion","sumRegion"]
[[[[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]],[2,1,4,3],[1,1,2,2],[1,2,2,4]]
输出: 
[null, 8, 11, 12]

解释:
NumMatrix numMatrix = new NumMatrix([[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (红色矩形框的元素总和)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (绿色矩形框的元素总和)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (蓝色矩形框的元素总和)
```



**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `-105 <= matrix[i][j] <= 105`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- 最多调用 `104` 次 `sumRegion` 方法



> 思路: `preSum[i][j]` 记录矩阵 `[0, 0, i , j]` 的元素和 (`i 和 j 从1开始`)
>
> 当前  `preSum[i][j]` 等于  `preSum[i-1][j] + preSum[i][j-1] - preSum[i-1][j-1] + matrix[i-1][j-1]`
>
> 反过来, ` sumRegion[row1, col1, row2, col2] ` 等于  `preSum[row1+1][row2+1] - preSum[row1][row2+1] - preSum[row1+1][row2] + preSum[row1][row2]`

```java
class NumMatrix {

    int[][] preSum;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        preSum = new int[m+1][n+1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
               preSum[i][j] = preSum[i-1][j] + preSum[i][j-1] - preSum[i-1][j-1] + matrix[i-1][j-1];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return preSum[row2+1][col2+1] - preSum[row1][col2+1] - preSum[row2+1][col1] + preSum[row1][col1];
    }
}
```



---

### 560-和为k的子数组



给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。



**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```



**提示：**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`

> 思路:

解法一: 先计算前缀和, 再通过前缀和之差穷举所有数组之和.

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int[] preSum = new int[n+1];
        preSum[0] = 0;
        for (int i = 1; i <= n; i++) {
            preSum[i] = preSum[i-1] + nums[i-1];
        }

        int res = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                if (preSum[i] - preSum[j] == k) {
                    res++;
                }
            }
        }

        return res;
    }
}
```

解法二:  `preSum[i] - k  == preSum[j]` 找到有几个j能够满足条件,  就res++

优化思路: 直接记录下有几个 `preSum[j] == preSum[i] - k ` , 直接更新结果,  **避免内层`for`循环**

<img src="D:\Data\Blog\hexo\source\_posts\Array\560-1" alt="560-1" style="zoom:90%;margin:0px;" />

**将和为k转化为前缀和等于preSum[j]**

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        // 记录前缀和出现的次数
        Map<Integer, Integer> preSum = new HashMap<>();
        preSum.put(0, 1);

        int num0_i = 0;
        int res = 0;
        for (int i = 0; i < n; i++) {
            num0_i += nums[i];
            int num0_j = num0_i - k;
            if (preSum.containsKey(num0_j)) {
                res += preSum.get(num0_j);
            }
            preSum.put(num0_i, preSum.getOrDefault(num0_i, 0)+1);
        }

        return res;
    }
}
```

---





