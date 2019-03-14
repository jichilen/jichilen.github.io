---
layout: post
title:  "leetcode"
date:   2019-3-13
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

### 21. Merge Two Sorted Lists

Easy

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

注意边界的判断，这个题目可以以一个链表为基础，这样就只需要$O(1)$的空间

也可以新建一个链表，这样略快一点，需要$O(m+n)$空间

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1==None:return l2
        if l2==None:return l1
        a=ListNode(0)
        a.next=l1
        l1=a
        while l1.next and l2:
            if l1.next.val<l2.val:
                l1=l1.next
            else:
                tmp=l2.next
                l2.next=l1.next
                l1.next=l2
                l2=tmp
        if l2:
            l1.next=l2
        return a.next
```

### 22. Generate Parentheses

Medium

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

这个题可以理解为一个条件二叉树，很明显可以用一个深度优先搜索来做，于是就会有一个递归的算法

```python
class Solution:
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        res = []  
        self.generate(n, n, "", res)  
        
        
        return res  

    def generate(self, left, right, str, res):  
        if left == 0 and right == 0:  
            res.append(str)  
            return  
        if left > 0:  
            self.generate(left - 1, right, str + '(', res)
           
        if right > left:  
            self.generate(left, right - 1, str + ')', res)
```

也可以采用广度优先搜索来做，不过带条件的搜索最好用两个队列或者字典来实现

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        dic={}
        dic[1]=['(']
        for i in range(2*n-1):
            dect={}
            for ke in dic:
                if ke <n:
                    for s in dic[ke]:
                        if ke+1 in dect:
                            dect[ke+1].append(s+'(')
                        else:
                            dect[ke+1]=[s+'(']
                if 2*ke>i+1:
                    for s in dic[ke]:
                        if ke in dect:
                            dect[ke].append(s+')')
                        else:
                            dect[ke]=[s+')']
            # print(dect)
            dic=dect
        return dic[n]                
                        
```

代码很简单，键值表达的意思是左括号的数量，通过循环的数量可以算出右括号的数量`i+1-ke​`，这样可以很轻松的遍历所有的情况

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