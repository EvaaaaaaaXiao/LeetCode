## 8、字符串转换整数 (atoi)

[题目地址](https://leetcode-cn.com/problems/string-to-integer-atoi/)

### 题目描述
请你来实现一个 `myAtoi(string s)` 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 `atoi` 函数）。

函数 `myAtoi(string s)` 的算法如下：

- 读入字符串并丢弃无用的前导空格
- 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
- 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
- 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 `0` 。必要时更改符号（从步骤 2 开始）。
- 如果整数数超过 32 位有符号整数范围 `[−2^31,  2^31 − 1]` ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 `−2^31` 的整数应该被固定为 `−2^31` ，大于 `2^31 − 1` 的整数应该被固定为 `2^31 − 1` 。
- 返回整数作为最终结果。


**注意：**

- 本题中的空白字符只包括空格字符 `' '` 。
- 除前导空格或数字后的其余字符串外，**请勿忽略** 任何其他字符。

示例1:

```
输入：s = "42"
输出：42
解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
解析得到整数 42 。
由于 "42" 在范围 [-231, 231 - 1] 内，最终结果为 42 。
```

示例2:

```
输入：s = "   -42"
输出：-42
解释：
第 1 步："   -42"（读入前导空格，但忽视掉）
            ^
第 2 步："   -42"（读入 '-' 字符，所以结果应该是负数）
             ^
第 3 步："   -42"（读入 "42"）
               ^
解析得到整数 -42 。
由于 "-42" 在范围 [-231, 231 - 1] 内，最终结果为 -42 。
```

示例3:

```
输入：s = "4193 with words"
输出：4193
解释：
第 1 步："4193 with words"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："4193 with words"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："4193 with words"（读入 "4193"；由于下一个字符不是一个数字，所以读入停止）
             ^
解析得到整数 4193 。
由于 "4193" 在范围 [-231, 231 - 1] 内，最终结果为 4193 。
```

示例4:

```
输入：s = "words and 987"
输出：0
解释：
第 1 步："words and 987"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："words and 987"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："words and 987"（由于当前字符 'w' 不是一个数字，所以读入停止）
         ^
解析得到整数 0 ，因为没有读入任何数字。
由于 0 在范围 [-231, 231 - 1] 内，最终结果为 0 。
```

示例5:

```
输入：s = "-91283472332"
输出：-2147483648
解释：
第 1 步："-91283472332"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："-91283472332"（读入 '-' 字符，所以结果应该是负数）
          ^
第 3 步："-91283472332"（读入 "91283472332"）
                     ^
解析得到整数 -91283472332 。
由于 -91283472332 小于范围 [-231, 231 - 1] 的下界，最终结果被截断为 -231 = -2147483648 。
```

**提示：**

- `0 <= s.length <= 200`
- `s` 由英文字母（大写和小写）、数字（`0-9`）、`' '`、`'+'`、`'-'` 和 `'.'` 组成


### 题解

#### 解法（108ms)

一开始想着直接字符串循环，遇到不同类型的元素进行不同的处理，提交了两次没通过，感觉实在是有太多种情况了，考虑起来比较麻烦。
重新想了一下，它是有一定规则的字符串匹配上后，再进行处理。就选用了正则表达式去匹配符合规则的部分，在进行字符串转整数的处理，就方便很多了。

(正则部分，急需加强学习

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var myAtoi = function(s) {
    var result = 0;
    s = s.match(/^[ ]*(\-|\+)?\d+/g);
    if(s){
        s = s[0];
        if(s.length < 10){
            result = parseInt(s);
        }else{
            result = parseInt(s);
            if(result < Math.pow(-2, 31)){
                result = Math.pow(-2, 31)
            }else if(result >= Math.pow(2, 31)){
                result = Math.pow(2, 31) - 1;
            }
        }
    }
    return result;
};
```



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



#### 二刷（2021/07/15)

呃呃 无脑暴力 下午犯困了

```javascript
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    if(haystack.length < needle.length) return -1;
    else if(needle.length == 0) return 0;
    var result = -1;
    for(var i = 0;i <= (haystack.length - needle.length);i++){
        if(haystack[i] == needle[0]){
            result = i;
            for(var j = 1;j < needle.length;j ++){
                if(haystack[i + j] != needle[j]){
                    result = -1;
                    break;
                }
            }
            if(result >= 0){
                return result;
            }
        }
    }
    return result;
};
```





## 38、外观数列

[题目地址](https://leetcode-cn.com/problems/count-and-say/submissions/)

### 题目描述
「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
`1` 被读作  `"one 1" ` ( `"一个一"`) , 即 `11`。
`11` 被读作` "two 1s"` (`"两个一"`）, 即 `21`。
`21` 被读作 `"one 2"`, ` "one 1"` （`"一个二"` , ` "一个一"`) , 即 `1211`。

给定一个正整数 n（1 ≤ n ≤ 30），输出外观数列的第 n 项。

注意：整数序列中的每一项将表示为一个字符串。

示例1:

```
输入: 1
输出: "1"
解释：这是一个基本样例。
```

示例2:

```
输入: 4
输出: "1211"
解释：当 n = 3 时，序列是 "21"，其中我们有 "2" 和 "1" 两组，"2" 可以读作 "12"，也就是出现频次 = 1 而 值 = 2；类似 "1" 可以读作 "11"。所以答案是 "12" 和 "11" 组合在一起，也就是 "1211"。
```

### 题解

首先要理解题意！

输入 `n` ，输出的则是对 `n-1` 项的值的描述。这个描述可以理解为**"几个几"**，就是要**将相同的元素分成一组**，用“a个b"、"c个d”……来描述，最终输出的就是 abcd…… 这样的一个字符串。

#### 解法一（76ms)

每一项的内容是和前一项息息相关的，所以可以用**递归**的思想。

```javascript
/**
 * @param {number} n
 * @return {string}
 */
var countAndSay = function(n) {
    if(n == 1) return '1';
    var str = countAndSay(n-1);
    var prev = str[0];
    var count = 0;
    var result = '';
    for(var i=0;i<str.length + 1;i++){
        if(i != str.length){
            if(str[i] == prev) count ++;
            else{
                result += count + prev
                prev = str[i]
                count = 1;
            }
        }
        else
            result += count + prev
    }
    return result
};
```

#### 解法二（64ms)

还是**递归**的思想，但是是用**正则表达式**将连续相同的数字分成几组，对每组数据获取长度与其中一个元素，即可得到我们要的"几个几"

此处用到的正则为 内容为数字`(\d) `且重复正则第一个圆括号内匹配到的内容`\1 ` 连续0次或者无数次`*`的全局匹配`g`

```javascript
/**
 * @param {number} n
 * @return {string}
 */
var countAndSay = function(n) {
    if(n == 1) return '1';
    result = countAndSay(n-1).replace(/(\d)\1*/g, function(item){
        return item.length+item[0]
    });
    return result
};
```

#### 二刷（2021/07/22)

递归+双指针  （死活没想到正则 T T

```javascript
/**
 * @param {number} n
 * @return {string}
 */
var countAndSay = function(n) {
    var result = "1";
    if(n > 1){
        for(;n > 1;n --){
            result = getString(result);
        }
    }
    return result;
};
function getString(str){
    var newstr = "";
    for(var i = 0,j = 0;j < str.length;j ++){
        if(str[i] != str[j]){
            newstr += (j - i) + str[i];
            i = j;
            if(i == (str.length - 1)){
                newstr += (j - i + 1) + str[i];
                break;
            }
        }else{
            if(j == (str.length - 1)){
                newstr += (j - i + 1) + str[i];
                break;
            }
        }
    }
    return newstr;
}
```

整理了一下

```javascript
/**
 * @param {number} n
 * @return {string}
 */
var countAndSay = function(n) {
    if(n == 1) return "1";
    var str = countAndSay(n-1);
    var result = "";
    for(var i = 0,j = 0;j < str.length;j ++){
        if(str[i] != str[j]){
            result += (j - i) + str[i];
            i = j;
            if(i == (str.length - 1)){
                result += (j - i + 1) + str[i];
                break;
            }
        }else{
            if(j == (str.length - 1)){
                result += (j - i + 1) + str[i];
                break;
            }
        }
    }
    return result;
};
```






## 58、最后一个单词的长度

[题目地址](https://leetcode-cn.com/problems/length-of-last-word/)

### 题目描述
给定一个仅包含大小写字母和空格` ' '` 的字符串` s`，返回其最后一个单词的长度。

如果字符串从左向右滚动显示，那么最后一个单词就是最后出现的单词。

如果不存在最后一个单词，请返回 0 。

**说明**：一个单词是指仅由字母组成、不包含任何空格的**最大子字符串**。

示例:

```
输入: "Hello World"
输出: 5
```

### 题解

#### 解法（68ms)

从字符串最后一个元素开始查找，遇到空格就左移，直到遇到字母，并记录之后左移的位数，当再次遇到空格就跳出循环

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    var len = 0;
    for(var i=s.length-1;i>=0;i--){
        if(s[i] != ' '){
            len ++ 
        }else if(s[i] == ' '&&len>0){
            break;
        }
    }
    return len
};
```



## 67、二进制求和

[题目地址](https://leetcode-cn.com/problems/add-binary/)

### 题目描述
给定两个二进制字符串，返回他们的和（用二进制表示）。

输入为非空字符串且只包含数字` 1` 和` 0`。

示例1:

```
输入: a = "11", b = "1"
输出: "100"
```

示例2:

```
输入: a = "1010", b = "1011"
输出: "10101"
```

### 题解

#### 解法（80ms)

创建一个进位的变量，从字符串最后一个元素开始，对应的内容与进位相加，因为合只可能为0，1，2 (10)，3 (11)这四种情况，所以将` 和%2 `作为当前位的值，将` 和/2 `作为进位存放

```javascript
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function(a, b) {
    var more = 0;
    var result = '';
    for(var i=a.length-1,j=b.length-1;i>=0||j>=0;i--,j--){
        var sum = more;
        sum = sum + (i>=0 ? parseInt(a[i]) : 0);
        sum = sum + (j>=0 ? parseInt(b[j]) : 0);
        result = (sum%2).toString() + result;
        more = Math.floor(sum/2);
    }
    if(more == 1){
        result = more.toString() + result;
    }
    return result;
};
```



## 125、验证回文串

[题目地址](https://leetcode-cn.com/problems/valid-palindrome/)

### 题目描述
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明**：本题中，我们将空字符串定义为有效的回文串。

示例1:

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

示例2:

```
输入: "race a car"
输出: false
```

### 题解

#### 解法（64ms)

先将字符串去除数字字母外的其他元素，并统一字母的大小写。用**双指针**的思想，一个从左至右，一个从右至左，从两头逼近中间，对比头尾内容是否符合回文串，一旦有不一样的，就输出false

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    var str = s.replace(/[^0-9a-zA-Z]/g,"").toLowerCase()
    for(var i=0,j=str.length-1;i<j;i++,j--){
        if(str[i]!=str[j]){
            return false
        }
    }
    return true
};
```

#### 二刷（2021/07/12)

双指针 嗯 和以前一样的思路 也是和以前一样不会正则T T

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    if(s == '') return true;
    s = s.toLowerCase();
    s = s.replace(/[^a-z0-9]+/g,"");
    for(var i = 0,j = s.length - 1;i < j;i ++, j --){
        if(s[i] != s[j]){
            return false;
        }
    }
    return true;
};
```





## 242、有效的字母异位词

[题目地址](https://leetcode-cn.com/problems/valid-anagram/)

### 题目描述

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。

示例1:

```
输入: s = "anagram", t = "nagaram"
输出: true
```

示例2:

```
输入: s = "rat", t = "car"
输出: false
```



**提示**:

- `1 <= s.length, t.length <= 5 * 10^4`
- `s` 和 `t` 仅包含小写字母





**进阶**: 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？



### 题解

#### 解法（104ms)

想着满足 1.长度一样 2.每次字母出现的次数一样 ，就用Map对象存储字母出现的次数，因为长度是一样的，所以两个字符串可以同时进行次数的加减处理

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if(s.length != t.length) return false;
    var smap = new Map();
    for(var i = 0;i < s.length;i ++){
        var sword = s[i];
        var tword = t[i];
        if(smap.has(sword)){
            var count = smap.get(sword) + 1;
            if(count == 0){
                smap.delete(sword);
            }else{
                smap.set(sword,count);
            }
        }else{
            smap.set(sword,1)
        }

        if(smap.has(tword)){
            var count = smap.get(tword) - 1;
            if(count == 0){
                smap.delete(tword);
            }else{
                smap.set(tword,count);
            }
        }else{
            smap.set(tword,-1)
        }
    }
    if(smap.size > 0){
        return false;
    }
    return true;
};
```





## 344、反转字符串

[题目地址](https://leetcode-cn.com/problems/reverse-string/)

### 题目描述

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。

不要给另外的数组分配额外的空间，你必须**原地修改输入数组**、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 `ASCII` 码表中的可打印字符。

示例1:

```
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

示例2:

```
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```


### 题解

#### 解法（96ms)

使用双指针，一个指向头，一个指向尾，交换两者的值

```javascript
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    for(var i = 0,j = s.length - 1;i < j;i ++,j --){
        var temp = s[i];
        s[i] = s[j];
        s[j] = temp;
    }
};
```





## 387、字符串中的第一个唯一字符

[题目地址](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

### 题目描述

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

示例:

```
s = "leetcode"
返回 0

s = "loveleetcode"
返回 2
```

**提示**：你可以假定该字符串只包含小写字母。


### 题解

#### 解法（104ms)

使用Map对象存储每种字母的出现情况，本来是想存`[word , index]`，出现第二次第三次的时候就 `+1` ，但这个不好去界定它是`出现了多次`还是只是`index比较大`。所以就改成了出现第二次的时候，`将value改成-1`，这样在最终就可以直接获取第一个 `非-1` 的 `value`

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
    var myMap = new Map();
    for(var i = 0;i < s.length;i ++){
        var word = s[i];
        if(myMap.has(word)){
            myMap.set(word, -1);
        }else{
            myMap.set(word, i);
        }
    }
    for(const value of myMap.values()){
        if(value != -1){
            return value;
        }
    }
    return -1;
};
```



## 820、单词的压缩编码

[题目地址](https://leetcode-cn.com/problems/short-encoding-of-words/)

### 题目描述

单词数组 `words` 的 有效编码 由任意助记字符串 `s` 和下标数组 `indices` 组成，且满足：

- `words.length == indices.length`

- 助记字符串 `s` 以` '#'` 字符结尾

- 对于每个下标 `indices[i]` ，`s` 的一个从 `indices[i]` 开始、到下一个 `'#'` 字符结束（但不包括 `'#'`）的 子字符串 恰好与 `words[i]` 相等

  

  给你一个单词数组 `words` ，返回成功对 `words` 进行编码的最小助记字符串 `s` 的长度 。

示例1:

```
输入：words = ["time", "me", "bell"]
输出：10
解释：一组有效编码为 s = "time#bell#" 和 indices = [0, 2, 5] 。
words[0] = "time" ，s 开始于 indices[0] = 0 到下一个 '#' 结束的子字符串，如加粗部分所示 "time#bell#"
words[1] = "me" ，s 开始于 indices[1] = 2 到下一个 '#' 结束的子字符串，如加粗部分所示 "time#bell#"
words[2] = "bell" ，s 开始于 indices[2] = 5 到下一个 '#' 结束的子字符串，如加粗部分所示 "time#bell#"
```



示例2:

```
输入：words = ["t"]
输出：2
解释：一组有效编码为 s = "t#" 和 indices = [0] 。
```



提示：

1. `1 <= words.length <= 2000`
2. `1 <= words[i].length <= 7`
3. `words[i]` 仅由小写字母组成

### 题解

#### 解法（64ms)



```javascript
/**
 * @param {string[]} words
 * @return {number}
 */
var minimumLengthEncoding = function(words) {
    var map = new Set(words);
    for(word of map) {
        for (var i = 1; i < word.length; i++) {
            var tmp = word.slice(i);
            words.includes(tmp) && map.delete(tmp);
        }
    }
    var result = 0;
    map.forEach(v => result += v.length + 1)
    return result;
};
```




## 1071、字符串的最大公因子

[题目地址](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

### 题目描述
对于字符串` S` 和` T`，只有在` S = T + ... + T`（`T` 与自身连接 1 次或多次）时，我们才认定 “`T` 能除尽` S`”。

返回最长字符串` X`，要求满足` X` 能除尽` str1` 且` X` 能除尽` str2`。

示例1:

```
输入：str1 = "ABCABC", str2 = "ABC"
输出："ABC" 
```

示例2:

```
输入：str1 = "ABABAB", str2 = "ABAB"
输出："AB"
```

示例3:

```
输入：str1 = "LEET", str2 = "CODE"
输出：""
```

**提示**：
1. `1 <= str1.length <= 1000`
2. `1 <= str2.length <= 1000`
3. `str1[i]` 和` str2[i]` 为大写英文字母

### 题解

#### 解法（96ms)



```javascript
/**
 * @param {string} str1
 * @param {string} str2
 * @return {string}
 */
var gcdOfStrings = function(str1, str2) {
    if (str1 + str2 !== str2 + str1){
        return "";
    }
    var index = common(str1.length, str2.length);
    return str1.substring(0, index);
};
function common(len1, len2) {
    var result = len1 % len2;
    if(result === 0){
        return len2;
    }else{
        return common(len2, len1 % len2);
    }
}
```



## 面试题 01.06、字符串压缩

[题目地址](https://leetcode-cn.com/problems/compress-string-lcci/)

### 题目描述
字符串压缩。利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串`aabcccccaaa`会变为`a2b1c5a3`。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（a至z）。

示例1:

```
输入："aabcccccaaa"
输出："a2b1c5a3"
```

示例2:

```
输入："abbccd"
输出："abbccd"
解释："abbccd"压缩后为"a1b2c2d1"，比原字符串长度更长。
```

**提示**：
1. 字符串长度在[0, 50000]范围内。

### 题解

#### 解法一（84ms)

**正则表达式**匹配替换 ` /([a-zA-Z])\1*/g`

```javascript
/**
 * @param {string} S
 * @return {string}
 */
var compressString = function(S) {
    if(S.length == 0) return ""
    var result = S.replace(/([a-zA-Z])\1*/g, function(item){
        return item[0] + item.length;
    });
    return S.length > result.length ? result : S;
};
```

#### 解法二（60ms)

**双指针**

```javascript
/**
 * @param {string} S
 * @return {string}
 */
var compressString = function(S) {
    if(S.length == 0) return ""
    var result = "";
    for(var i = 0,j = 0;j < S.length;j ++){
        if(S[j + 1] != S[i]){
            result += S[i] + (j - i + 1);
            i = j + 1;
        }
    }
    return S.length > result.length ? result : S;
};
```