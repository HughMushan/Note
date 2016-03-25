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
        
链表问题常见会犯的错误主要是**遍历链表不向前递推节点**，**遍历链表前未保存头节点**，**返回链表节点指针错误**，**不判断当前节点是否为空时的异常**

##链表反转
---

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
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        ListNode *prev = NULL;
        ListNode *curr = head;
        while (curr != NULL) {
            ListNode *temp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = temp;
        }
        head = prev;
        return head;
    }
};
```
链表反转还有升级版，就是区域链表反转

- leetcode: [Reverse Linked List II | LeetCode OJ](https://leetcode.com/problems/reverse-linked-list-ii/)
- lintcode: [(36) Reverse Linked List II](http://www.lintcode.com/en/problem/reverse-linked-list-ii/)

```
Reverse a linked list from position m to n.

Example
Given 1->2->3->4->5->NULL, m = 2 and n = 4, return
1->4->3->2->5->NULL.
Given m, n satisfy the following condition: 1 ≤ m ≤ n ≤ length of list.

Challenge
Reverse it in-place and in one-pass
```
思路也很简单，先找到m节点，同时也要记录m节点的前一节点`prem`，然后将m到n节点的next指针指向前一个节点，记录n节点的后一节点`postn`，将prem的next指针指向n，postn的next指针指向m。

```c++
ListNode *reverseBetween(ListNode *head, int m, int n) {
    if(head == NULL || m > n) return NULL;
    ListNode *dummy = new ListNode(0);
    dummy->next = head;
    ListNode *curr = dummy;
    for(int i = 0; i < m-1; i ++) {
        if(curr == NULL) return NULL;
        else {
          curr = curr->next;
        }
    }
    ListNode *prem = curr, *mNode = curr->next;
    ListNode *nNode = mNode, *postn = mNode->next;
    for(int i = m, i < n; i ++) {
      if(postn == NULL) return NULL;
      ListNode *temp = postn->next;
      postn->next = nNode;
      nNode = postn;
      postn = temp;
    }
    
    prem->next = nNode;
    postn->next = mNode;
    
    return dummy->next;
    
}

```

##判断链表是否有环
---
- leetcode: [Linked List Cycle II | LeetCode OJ](https://leetcode.com/problems/linked-list-cycle-ii/)
- lintcode: [(103) Linked List Cycle II](http://www.lintcode.com/en/problem/linked-list-cycle-ii/)

```
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Example
Given -21->10->4->5, tail connects to node index 1，return node 10

Challenge
Follow up:
Can you solve it without using extra space?
```

