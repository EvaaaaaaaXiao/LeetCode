## 136、只出现一次的数字

[题目地址](https://leetcode-cn.com/problems/single-number/submissions/)

### 题目描述
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例1:

```
输入: [2,2,1]
输出: 1
```

示例2:

```
输入: [4,1,2,1,2]
输出: 4
```



### 题解

#### 解法

参考LeetCode用户`灵魂画手`思路

根据题意，要不使用额外空间，且只有某个元素出现一次，其他元素都是**出现两次**

此处可以运用**位运算**中的**异或运算**，运算规则如下：
- 一个数与 0 做异或运算，等于本身
- 一个数与本身做异或运算，等于 0 
- 异或运算满足交换律和结合律，所以结果与运算顺序无关

所以可以对所有数字进行异或运算，最终得到的结果就是唯一只出现一次的那个元素，因为其他出现两次的都变成了 0 

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    var result = 0;
    for(num of nums){
        result = result ^ num;
    }
    return result;
};
```



## 409、最长回文串

[题目地址](https://leetcode-cn.com/problems/longest-palindrome/)

### 题目描述
给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如` "Aa"` 不能当做一个回文字符串。

**注意**:
假设字符串的长度不会超过 1010。

示例1:

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```



### 题解

#### 解法



```javascript
/**
 * @param {string} s
 * @return {number}
 */
var longestPalindrome = function(s) {
    var length = 0;
    var result = new Set();
    for(var index = 0;index < s.length;index ++) {
        if(!result.has(s[index])){
            result.add(s[index]);
        } else {
            length += 2;
            result.delete(s[index]);
        }
    }
    return result.size === 0 ? length : length + 1;
};
```