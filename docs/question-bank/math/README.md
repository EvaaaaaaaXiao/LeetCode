## 7、整数反转

[题目地址](https://leetcode-cn.com/problems/reverse-integer/)

### 题目描述
给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 `[−231,  231 − 1]` ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

示例1:

```
输入：x = 123
输出：321
```

示例2:

```
输入：x = -123
输出：-321
```

示例3:

```
输入：x = 120
输出：21
```

示例4:

```
输入：x = 0
输出：0
```

**提示：**

- -231 <= x <= 231 - 1


### 题解

#### 解法（76ms)

有想过把整数转成数组或者字符串等其他数据格式，但感觉偏离题目了。

没有什么算法，就是纯数学题。

```javascript
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    var result = 0;
    var j = (x < 0) ? -1 : 1;
    x *= j;
    for(;x != 0;){
        result = result * 10 + x % 10;
        x = Math.floor(x / 10);
    }
    result *= j;
    if(result >= Math.pow(2 , 31) || result < Math.pow(-2 , 31)){
        return 0;
    }
    return result;
};
```



## 365、水壶问题

[题目地址](https://leetcode-cn.com/problems/water-and-jug-problem/)

### 题目描述
有两个容量分别为 x升 和 y升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 z升 的水？

如果可以，最后请用以上水壶中的一或两个来盛放取得的 z升 水。

你允许：

- 装满任意一个水壶
- 清空任意一个水壶
- 从一个水壶向另外一个水壶倒水，直到装满或者倒空

示例1:

```
输入: x = 3, y = 5, z = 4
输出: True
```

示例2:

```
输入: x = 2, y = 6, z = 5
输出: False
```


### 题解

#### 解法（80ms)



```javascript
/**
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @return {boolean}
 */
var canMeasureWater = function(x, y, z) {
    if( x + y < z) return false;
    else {
        if(x === 0 && y ===0){
            return z === 0;
        }else{
            return z % gcd(x, y) === 0;
        }
    }
};
function gcd(x, y){
    if(y === 0) return x;
    else{
        x %= y;
    }
    return gcd(y, x);
}
```



## 892、三维形体的表面积

[题目地址](https://leetcode-cn.com/problems/surface-area-of-3d-shapes/)

### 题目描述
在` N * N `的网格上，我们放置一些` 1 * 1 * 1 ` 的立方体。

每个值` v = grid[i][j]` 表示` v `个正方体叠放在对应单元格` (i, j) `上。

请你返回最终形体的表面积。

示例1:

```
输入：[[2]]
输出：10
```

示例2:

```
输入：[[1,2],[3,4]]
输出：34
```

示例3:

```
输入：[[1,0],[0,2]]
输出：16
```

示例4:

```
输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：32
```

示例5:

```
输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：46
```

**提示**：

- `1 <= N <= 50`
- `0 <= grid[i][j] <= 50`


### 题解

#### 解法（80ms)



```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var surfaceArea = function(grid) {
    var [n, result, r, c] = [grid.length, 0];
    if (!n) return result;
    for (r = 0; r < n; r++) {
        for (c = 0; c < n; c++) {
            if (grid[r][c] > 0) result += 4 * grid[r][c] + 2;
            if (r - 1 >= 0) result -= 2 * (Math.min(grid[r][c], grid[r - 1][c]));
            if (c - 1 >= 0) result -= 2 * (Math.min(grid[r][c], grid[r][c - 1]));
        }
    }
    return result;
};
```



## 836、矩形重叠

[题目地址](https://leetcode-cn.com/problems/rectangle-overlap/)

### 题目描述
矩形以列表` [x1, y1, x2, y2]` 的形式表示，其中` (x1, y1) `为左下角的坐标，`(x2, y2) `是右上角的坐标。

如果相交的面积为正，则称两矩形重叠。需要明确的是，只在角或边接触的两个矩形不构成重叠。

给出两个矩形，判断它们是否重叠并返回结果。

示例1:

```
输入：rec1 = [0,0,2,2], rec2 = [1,1,3,3]
输出：true
```

示例2:

```
输入：rec1 = [0,0,1,1], rec2 = [1,0,2,1]
输出：false
```

**提示**：

1. 两个矩形` rec1` 和 `rec2` 都以含有四个整数的列表的形式给出。
2. 矩形中的所有坐标都处于 `-10^9` 和` 10^9 `之间。
3. `x` 轴默认指向右，`y` 轴默认指向上。
4. 你可以仅考虑矩形是正放的情况。


### 题解

#### 解法（64ms)

将上下左右不重叠的情况列出来，结果反向

```javascript
/**
 * @param {number[]} rec1
 * @param {number[]} rec2
 * @return {boolean}
 */
var isRectangleOverlap = function(rec1, rec2) {
    var result = rec2[2] <= rec1[0] || rec2[0] >= rec1[2] ||rec2[1] >= rec1[3] || rec2[3] <= rec1[1];
    return !result;
};
```



## 914、卡牌分组

[题目地址](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/)

### 题目描述
给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 `X`，使我们可以将整副牌按下述规则分成 1 组或更多组：

- 每组都有` X` 张牌。
- 组内所有的牌上都写着相同的整数。
仅当你可选的` X >= 2` 时返回` true`。

示例1:

```
输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
```

示例2:

```
输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。
```

示例3:

```
输入：[1]
输出：false
解释：没有满足要求的分组。
```

示例4:

```
输入：[1,1]
输出：true
解释：可行的分组是 [1,1]
```

示例5:

```
输入：[1,1,2,2,2,2]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[2,2]
```

**提示**：

1. `1 <= deck.length <= 10000`
2. `0 <= deck[i] < 10000`


### 题解

#### 解法（68ms)



```javascript
/**
 * @param {number[]} deck
 * @return {boolean}
 */
var hasGroupsSizeX = function(deck) {
    if(deck.length === 0) return false;
    var arr = [];
    deck.forEach((item)=>{
        arr[item] = arr[item] ? arr[item] + 1 : 1;
    })
    
    var temp = [...new Set(arr)];
    var result = 1;
    if(temp.length) result = temp[0];
    
    while(temp.length > 1){
        var num1 = temp[0], num2 = temp[1];
        result = gcd(num1,num2)
        if(result == 1){
            return false
        } else {
            temp.splice(0, 2, result)
        }
    }
    return result > 1 ? true : false;
};
function gcd(x,y){
    var a = x%y;
    while(a){
        x = y;
        y = a;
        a = x%y;
    }
    return y;
}
```





## 1103、分糖果 II

[题目地址](https://leetcode-cn.com/problems/distribute-candies-to-people/)

### 题目描述
排排坐，分糖果。

我们买了一些糖果` candies`，打算把它们分给排好队的` n = num_people` 个小朋友。

给第一个小朋友 1 颗糖果，第二个小朋友 2 颗，依此类推，直到给最后一个小朋友` n` 颗糖果。

然后，我们再回到队伍的起点，给第一个小朋友` n + 1` 颗糖果，第二个小朋友` n + 2` 颗，依此类推，直到给最后一个小朋友` 2 * n `颗糖果。

重复上述过程（每次都比上一次多给出一颗糖果，当到达队伍终点后再次从队伍起点开始），直到我们分完所有的糖果。注意，就算我们手中的剩下糖果数不够（不比前一次发出的糖果多），这些糖果也会全部发给当前的小朋友。

返回一个长度为` num_people`、元素之和为` candies `的数组，以表示糖果的最终分发情况（即` ans[i] `表示第` i `个小朋友分到的糖果数）。

示例1:

```
输入：candies = 7, num_people = 4
输出：[1,2,3,1]
解释：
第一次，ans[0] += 1，数组变为 [1,0,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0,0]。
第三次，ans[2] += 3，数组变为 [1,2,3,0]。
第四次，ans[3] += 1（因为此时只剩下 1 颗糖果），最终数组变为 [1,2,3,1]。
```

示例2:

```
输入：candies = 10, num_people = 3
输出：[5,2,3]
解释：
第一次，ans[0] += 1，数组变为 [1,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0]。
第三次，ans[2] += 3，数组变为 [1,2,3]。
第四次，ans[0] += 4，最终数组变为 [5,2,3]。
```

**提示**：

- `1 <= candies <= 10^9`
- `1 <= num_people <= 1000`


### 题解

#### 解法（64ms)

单纯的发糖过程

```javascript
/**
 * @param {number} candies
 * @param {number} num_people
 * @return {number[]}
 */
var distributeCandies = function(candies, num_people) {
    var result = new Array(num_people).fill(0);
    for (var i = 0,j = 1;i < num_people;) {
        if (j <= candies) {
            result[i] += + j;
            candies -= j;
        } else {
            result[i] += candies;
            break;
        }
        i++;
        j++;
        if (i == num_people) {
            i = 0
        }
    }
    return result;
};
```





## 面试题62、圆圈中最后剩下的数字

[题目地址](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

### 题目描述
0,1……n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

示例1:

```
输入: n = 5, m = 3
输出: 3
```

示例2:

```
输入: n = 10, m = 17
输出: 2
```

**限制**：

- `1 <= n <= 10^5`
- `1 <= m <= 10^6`


### 题解

#### 解法（72ms)

递归

```javascript
/**
 * @param {number} n
 * @param {number} m
 * @return {number}
 */
var lastRemaining = function(n, m) {
    var result = 0;
    for(var i = 2; i <= n; i++) {
        result = (m + result) % i;
    }
    return result;
};
```

