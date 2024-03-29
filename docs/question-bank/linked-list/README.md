## 19、删除链表的倒数第 N 个结点

[题目地址](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### 题目描述

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。 

**进阶：**你能尝试使用一趟扫描实现吗？

示例1:

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

示例2:

```
输入：head = [1], n = 1
输出：[]
```

示例3:

```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

### 题解

#### 解法（68ms)

用**双指针**的思想

1、先设置一个节点 `dummyHead` 用于指向 `head`

2、设定 `fastNode` 和 `slowNode` ，初始都指向虚拟节点 `dummyHead` 

3、移动 `fastNode` ，直到 `slowNode` 与 `fastNode` 之间相隔的元素个数为 n

4、同时移动 `slowNode` 与 `fastNode` ，直到 `fastNode` 指向的为 `null`，即指向了末尾

5、将 `slowNode` 的下一个节点指向下下个节点，即删除了目标节点



```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    var dummyHead = new ListNode();
    dummyHead.next = head;
    var fastNode = dummyHead;
    var slowNode = dummyHead;
    for(var i = 0; i < n; i++){
        fastNode = fastNode.next;
    }
    while(fastNode.next != null){
        slowNode = slowNode.next;
        fastNode = fastNode.next;
    }
    slowNode.next = slowNode.next.next;
    return dummyHead.next
};
```



## 21、合并两个有序链表

[题目地址](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

### 题目描述
将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例:

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

### 题解

#### 解法（76ms)

用**递归**的思想，改变两个有序链表的指向，将已排好的链表指向还未排序的l1 , l2中值更小的那个，最终当某一条链表的next的值为null，意味着本条链表已排完序了，直接指向另一条还未排完的有序链表就可以了

以示例为例：
l1 : 1->2->4->5
l2 : 1->3->4

1、由于1 == 1，所以不改变还是指向 l1，并用 l1.next 与 l2 比较
2、由于1 < 2 ，所以 l1.next 指向 l2，1->1，继续用 l1 与 l2.next 比较
3、由于2 < 3 ，所以 l2.next 指向 l1，1->1->2，继续用 l1.next 与 l2 比较
4、由于3 < 4 ，所以 l1.next 指向 l2，1->1->2->3，继续用 l2.next 与 l1 比较
5、由于4 == 4 ，所以不改变还是指向 l2，1->1->2->3->4，继续用 l2.next 与 l1 比较
6、由于 l2.next == null，也就是当前的 l2 为null，所以可以结束比较了，直接指向还没有结束的 l1 ，1->1->2->3->4->4->5

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    if(l1 == null) return l2
    if(l2 == null) return l1
    if(l1.val > l2.val){
        l2.next = mergeTwoLists(l1, l2.next)
        return l2
    }else {
        l1.next = mergeTwoLists(l1.next, l2)
        return l1
    }
};
```




## 83、删除排序链表中的重复元素

[题目地址](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

### 题目描述
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

示例1:

```
输入: 1->1->2
输出: 1->2
```

示例2:

```
输入: 1->1->2->3->3
输出: 1->2->3
```

### 题解

#### 解法（76ms)

对比当前元素与下一个元素的值，相同则当前元素指向改变，指向下一个的下一个，不同则当前元素指向下一个不变。

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var deleteDuplicates = function(head) {
    var current = head;
    while(current && current.next){
        if(current.val == current.next.val){
            current.next = current.next.next
        }else{
            current = current.next
        }
    }
    return head
};
```




## 141、环形链表

[题目地址](https://leetcode-cn.com/problems/linked-list-cycle/)

### 题目描述
给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数` pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是` -1`，则在该链表中没有环。

示例1:

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

示例2:

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

**进阶：**

你能用 O(1)（即，常量）内存解决此问题吗？

### 题解

#### 解法（80ms)

用**双指针**的思想，一个指向`.next`，一个指向`.next.next`。若没有环，则两个指针并不会相遇，若有环，两个指针必会相遇

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    if(!head||!head.next) return false
    var pointer = head.next.next;
    while(head && pointer && pointer.next){
        if(pointer != head){
            head = head.next
            pointer = pointer.next.next
        }else{
            return true
        }
    }
    return false
};
```




## 206、反转链表

[题目地址](https://leetcode-cn.com/problems/reverse-linked-list/)

### 题目描述
反转一个单链表。

示例:

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶**:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

### 题解

#### 解法一（72ms)

**迭代**

- 初始化节点直接指向`null`
- 将当前的节点 ` cur ` 指向前一个节点 `prev`
- 将当前的节点 ` cur ` 和前一个节点 `prev` 都后移一位，得到新的 ` cur ` 和 `prev`，重复前一步骤
**注意**：这一步的后移需要提前将 ` cur.next `存放好，因为` cur`要指向`prev` ，会修改 ` cur.next `的内容
- 当 ` cur ` 为 ` null` 时停止

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    let prev = null;
    let cur = head;
    while(cur != null){
        let next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next
    }
    return prev;
};
```

#### 解法二（120ms)

**递归**

从原链表的最后两个节点开始操作

- 递归到最后的节点时，直接返回节点本身
- 暂存` head.next `的内容，并用` head.next `进行函数递归
- 断开` head `原来的链接，将` head.next `指向` null `
- 将之前存放的` head.next `指向` head `

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    if(!head || !head.next){
        return head
    }else{
        let nextNode = head.next;
        reverseNode = reverseList(nextNode);
        head.next = null;
        nextNode.next = head;
        return reverseNode
    }
};
```




## 876、链表的中间结点

[题目地址](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

### 题目描述
给定一个带有头结点` head` 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

示例1:

```
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```

示例2:

```
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```

**提示**:
- 给定链表的结点数介于` 1` 和` 100` 之间。

### 题解

#### 解法（96ms)

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var middleNode = function(head) {
    var node = head;
    var result = head;
    while(node && node.next){
        node = node.next.next;
        result = result.next;
    }
    return result;
};
```