## 题库

#### 1、两数之和 

[题目地址](https://leetcode-cn.com/problems/two-sum/)

##### 题目描述
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



##### 题解

###### 解法一（192ms)

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



###### 解法二（56ms）

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



##### 思考

###### Map vs Object

- 当存储的是简单数据类型，并且 **key 都为字符串或者整数或者 Symbol** 的时候，优先使用 Object 。因为Object可以使用字符变量的方式创建，更加高效
- 当需要在单独的逻辑中访问属性或者元素的时候，应该使用 Object
- JSON 直接支持 Object，但不支持 Map
- Map 是纯粹的 hash， 而 Object 还存在一些其他内在逻辑，所以在执行 delete 的时候会有性能问题。所以**写入删除密集**的情况应该使用 Map。
- Map 会**按照插入顺序保持元素的顺序**，而Object做不到。
- Map 在**存储大量元素**的时候性能表现更好，特别是在代码执行时**不能确定 key 的类型**的情况。



###### 若题目数组有重复内容



## 理论知识

#### Map

是一组键值对的结构，具有极快的查找速度。其中**唯一键映射到值**。密钥和值都可以是**任何数据类型**。map是一个可迭代的数据结构，这意味着会**按照插入顺序保持元素的顺序**

##### 初始化

```javascript
var m = new Map(); // 空Map
var m = new Map([['Michael', 95], ['Tracy', 85]]); //二维数组
```

##### 方法

```javascript
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

##### 唯一键

```javascript
var m = new Map();
m.set('Adam', 67);
m.set('Adam', 88);
m.get('Adam'); // 88 后面的值会把前面的值冲掉
```

##### 使用情况

Map 在**存储大量元素**的时候性能表现更好，特别是在代码执行时**不能确定 key 的类型**的情况。还有**写入删除密集**的情况应该使用 Map。

