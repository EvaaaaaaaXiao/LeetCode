## 121、买卖股票的最佳时机

[题目地址](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

### 题目描述
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例1:

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

示例2:

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```


### 题解

#### 解法（392ms)

暴力解法

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    if(prices.length < 2) return 0;
    var result = 0;
    for(var i = 0;i < prices.length;i++) {
        for(var j = i + 1;j < prices.length;j++) {
            result = Math.max(result, prices[j] - prices[i]);
        }
    }
    return result;
};
```



## 300、最长上升子序列

[题目地址](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

### 题目描述
给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明**:

- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(n2) 。

**进阶**: 你能将算法的时间复杂度降低到 O(n log n) 吗?


### 题解

#### 解法（108ms)

暴力解法

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    if(!nums.length){
        return 0;
    }
    var dp = new Array(nums.length).fill(1);
    var max = 1;
    for(var i = 1; i < nums.length; i++) {
        for(var j = 0; j < i; j++) {
            if(nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
                max = Math.max(dp[i], max);
            }
        }
    }
    return max;
};
```




## 322、零钱兑换

[题目地址](https://leetcode-cn.com/problems/coin-change/)

### 题目描述
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回` -1`。

示例1:

```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

示例2:

```
输入: coins = [2], amount = 3
输出: -1
```

**说明**:
你可以认为每种硬币的数量是无限的。


### 题解

#### 解法（96ms)



```javascript
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    if(amount === 0) return 0;
    if(coins.length === 0) return -1;
    var result = new Array(amount + 1).fill(Infinity);
    result[0] = 0;
    for(var i = 1;i < amount + 1; i++){
        for(var j = 0;j < coins.length; j++){
            if (i - coins[j] >= 0) {
                result[i] = Math.min(result[i], result[i - coins[j]] + 1);
            }
        }
    }
    return result[amount] === Infinity ? -1 : result[amount];
};
```




## 面试题 17.16、按摩师

[题目地址](https://leetcode-cn.com/problems/the-masseuse-lcci/)

### 题目描述
一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。

**注意**：本题相对原题稍作改动

示例1:

```
输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。
```

示例2:

```
输入： [2,7,9,3,1]
输出： 12
解释： 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。
```

示例3:

```
输入： [2,1,4,5,3,1,1,3]
输出： 12
解释： 选择 1 号预约、 3 号预约、 5 号预约和 8 号预约，总时长 = 2 + 4 + 3 + 3 = 12。
```


### 题解

#### 解法（52ms)



```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var massage = function(nums) {
    var len = nums.length;
    if(len == 0) return 0;
    else if(len == 1) return nums[0];
    var result = [];
    result[0] = nums[0];
    result[1] = Math.max(nums[0], nums[1]);
    for(var i = 2;i < len;i ++){
        result[i] = Math.max(result[i -2] + nums[i], result[i - 1]);
    }
    return result[len - 1];
};
```