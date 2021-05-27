## 994、腐烂的橘子

[题目地址](https://leetcode-cn.com/problems/rotting-oranges/)

### 题目描述
在给定的网格中，每个单元格可以有以下三个值之一：

- 值` 0 `代表空单元格；
- 值` 1 `代表新鲜橘子；
- 值` 2 `代表腐烂的橘子。
每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回` -1`。

示例1:

```
输入：[[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

示例2:

```
输入：[[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。
```

示例3:

```
输入：[[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

**提示**：

1. `1 <= grid.length <= 10`
2. `1 <= grid[0].length <= 10`
3. `grid[i][j]` 仅为` 0`、`1 `或` 2`


### 题解

#### 解法（ms)



```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
 var orangesRotting = function (grid) {
    var bad = [];
    var normal = 0;
    var result = 0;
    for (var i = 0;i < grid.length;i++) {
        for (var j = 0;j < grid[i].length;j++) {
            if (grid[i][j] === 2){
                bad.push([i, j]);
            }
            else if (grid[i][j] === 1){
                normal++;
            }
        }
    }
    while (bad.length && normal) {
        var addBad = [];
        while (bad.length) {
            var badOrange = bad.shift();
            var i = badOrange[0];
            var j = badOrange[1]
            if (i > 0 && grid[i - 1][j] == 1) {
                grid[i - 1][j] = 2;
                addBad.push([i - 1, j]);
                normal--;
            }
            if (j > 0 && grid[i][j - 1] == 1) {
                grid[i][j - 1] = 2;
                addBad.push([i, j - 1]);
                normal--;
            }
            if (i < grid.length - 1 && grid[i + 1][j] == 1) {
                grid[i + 1][j] = 2;
                addBad.push([i + 1, j]);
                normal--;
            }
            if (j < grid[0].length - 1 && grid[i][j + 1] == 1) {
                grid[i][j + 1] = 2;
                addBad.push([i, j + 1]);
                normal--;
            }
        }
        result++;
        bad = addBad;
    }
    if (normal != 0) return -1;
    return result;
};
```



## 1162、地图分析

[题目地址](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

### 题目描述
你现在手里有一份大小为 N x N 的『地图』（网格） `grid`，上面的每个『区域』（单元格）都用` 0 `和` 1 `标记好了。其中` 0` 代表海洋，`1` 代表陆地，你知道距离陆地区域最远的海洋区域是是哪一个吗？请返回该海洋区域到离它最近的陆地区域的距离。

我们这里说的距离是『曼哈顿距离』（ Manhattan Distance）：`(x0, y0)` 和` (x1, y1)` 这两个区域之间的距离是` |x0 - x1| + |y0 - y1| `。

如果我们的地图上只有陆地或者海洋，请返回` -1`。

示例1:

```
输入：[[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释： 
海洋区域 (1, 1) 和所有陆地区域之间的距离都达到最大，最大距离为 2。
```

示例2:

```
输入：[[1,0,0],[0,0,0],[0,0,0]]
输出：4
解释： 
海洋区域 (2, 2) 和所有陆地区域之间的距离都达到最大，最大距离为 4。
```

**提示**：

1. `1 <= grid.length == grid[0].length <= 100`
2. `grid[i][j]` 不是` 0 `就是 `1`


### 题解

#### 解法（ms)



```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxDistance = function(grid) {
    let [ queue_1, maxDistance] = [ [], -1]
    for(let i = 0; i < grid.length; i++){
        for(let j = 0; j < grid[0].length; j++){
            if(grid[i][j] == 1)
                queue_1.push([i, j])
        }        
    }
    if(queue_1.length == (0 || (grid.length*grid[0].length))) return -1
    while(queue_1.length != 0){
        let len = queue_1.length;
        for(let k = 0; k < len; k++){
            let [i1, j1] = queue_1.shift();
            if(i1 > 0 && grid[i1-1][j1] == 0){
                grid[i1-1][j1] = 1;
                queue_1.push([i1-1,j1]);
            }
            if(i1 < grid.length - 1 && grid[i1+1][j1] == 0){
                grid[i1+1][j1] = 1;
                queue_1.push([i1+1,j1]);
            }
            if(j1 > 0 && grid[i1][j1-1] == 0){
                grid[i1][j1-1] = 1;
                queue_1.push([i1,j1-1]);
            }
            if(j1 < grid[0].length - 1 && grid[i1][j1+1] == 0){
                grid[i1][j1+1] = 1;
                queue_1.push([i1,j1+1]);
            }
        }
        maxDistance++;
    }
    return maxDistance;
};
```