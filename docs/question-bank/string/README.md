## 13、罗马数字转整数

[题目地址](https://leetcode-cn.com/problems/roman-to-integer/)

### 题目描述
罗马数字包含以下七种字符: `I`，`V`，` X`，` L`，`C`，`D` 和` M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做` XII` ，即为` X` +` II` 。 27 写做  `XXVII`, 即为 `XX` +` V` +` II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做` IIII`，而是` IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为` IX`。这个特殊的规则只适用于以下六种情况：

`I` 可以放在` V` (5) 和` X` (10) 的左边，来表示 4 和 9。
`X` 可以放在 `L `(50) 和` C` (100) 的左边，来表示 40 和 90。 
`C` 可以放在` D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

示例:

```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```



### 题解

#### 解法一（168ms)

将字符串内容转换成对应的数值，边比较大小边计算总和
当前数值比右侧数值小，就将总和减去当前数值；当前数值大，就将总和加上当前数值

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
    var sum = 0
    var obj = {
        'I':1,
        'V':5,
        'X':10,
        'L':50,
        'C':100,
        'D':500,
        'M':1000
    }
    for(var i=0;i<s.length;i++){
        obj[s[i]]<obj[s[i+1]]?sum -= obj[s[i]]:sum += obj[s[i]]
    }
    return sum
};
```



### 思考

####  str[i] vs str.charAt(i) 字符串下标取值

javascript 中 String 实例是一个类数组，所以可以用 str[i] 获取一个 String 对象中键名为 i 的值，也可以用 String 类自带的 charAt( i ) 获取 i 的值

两者的区别体现在下标不存在时，**str[index] 会返回 undefined (未定义)， str.charAt(index)会返回""(空字符串）**
```javascript
var str="abc";
str[3]; //undefined
str.charAt(3); //""
```