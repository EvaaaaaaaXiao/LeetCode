## Map

是一组键值对的结构，具有极快的查找速度。其中**唯一键映射到值**。密钥和值都可以是**任何数据类型**。map是一个可迭代的数据结构，这意味着会**按照插入顺序保持元素的顺序**



### 使用情况

Map 在**存储大量元素**的时候性能表现更好，特别是在代码执行时**不能确定 key 的类型**的情况。还有**写入删除密集**的情况应该使用 Map。



### Map与常规对象Object的区别

1. Map的key的类型无限制
- Object只能使用**字符串**值作为键名，但Map的键名可以是**任意类型**

2. Map可直接遍历
- 常规对象里，为了遍历keys、values和entries，你必须将它们转换为数组，如使用Object.keys()、Object.values()和Object.entries()，或使用for ... in，另外for ... in循环还有一些限制：它仅仅遍历可枚举属性、非Symbol属性，并且遍历的顺序是任意的。
- 但Map可直接遍历，且因为它是键值对集合，所以**可直接使用for…of或forEach来遍历**。这点不同的优点是为你的程序带来更高的执行效率。



### 初始化

```javascript
var m = new Map(); // 空Map
var m = new Map([['Michael', 95], ['Tracy', 85]]); //二维数组
```



### 常用方法

```javascript
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
m.clear(); // 清空指定map对象中的所有内容 
```



### Map遍历

首先掌握`keys( )`、`values( )` 和 `entries( )`

#### key( )

```javascript
var m = new Map([[1, 'red'], [2, 'blue']]);
console.log(m.keys());     // { 1, 2 }
```


#### values( )

```javascript
var m = new Map([[1, 'red'], [2, 'blue']]);
console.log(m.values());     // { "red", "blue" }
```
#### entries( )

```javascript
var m = new Map([[1, 'red'], [2, 'blue']]);
console.log(m.entries());     // {1 => "red", 2 => "blue"}
```
#### 遍历
```javascript
let myFavoriteBooks = new Map();

myFavoriteBooks.set(1, 'Rich Dad Poor Dad');
myFavoriteBooks.set(2, 'The Magic of Thinking Big');
myFavoriteBooks.set(3, 'Think and Grow Rich');
myFavoriteBooks.set(4, 'How to Win Friends & Influence People');
myFavoriteBooks.set(5, 'Shoe Dog');

/**
 * 1
 * 2
 * 3
 * 4
 * 5
 */
for (const key of myFavoriteBooks.keys()) {
    console.log(key);
}

/**
 * Rich Dad Poor Dad
 * The Magic of Thinking Big
 * Think and Grow Rich
 * How to Win Friends & Influence People
 * Shoe Dog
 */
for (const value of myFavoriteBooks.values()) {
    console.log(value);
}

/**
 * 1 "Rich Dad Poor Dad"
 * 2 "The Magic of Thinking Big"
 * 3 "Think and Grow Rich"
 * 4 "How to Win Friends & Influence People"
 * 5 "Shoe Dog"
 */
for (const [key, value] of myFavoriteBooks.entries()) {
    console.log(key, value);
}
```