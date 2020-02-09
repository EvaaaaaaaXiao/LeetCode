## Map

是一组键值对的结构，具有极快的查找速度。其中**唯一键映射到值**。密钥和值都可以是**任何数据类型**。map是一个可迭代的数据结构，这意味着会**按照插入顺序保持元素的顺序**

### 初始化

```javascript
var m = new Map(); // 空Map
var m = new Map([['Michael', 95], ['Tracy', 85]]); //二维数组
```

### 方法

```javascript
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

### 唯一键

```javascript
var m = new Map();
m.set('Adam', 67);
m.set('Adam', 88);
m.get('Adam'); // 88 后面的值会把前面的值冲掉
```

### 使用情况

Map 在**存储大量元素**的时候性能表现更好，特别是在代码执行时**不能确定 key 的类型**的情况。还有**写入删除密集**的情况应该使用 Map。