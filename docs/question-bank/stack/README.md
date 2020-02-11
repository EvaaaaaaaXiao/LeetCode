## 20、有效的括号

[题目地址](https://leetcode-cn.com/problems/valid-parentheses/)

### 题目描述
给定一个只包括` '('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

示例1:

```
输入: "()[]{}"
输出: true
```

示例2:

```
输入: "([)]"
输出: false
```



### 题解

#### 解法（64ms)

将遇到的左括号都存入栈，遇到的右括号与栈底的左括号进行匹配……最终栈为空则为有效

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    if(s.length==0) return true
    if(s.length%2==1) return false
    var obj={
        '(': ')',
        '{': '}',
        '[': ']'
    }
    var arr=[];
    for(var i=0;i<s.length;i++){
        if(obj.hasOwnProperty(s[i]))
            arr.push(s[i]);
        else{
            if(s[i]!=obj[arr.pop()]) return false
            //此处将pop()直接放置条件中的写法有参考
        }
    }
    return !arr.length
};
```



### 思考

####  用正则实现

虽然不是第一反应，但有想过，所以还是想实现一下

参考LeetCode用户` 糖豆s `思路

用最小的`()`、`{}`、`[]`去匹配整个字符串并去除最小括号单位，一直到所有都被匹配完全，则为有效字符串

示例：

[ **( )** ] { [ **( )** ] }  => **[ ]** { **[ ]** } => **{ } **

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
	var re = /(\[\])|(\(\))|(\{\})/g
	while (re.test(s)) {
		s = s.replace(re, "")
	}
	return !s.length
};
```