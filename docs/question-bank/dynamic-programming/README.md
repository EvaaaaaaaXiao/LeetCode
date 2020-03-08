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