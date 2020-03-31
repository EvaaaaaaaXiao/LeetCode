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




## 921、排序数组

[题目地址](https://leetcode-cn.com/problems/sort-an-array/)

### 题目描述
给你一个整数数组 `nums`，请你将该数组升序排列。

示例1:

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

示例2:

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

**提示**: 
1. `1 <= nums.length <= 50000`
2. `-50000 <= nums[i] <= 50000`


### 题解

#### 解法（ms)

冒泡

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
    for (let i = nums.length - 1; i >= 0 ; i--) {
        for (let j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                [nums[j], nums[j + 1]] = [nums[j + 1], nums[j]]
            }
        }
    }
    return nums;
};
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