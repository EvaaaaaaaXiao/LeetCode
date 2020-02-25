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