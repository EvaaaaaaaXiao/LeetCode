## 543、二叉树的直径

[题目地址](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

### 题目描述
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

示例:

给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5   
```
返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

**注意**：两结点之间的路径长度是以它们之间边的数目表示。

### 题解

#### 解法（64ms)



```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var diameterOfBinaryTree = function(root) {
    var result = 1;
    depth(root);
    return result - 1;
    
    function depth(rootNode) {
        if (!rootNode) return 0;
        var left = depth(rootNode.left);
        var right = depth(rootNode.right);
        result = Math.max(left + right + 1, result);
        return Math.max(left, right) + 1;
    }
};
```