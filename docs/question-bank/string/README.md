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

#### 解法（168ms)

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



## 14、最长公共前缀

[题目地址](https://leetcode-cn.com/problems/longest-common-prefix/)

### 题目描述
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串` ""`。

示例1:

```
输入: ["flower","flow","flight"]
输出: "fl"
```

示例2:

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```



### 题解

#### 解法（68ms)

将第一个字符串与第二个字符串匹配得出公共前缀，再将这个公共前缀与第三个字符串匹配，再……循环匹配至最后一个元素

- 当字符串数组为空，直接返回空字符串
- 当公共前缀为空字符串，直接返回空字符串，无需往后匹配

```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    var result=""
    if(strs.length>0){
        result=strs[0]
        for(var i=1;i<strs.length;i++){
            if(result=="")
                break;
            else
                result = getSame(result,strs[i])
        }
    }
    return result
};
function getSame(str1,str2){
    var len = str1.length < str2.length ? str1.length : str2.length
    var i = 0
    for(;i<len;i++){
        if(str1[i]!=str2[i]){
           break;
        }
    }
    return str1.substr(0,i)
}
```



### 思考

####  str.startsWith() 

此方法可用于检测字符串是否以指定的子字符串开始，如果是以指定的子字符串开头返回 true，否则 false。

**！对大小写敏感**

```javascript
var str = "Hello world, welcome to the Runoob.";
var n = str.startsWith("Hello"); //true
var m = str.startsWith("world", 6); //true
```



## 28、实现strStr()

[题目地址](https://leetcode-cn.com/problems/implement-strstr/)

### 题目描述
实现 `strStr()` 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回**  -1**。

示例1:

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

示例2:

```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 `strstr()` 以及 Java的 `indexOf()` 定义相符。



### 题解

#### 解法（68ms)

**！用了 `substr` ，不是好解法，**是第一反应还是提交了

用 needle 的第一个字符去 haystack 匹配，再用 `substr` 截取 needle 长度的字符串与 needle 匹配

```javascript
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    var h = haystack.length
    var n = needle.length
    if (h < n) return -1
    if (n == 0) return 0
    var index = -1
    for(var i=0;i<h;i++){
        if(haystack[i] == needle[0] && haystack.substr(i,i+n) == needle){
            index = i
            break
        }
    }
    return index
};
```