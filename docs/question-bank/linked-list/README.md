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