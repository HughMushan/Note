#链表相关算法
---
> 本篇主要总结与链表相关的算法，主要涉及到链表的基本操作，如删除、插入以及合并等等。此外还有链表反转、查找、环的判断等问题。

链表是线性表的一种。线性表是最基本、最简单、也是最常用的一种数据结构。线性表中数据与元素之间具有一一对应关系。线性表有两种存储方式，分别是顺序存储结构和链式存储结构。链式存储结构就是两个相邻的元素在内存中可能不是相邻的，每一个元素都有一个指针域，指针域一般是存储着到下一个元素的指针。这种存储方式的**优点**是插入和删除的时间复杂度为 O(1)，不会浪费太多内存，添加元素的时候才会申请内存，删除元素会释放内存。缺点是访问的时间复杂度最坏为 O(n)。

在介绍链表相关的算法之前先来看链表问题中常用到的两个技巧，分别是**Dummy Node**和**快慢指针**。

**Dummy Node** 就是在链表表头 head 前加一个节点指向 head。主要是为了针对单链表中没有前向指针的问题，表征链表的头节点不会在删除操作中丢失。此外还可以简化代码。

**快慢指针** 快慢是指指针向前移动的步长。可以通过快慢指针解决以下两个问题:

- 快速找出未知长度单链表的中间节点
	设置两个指针 `*fast`、`*slow` 都指向单链表的头节点，其中`*fast`的移动速度是`*slow`的2倍，当`*fast`指向末尾节点的时候，`slow`正好就在中间了。
    
- 判断单链表是否有环
	利用快慢指针的原理，同样设置两个指针 `*fast`、`*slow` 都指向单链表的头节点，其中 `*fast`的移动速度是`*slow`的2倍。如果 `*fast = NULL`，说明该单链表 以 `NULL`结尾，不是循环链表；如果 `*fast = *slow`，则快指针追上慢指针，说明该链表是循环链表。

##链表反转

- leetcode: [Reverse Linked List | LeetCode OJ](https://leetcode.com/problems/reverse-linked-list/)
- lintcode: [(35) Reverse Linked List](http://www.lintcode.com/en/problem/reverse-linked-list/)

```
Reverse a linked list.

Example
For linked list 1->2->3, the reversed linked list is 3->2->1

Challenge
Reverse it in-place and in one-pass
```
链表反转是链表问题当中比较简单又比较常问的题，应该要很快速的写出来。

```c++
ListNode* ReverseList(ListNode *head) {
    ListNode *prev = NULL;
    ListNode *curr = head;
    while(curr != NULL) {
       ListNode *temp = curr->next;
       curr->next = prev;
       prev = curr;
       curr = temp;
    }
    head = prev;
    return head;
}
```