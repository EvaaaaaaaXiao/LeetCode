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




## 225、用队列实现栈

[题目地址](https://leetcode-cn.com/problems/implement-stack-using-queues/)

### 题目描述
使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

**注意**:

- 你只能使用队列的基本操作-- 也就是 push to back, peek/pop from front, size, 和 is empty 这些操作是合法的。
- 你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
- 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。



### 题解

#### 解法（64ms)

栈：**后进先出**，它的增加与删除操作都是在同一端执行的
队列：**先进先出**，它的增加操作在一端执行，删除操作在另一端执行

用数组实现栈

```javascript
/**
 * Initialize your data structure here.
 */
var MyStack = function() {
    this.stack = []
};

/**
 * Push element x onto stack. 
 * @param {number} x
 * @return {void}
 */
MyStack.prototype.push = function(x) {
    this.stack[this.stack.length] = x;
    return this.stack;
};

/**
 * Removes the element on top of the stack and returns that element.
 * @return {number}
 */
MyStack.prototype.pop = function() {
    let result = this.stack[this.stack.length - 1];
    this.stack.length --;
    return result;
};

/**
 * Get the top element.
 * @return {number}
 */
MyStack.prototype.top = function() {
    return this.stack[this.stack.length - 1];
};

/**
 * Returns whether the stack is empty.
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return this.stack.length == 0;
};

/**
 * Your MyStack object will be instantiated and called as such:
 * var obj = new MyStack()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.empty()
 */
```