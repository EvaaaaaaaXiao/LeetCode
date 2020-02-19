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