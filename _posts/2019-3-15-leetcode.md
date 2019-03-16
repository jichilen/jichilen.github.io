---
layout: post
title:  "leetcode"
date:   2019-3-15
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

### 24. Swap Nodes in Pairs

Medium

Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

 

**Example:**

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

问题是两两相逆之后怎么连起来

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        p1=head
        if p1==None or p1.next==None:return p1 
        head=p1.next
        nn=ListNode(0)
        while p1!=None and p1.next!=None:
            nn.next=p1.next
            tmp=p1.next.next
            p1.next.next=p1
            p1.next=tmp
            nn=nn.next.next
            p1=nn.next
        return head
```



### 25. Reverse Nodes in k-Group

Hard

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.



**Example:**

Given this linked list: `1->2->3->4->5`

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.

对于链表来说，要想提高算法效率，就需要多指针配合

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        nn=ListNode(0)
        nn.next=head
        p1=p2=head
        def reverse(p1,p2,nn,n):
            # print(p1.val,p2.val,nn.val)
            if n<=1:
                return
            nn.next=p2
            p2.next=p1
            for i in range(n-2):
                p1=p1.next
            # nn=p2
            # p1,p2=p2.next,p1
            reverse(p2.next,p1,p2,n-1)
        head=nn
        while p2:
            for i in range(k-1):
                if p2.next:
                    p2=p2.next
                else:
                    return head.next
            tmp=p2.next
            reverse(p1,p2,nn,k)
            p1.next=tmp
            nn=p1
            p1=p2=tmp
        return head.next
```

这里是利用递归来进行翻转，其实也可以不用递归，每次用一个指针存放第一个指针，用第二个指针指向这个指针