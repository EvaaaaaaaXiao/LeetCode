## 1、两数之和 

[题目地址](https://leetcode-cn.com/problems/two-sum/)

### 题目描述
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



### 题解

#### 解法一（192ms)

循环数组，查看数组中是否存在target和当前数值的差值，

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
	var targetIndex = 0;
	var result = [];
	for(key in nums){
	    targetIndex = nums.indexOf(target - nums[key]);
	    if(targetIndex > -1){
            if(targetIndex != key){
                result.push(key);
                result.push(targetIndex);
                break;
              }
		}
	}
	return result;
};
```



#### 解法二（56ms）

循环数组，边找边存，将数组存成{nums[i]: i}的对象

但单纯针对这道题说的“每种输入只会对应一个答案”

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var obj = {}
    var len = nums.length
    for(var i=0; i<len; i++){
        if(obj.hasOwnProperty(target-nums[i])){
            return [obj[target-nums[i]],i]
        }else{
            obj[nums[i]]=i
        }
    }
};
```



### 思考

#### Map vs Object

- 当存储的是简单数据类型，并且 **key 都为字符串或者整数或者 Symbol** 的时候，优先使用 Object 。因为Object可以使用字符变量的方式创建，更加高效
- 当需要在单独的逻辑中访问属性或者元素的时候，应该使用 Object
- JSON 直接支持 Object，但不支持 Map
- Map 是纯粹的 hash， 而 Object 还存在一些其他内在逻辑，所以在执行 delete 的时候会有性能问题。所以**写入删除密集**的情况应该使用 Map。
- Map 会**按照插入顺序保持元素的顺序**，而Object做不到。
- Map 在**存储大量元素**的时候性能表现更好，特别是在代码执行时**不能确定 key 的类型**的情况。



#### 二刷（2021/07/08）

开始尝试用Map对象啦，思路就像上面的解法二一样，不多说惹。

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var myMap = new Map();
    for(var i = 0;i < nums.length;i ++){
        if(myMap.has(target - nums[i])){
            return [myMap.get(target - nums[i]), i];
        }else{
            myMap.set(nums[i],i);
        }
    }
};
```





## 26、删除排序数组中的重复项

[题目地址](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

### 题目描述
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例:

```
给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
```



### 题解

#### 解法一（100ms)

题目中的**“排序数组”**意味着每轮对比无需从头到尾，他一定是从小到大或是从大到小排序的

使用 splice 将重复的元素直接从原数组中移除，最终留下的就是所需的新数组内容

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    for(var i=0;i<nums.length;){
        if(nums[i] == nums[i+1]){
            nums.splice(i+1,1)
        }else{
            i++
        }
    }
    return nums.length
};
```



#### 解法二（80ms）

**双指针算法**

题目中的**“前n个元素被修改，不用考虑超出新长度后的元素”**意味着无需将原数组处理成和输出数组一样，只要将不重复的内容赋值或者移动到前n位就行，没必要删去多余的数组内容，题目标题略有误导性

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    var index = 0
    var len = nums.length
    for(var i=1;i<len;i++){
        if(nums[i] != nums[i-1]){
            index++
            nums[index]=nums[i]
        }
    }
    return index+1
};
```



#### 二刷（2021/05/27）

这次有自己想到双指针**（解法二）**，不过一开始蒙头瞎写居然搞了两个for循环…结果还是没审好题，**只要返回长度就好**

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    var i = 0;
    for(var j = 1;j < nums.length;j++){
        if(nums[j] != nums[i]){
            i ++;
            nums[i] = nums[j];
        }
    }
    return ++i;
};
```



## 27、移除元素

[题目地址](https://leetcode-cn.com/problems/remove-element/)

### 题目描述
给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例1:

```
给定 nums = [3,2,2,3], val = 3,

函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。

你不需要考虑数组中超出新长度后面的元素。
```

示例2:

```
给定 nums = [0,1,2,2,3,0,4,2], val = 2,

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。
```



### 题解

#### 解法（64ms)

用非val值的元素去覆盖数组内容

```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    var result = 0;
    for(var i=0;i<nums.length;i++){
        if(nums[i] != val){
            nums[result] = nums[i]
            result++
        }
    }
    return result
};
```



## 35、搜索插入位置

[题目地址](https://leetcode-cn.com/problems/search-insert-position/)

### 题目描述
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

示例1:

```
输入: [1,3,5,6], 5
输出: 2
```

示例2:

```
输入: [1,3,5,6], 7
输出: 4
```



### 题解

#### 解法一（68ms)

没头脑的比较值

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
    var result = nums.length;
    for(var i=0;i<nums.length;){
        if(nums[i] < target){
            i++;
        }
        else {
            result = i;
            break;
        }
    }
    return result
};
```



#### 解法二（104ms)

用**二分查找**的思想。
- 首先，将`target`大于最右的元素作为一个特例列出来；
- 其次，锁定查找的初始范围，为`[ left , right ]`，中位数`middle`为` (left + right) >>> 1` （其中 `>>>1`为二进制右移 1 位，可以理解为除以2），此处是**向下取整**
- 若 nums[middle] 小于target，查找范围变为`[ middle+1 , right]`，也就是中位数左侧包括中位数都是远小于target的，可以**一并排除**
- 若 nums[middle] 大于或是等于target，查找范围变为`[ left , middle]`，也就是中位数右侧都是远大于target的，可以**一并排除**
- 一直到`left == right`为止
- 最终return left 还是 right 都可以，因为此时两个值是一样的

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
    var left = 0, right = nums.length -1;
    if(target > nums[right]) return right + 1;
    while(left < right){
        var middle = Math.floor((left + right) >>> 1);
        if(target > nums[middle]){
            left = middle + 1
        }
        else{
            right = middle
        }
    }
    return right
};
```



## 36、有效的数独

[题目地址](https://leetcode-cn.com/problems/valid-sudoku/)

### 题目描述
请你判断一个 `9x9` 的数独是否有效。只需要 **根据以下规则** ，验证已经填入的数字是否有效即可。

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**注意：**

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。

示例1:

```
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
```

示例2:

```
输入：board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：false

解释：除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。 但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**提示：**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` 是一位数字或者 `'.'`


### 题解

#### 解法一（96ms)

没头脑的按照3种规则比较值是否有效，因为数据量9x9是固定的，所以处理的量也不大

```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
    var rowMap = new Map();
    var columMap = new Map();
    var blockMap = new Map();
    for(var i = 0;i < 9;i ++){
        rowMap.clear();
        for(var j = 0;j < 9;j ++){
            var num = board[i][j];
            if(num != '.'){
                if(rowMap.has(num)){
                    return false;
                }else{
                    rowMap.set(num, j);
                }
            }
        }
    }
    for(var i = 0;i < 9;i ++){
        columMap.clear();
        for(var j = 0;j < 9;j ++){
            var num = board[j][i];
            if(num != '.'){
                if(columMap.has(num)){
                    return false;
                }else{
                    columMap.set(num, j);
                }
            }
        }
    }
    for(var row = 0;row < 3;row ++){
        for(var column = 0;column < 3;column ++){
          blockMap.clear();
          for(var i = 0;i < 3;i ++){
            for(var j = 0;j < 3;j ++){
                var num = board[row*3 + i][j + column*3];
                if(num != '.'){
                    if(blockMap.has(num)){
                        return false;
                    }else{
                        blockMap.set(num, j);
                    }
                }
            }
          }
        }
    }
    return true;
};
```



#### 解法二（72ms)

看到唯一又想到了Map类型，最近出现太多次了……

但是想了下1-9的数字肯定会被后面相同值的覆盖，所以根据不同的规则制定命名规则确保唯一性，还是很无脑……


```javascript
/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
    var myMap = new Map();
    for(var i = 0;i < 9;i ++){
        for(var j = 0;j < 9;j ++){
            var num = board[i][j];
            if(num != '.'){
                var rowKey = j + 'r' + num; // 每行的key规则
                var columnKey = i + 'c' + num; // 每列的key规则
                var blockKey = Math.floor(j/3) + 'b' + Math.floor(i/3) + 'b' + num; // 每块的key规则
                if(myMap.has(rowKey) || myMap.has(columnKey) || myMap.has(blockKey)){
                    return false;
                }
                myMap.set(rowKey, 1);
                myMap.set(columnKey, 1);
                myMap.set(blockKey, 1);
            }
        }
    }
    return true;
};
```


## 48、旋转图像

[题目地址](https://leetcode-cn.com/problems/rotate-image/)

### 题目描述
给定一个 *n × n* 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 `原地` 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

示例1:

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

示例2:

```
输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

示例3:

```
输入：matrix = [[1]]
输出：[[1]]
```

示例4:

```
输入：matrix = [[1,2],[3,4]]
输出：[[3,1],[4,2]]
```

**提示**：
- `matrix.length == n`
- `matrix[i].length == n`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

### 题解

#### 解法（68 ms)

最简单肯定就是用新数组存旋转后

原地的话，让我想到了 `189、旋转数组` 里的环。
- 四边形所以每个环的长度就是固定的4
- 环的个数为四等分矩形后左上角的小矩形的元素个数，如果长度是奇数，那么为左上角小矩形完整元素个数+1


```javascript
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var rotate = function(matrix) {
    var len = Math.ceil(matrix.length / 2);
    var isOdd = (matrix.length % 2 == 0) ? 0 : 1 ; // 如果数组长度为奇数
    var holdvalue = 0;
    var oldvalue = 0;
    var newi = 0, newj = 0;
    var oldi = 0, oldj = 0;
    for(var row = 0;row < (len - isOdd);row ++){
        for(var column = 0;column < len;column ++){
            oldi = row,oldj = column;
            oldvalue = matrix[oldi][oldj];
            for(var count = 0;count < 4;count ++){
                newi = oldj;
                newj = matrix.length - oldi - 1;
                holdvalue = matrix[newi][newj];
                matrix[newi][newj] = oldvalue;
                oldvalue = holdvalue;
                oldi = newi;
                oldj = newj;
            }
        }
    }
};
```

看了官方题解，傻了我，4个的循环赋值可以直接写出来…何必搞那么多变量名…

**整理后**

```javascript
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var rotate = function(matrix) {
    var len = matrix.length;
    var isOdd = (len % 2 == 0) ? 0 : 1 ;
    var holdvalue = 0;
    for(var row = 0;row < Math.ceil(len / 2) - isOdd;row ++){
        for(var column = 0;column < Math.ceil(len / 2);column ++){
            holdvalue = matrix[row][column];
            matrix[row][column] = matrix[len - column - 1][row];
            matrix[len - column - 1][row] = matrix[len - row - 1][len - column - 1];
            matrix[len - row - 1][len - column - 1] = matrix[column][len - row - 1];
            matrix[column][len - row - 1] = holdvalue;
        }
    }
};
```

## 66、 加一

[题目地址](https://leetcode-cn.com/problems/plus-one/)

### 题目描述
给定一个由**整数**组成的**非空**数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例1:

```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

示例2:

```
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
```

示例3:

```
输入：digits = [0]
输出：[1]
```

**提示**：
1. `1 <= digits.length <= 100`
2. `0 <= digits[i] <= 9` 

### 题解

#### 解法（68 ms)



```javascript
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function(digits) {
    var p = 1;
    for(var i = digits.length - 1;i > -1;){
        digits[i] = (digits[i] + p) % 10;
        p = (digits[i] == 0) ? 1 : 0;
        if(p == 0){
            break;
        }else{
            if(i == 0) {
                digits.unshift(1)
            }
        }
        i --;
    }
    return digits;
};
```



## 122、 买卖股票的最佳时机 II

[题目地址](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

### 题目描述
给定一个数组 `prices` ，其中 `prices[i]` 是一支给定股票第` i `天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意**：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例1:

```
输入: prices = [7,1,5,3,6,4]
输出: 7
解释: 
在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

示例2:

```
输入: prices = [1,2,3,4,5]
输出: 4
解释: 
在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

示例3:

```
输入: prices = [7,6,4,3,1]
输出: 0
解释: 
在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**提示**：
1. `1 <= prices.length <= 3 * 104`
2. `0 <= prices[i] <= 104` 

### 题解

#### 解法（92 ms)

1. 下降的不买入
2. 上升的买入，持续上升就继续持有，一直到出现下降卖出
因为一直只有一支股票，所以就是单纯的把所有上升的幅度都加起来就好了，也就是把所有数组里**后一个减去前一个为正**的差值都相加。

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    var num = 0;
    var buy = 0;
    for(var sale = 1;sale < prices.length;sale ++){
        if(prices[buy] < prices[sale]){
            num += (prices[sale] - prices[buy]);   
        }
        buy = sale;
    }
    return num;
};
```



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



#### 二刷（2021/07/06）

想到了两个相同的数字抵消，但没有记住能实现这个的叫什么，所以暂且用加减来抵消，使用了额外的空间……

**默念三遍：位运算 位运算 位运算**

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    var obj = {};
    var result = 0;
    for(var i = 0;i < nums.length;i ++){
        if(obj[nums[i]]) result -= nums[i];
        else{
            result += nums[i];
            obj[nums[i]] = true;
        }
    }
    return result;
};
```



## 169、多数元素

[题目地址](https://leetcode-cn.com/problems/majority-element/)

### 题目描述

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于` ⌊ n/2 ⌋ `的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例1:

```
输入: [3,2,3]
输出: 3
```

示例2:

```
输入: [2,2,1,1,1,2,2]
输出: 2
```

### 题解

#### 解法（80ms)



```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
    var count = 1;
    var result = nums[0];
    for(var i = 1; i < nums.length; i++) {
        if(count === 0) {
            result = nums[i];
            count = 1;
        }else if(nums[i] === result) {
            count++;
        }else {
            count--;
        }
    }
    return result;
};
```



## 189、 旋转数组

[题目地址](https://leetcode-cn.com/problems/rotate-array/)

### 题目描述
给定一个数组，将数组中的元素向右移动`k`个位置，其中`k`是非负数

**进阶**：
- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
- 你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？

示例1:

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

示例2:

```
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

**提示**：
1. `1 <= nums.length <= 2 * 10^4`
2. `-2^31 <= nums[i] <= 2^31 - 1` 
3. `0 <= k <= 10^5`

### 题解

#### 解法一（132ms）

将原数组的值存下来，新位置 =（原位置+移动值）% 数组长度

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var rotate = function(nums, k) {
    var len = nums.length;
    var n = [];
    for(var i = 0;i < len;i ++){
        n[i] = nums[i];
    }
    for(var i = 0;i < len;i ++){
        nums[(i + k) % len] = n[i];
    }
};
```



#### 解法二（132ms）

从第0个元素开始，寻找它的新位置，再帮它新位置上的原数组值寻找它的新位置……

**提交错误：没有考虑数组长度是k的倍数的情况，会重复一个小循环**，所以用一个对象保存已经变过的位置，如果是已经变过的，就处理它后面一位。

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var rotate = function(nums, k) {
    var len = nums.length;
    var j = k % len;
    if(j == 0 || len == 1) return;

    j = 0;
    var j_value;
    var i_value = nums[j];
    var changed = {};
    for(var count = 0;count < len;count ++){
        j = (j + k) % len;
        if(changed[j]){
            j ++;
            i_value = nums[j];
            count --;
        }else{
            changed[j] = true;
            j_value = nums[j];
            nums[j] = i_value;
            i_value = j_value;
        }
    }
};
```



## 217、 存在重复元素

[题目地址](https://leetcode-cn.com/problems/contains-duplicate/)

### 题目描述

给定一个整数数组，判断是否存在重复元素。

如果存在一值在数组中出现至少两次，函数返回`true`。如果数组中每个元素都不相同，则返回`false`。

示例1:

```
输入: [1,2,3,1]
输出: true
```

示例2:

```
输入: [1,2,3,4]
输出: false
```

示例3:

```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```


### 题解

#### 解法（96ms)



```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var containsDuplicate = function(nums) {
    var len = nums.length;
    if(len == 1) return false;

    var obj = {};
    var result = false;
    for(var i = 0;i < len;i ++){
        if(obj[nums[i]]){
            result = true;
            break;
        }else{
            obj[nums[i]] = true;
        }
    }
    return result;
};
```



## 283、移动零

[题目地址](https://leetcode-cn.com/problems/move-zeroes/)

### 题目描述

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明**

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。


### 题解

#### 解法一（100ms)

用双指针，`i` 指向重新赋值后的非零元素，`j` 指向原数组中的非零元素，当 `j` 走完了所有元素，剩下还未重新赋值后的数组元素均为`0




```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    for(var i = 0,j = 0;i < nums.length;j ++){
        if(nums[j] != 0){
            if(j < nums.length){
                nums[i] = nums[j];
            }else{
                nums[i] = 0;
            }
            i ++;
        }
    }
};
```

#### 解法二（80ms)

看了一下别人的题解，自己的解法一不太符合题意里的 `移动`

解法二还是双指针，如果 `i` 指向的是零， `j` 指向的是非零，则进行交换

**`交换用的是异或`**


```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    for(var i = 0, j = 1;j < nums.length;){
        if(nums[i] == 0){
            if(nums[j] != 0){
                nums[i] ^= nums[j];
                nums[j] ^= nums[i];
                nums[i] ^= nums[j];
                i++;
            }
        }else{
            i++;
        }
        j ++;
    }
};
```


## 350、两个数组的交集II

[题目地址](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

### 题目描述

给定两个数组，编写一个函数来计算它们的交集。

示例1:

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

示例2:

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```

**说明：**
- 输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
- 我们可以不考虑输出结果的顺序。

**进阶：**
- 如果给定的数组已经排好序呢？你将如何优化你的算法？
- 如果*nums1*的大小比*nums2*小很多，哪种方法更优？
- 如果*nums2*的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

### 题解

#### 解法（76ms)

循环第一个数组，将数组的元素作为key，以元素出现的次数作为value，再循环第二个数组，如果数组中的元素在map对象中存在，则将value减1，并且存入结果数组中，直到value为0，将key从map中删除

```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function(nums1, nums2) {
    var myMap = new Map();
    var result = [];
    for(var i = 0;i < nums1.length;i ++){
        var n = nums1[i];
        if(myMap.has(n)){
            myMap.set(n, myMap.get(n)+1);
        }else{
            myMap.set(n, 1);
        }
    }
    for(var j = 0;j < nums2.length;j ++){
        var m = nums2[j];
        if(myMap.has(m)){
            if(myMap.get(m) == 0){
                myMap.delete(m);
            }else{
                result.push(m)
                myMap.set(m, myMap.get(m)-1);
            }
        }else{
            //
        }
    }
    return result;
}
```



## 695、岛屿的最大面积

[题目地址](https://leetcode-cn.com/problems/max-area-of-island/)

### 题目描述

给定一个包含了一些 0 和 1的非空二维数组` grid` , 一个**岛屿** 是由四个方向 (水平或垂直) 的` 1` (代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为0。)

示例1:

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

对于上面这个给定矩阵应返回` 6`。注意答案不应该是11，因为岛屿只能包含水平或垂直的四个方向的‘1’。

示例2:

```
[[0,0,0,0,0,0,0,0]]
```

对于上面这个给定的矩阵, 返回` 0`。

**注意**: 给定的矩阵`grid` 的长度和宽度都不超过 50。

### 题解

#### 解法（100ms)



```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxAreaOfIsland = function(grid) {
    var len = grid.length;
    var len0 = grid[0].length;
    var result = 0;
    for(var i=0;i < len;i++){
        for(var j=0;j < len0;j++){
            if(grid[i][j] === 1){
                result = Math.max(result,countArea(grid, i, j, len, len0));
            }
        }
    }
    return result;
};

var countArea = (grid, i, j, x, y) =>{
    if(i<0 || i>=x || j<0 || j>=y || grid[i][j]==0) return 0;
    grid[i][j] = 0;
    var count = 1;
    count += countArea(grid, i+1, j, x, y);
    count += countArea(grid, i-1, j, x, y);
    count += countArea(grid, i, j+1, x, y);
    count += countArea(grid, i, j-1, x, y);
    return count
}
```



## 945、使数组唯一的最小增量

[题目地址](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/)

### 题目描述

给定整数数组 A，每次 move 操作将会选择任意 `A[i]`，并将其递增` 1`。

返回使` A `中的每个值都是唯一的最少操作次数。

示例1:

```
输入：[1,2,2]
输出：1
解释：经过一次 move 操作，数组将变为 [1, 2, 3]。
```

示例2:

```
输入：[3,2,1,2,1,7]
输出：6
解释：经过 6 次 move 操作，数组将变为 [3, 4, 1, 2, 5, 7]。
可以看出 5 次或 5 次以下的 move 操作是不能让数组的每个值唯一的。
```

**提示**: 

1. `0 <= A.length <= 40000`
2. `0 <= A[i] < 40000`

### 题解

#### 解法（160ms)



```javascript
/**
 * @param {number[]} A
 * @return {number}
 */
var minIncrementForUnique = function(A) {
    if(A.length === 0) return 0;
    A.sort((a,b) => a - b);
    var result = 0;
    for(var i = 0;i < A.length-1;i ++){
        if(A[i] >= A[i+1]){
            result = result + A[i] - A[i+1] + 1;
            A[i+1] = A[i] + 1;
        }
    }
    return result;
};
```



## 999、车的可用捕获量

[题目地址](https://leetcode-cn.com/problems/available-captures-for-rook/)

### 题目描述
在一个 8 x 8 的棋盘上，有一个白色车（rook）。也可能有空方块，白色的象（bishop）和黑色的卒（pawn）。它们分别以字符 “R”，“.”，“B” 和 “p” 给出。大写字符表示白棋，小写字符表示黑棋。

车按国际象棋中的规则移动：它选择四个基本方向中的一个（北，东，西和南），然后朝那个方向移动，直到它选择停止、到达棋盘的边缘或移动到同一方格来捕获该方格上颜色相反的卒。另外，车不能与其他友方（白色）象进入同一个方格。

返回车能够在一次移动中捕获到的卒的数量。

示例1:

```
输入：[[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：3
解释：
在本例中，车能够捕获所有的卒。
```

示例2:

```
输入：[[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：0
解释：
象阻止了车捕获任何卒。
```

示例3:

```
输入：[[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：3
解释： 
车可以捕获位置 b5，d6 和 f5 的卒。
```

**提示**：
1. `board.length == board[i].length == 8`
2. `board[i][j]` 可以是` 'R'`，`'.'`，`'B'` 或` 'p'`
3. 只有一个格子上存在` board[i][j] == 'R'`

### 题解

#### 解法（68ms)



```javascript
/**
 * @param {character[][]} board
 * @return {number}
 */
var numRookCaptures = function(board) {
    var result = 0;
    for (var i = 0; i < 8; i++) {
        for (var j = 0; j < 8; j++) {
            if (board[i][j] == 'R') {
                var m = i;
                var n = j;
                while (--m >= 0) {   
                    if (board[m][n] == 'B') break;
                    if (board[m][n] == 'p') {
                        result++;
                        break;
                    }
                }
                m = i;
                while (++m < 8) {
                    if (board[m][n] == 'B') break;
                    if (board[m][n] == 'p') {
                        result++;
                        break;
                    }
                }
                m = i;
                while (--n >= 0) {
                    if (board[m][n] == 'B') break;
                    if (board[m][n] == 'p') {
                        result++;
                        break;
                    }
                }
                n = j;
                while (++n < 8) {
                    if (board[m][n] == 'B') break;
                    if (board[m][n] == 'p') {
                        result++;
                        break;
                    }
                }
                return result;
            }
        }
    }
};
```



## 1013、将数组分成和相等的三个部分

[题目地址](https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/)

### 题目描述
给你一个整数数组` A`，只有可以将其划分为三个和相等的非空部分时才返回` true`，否则返回 `false`。

形式上，如果可以找出索引` i+1 < j `且满足` (A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1]) `就可以将数组三等分。

示例1:

```
输出：[0,2,1,-6,6,-7,9,1,2,0,1]
输出：true
解释：0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1  
```

示例2:

```
输入：[0,2,1,-6,6,7,9,-1,2,0,1]
输出：false 
```

示例3:

```
输入：[3,3,6,5,-2,2,5,1,-9,4]
输出：true
解释：3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
```

**提示**：
1. `3 <= A.length <= 50000`
2. `-10^4 <= A[i] <= 10^4`

### 题解

#### 解法（68ms)

算出总和的三分之一的值，边循环边计算，等到和为三分之一的值后，截取数组

```javascript
/**
 * @param {number[]} A
 * @return {boolean}
 */
var canThreePartsEqualSum = function(A) {
    var sum = 0;
    for (var i = 0;i < A.length;i ++) {
        sum += A[i];
    }
    var target = sum / 3;  
    var count = 0;
    var remain = 0;
    for(var j = 0;j < A.length;j ++){
        remain += A[j];
        if(remain === target){
            count++;
            remain = 0;
        }
    }
    if (target === 0 && count > 3) return true 
    return count === 3
};
```



## 1160、拼写单词

[题目地址](https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters/)

### 题目描述
给你一份『词汇表』（字符串数组）` words` 和一张『字母表』（字符串）` chars`。

假如你可以用` chars` 中的『字母』（字符）拼写出` words` 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。

注意：每次拼写时，`chars` 中的每个字母都只能用一次。

返回词汇表 `words` 中你掌握的所有单词的**长度之和**。

示例1:

```
输入：words = ["cat","bt","hat","tree"], chars = "atach"
输出：6
解释： 
可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。
```

示例2:

```
输入：words = ["hello","world","leetcode"], chars = "welldonehoneyr"
输出：10
解释：
可以形成字符串 "hello" 和 "world"，所以答案是 5 + 5 = 10。
```

**提示**：
1. `1 <= words.length <= 1000`
2. `1 <= words[i].length, chars.length <= 100`
3. 所有字符串中都仅包含小写英文字母

### 题解

#### 解法（144ms)



```javascript
/**
 * @param {string[]} words
 * @param {string} chars
 * @return {number}
 */
var countCharacters = function(words, chars) {
    var result = 0;
    for(item of words){
        var str = chars;
        var list = item.split('');
        while(list.length > 0) {
            var word = list[0];
            if(str.indexOf(word) > -1){
                list.shift();
                str = str.replace(word,'');
                continue;
            }else{
                break;
            }
        }
        if(list.length === 0){
            result += item.length;
        }
    }
    return result;
}
```



## 面试题 10.01、合并排序的数组

[题目地址](https://leetcode-cn.com/problems/sorted-merge-lcci/)

### 题目描述
给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。

初始化 A 和 B 的元素数量分别为 m 和 n。

示例:

```
输入:
A = [1,2,3,0,0,0], m = 3
B = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
```



### 题解

#### 解法（68ms)

用双指针的思想

当`m`与`n`都不为零时，由于A数组已经预留好了空间，所以考虑从后往前比较两个数组的值，将值较大的填入A数组后部。若从前往后比较，必定会覆盖A数组原元素，很不方便

当`m = 0`跳出循环时，依次填入B数组剩下的排序内容即可

当`n = 0`跳出循环时，无需再去动A数组，它本身就是已经排好序的了

```javascript
/**
 * @param {number[]} A
 * @param {number} m
 * @param {number[]} B
 * @param {number} n
 * @return {void} Do not return anything, modify A in-place instead.
 */
var merge = function(A, m, B, n) {
    let len = A.length - 1;
    while(m && n){
        if(A[m - 1] > B[n - 1]){
            A[len] = A[m - 1];
            m--;
        }else{
            A[len] = B[n - 1];
            n--;
        }
        len--;
    }
    while(n){
        A[len] = B[n - 1];
        n--;
        len--;
    }
};
```



## 面试题57 - II、和为s的连续正数序列

[题目地址](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

### 题目描述
输入一个正整数` target` ，输出所有和为` target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

示例1:

```
输入：target = 9
输出：[[2,3,4],[4,5]]
```

示例2:

```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

**限制**：

- 1 <= target <= 10^5


### 题解

#### 解法（80ms)

在` [1, 中位数+1]`的范围中，用**滑动窗口**计算窗口内容的和，将其与`target`进行比较。

**注意：要对每组符合的数组进行深拷贝**

```javascript
/**
 * @param {number} target
 * @return {number[][]}
 */
var findContinuousSequence = function(target) {
    if(target<3) return [];
    var index = Math.floor(target/2) + 1, sum = 0, item = [], result = [];
    for(var i = 1;i <= index;i++) {
        sum += i;
        item.push(i);
        while(sum > target){
            sum -= item.shift();
        }
        if(sum === target){
            result.push(Array.from(item));
        }
    }
    return result;
};
```

