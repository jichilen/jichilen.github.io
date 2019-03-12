---
layout: post
title:  "leetcode"
date:   2019-3-11
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

### 16. 3Sum Closest

Medium

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

对于这种问题，要减少复杂度，排序是必须的

重要的是如何利用排序之后的结果 

一个简单的思路是暴力破解，这样的话排序就是没有必要的了，复杂度为$O(n^3)$



```python
class Solution:
    def threeSumClosest(self, nums: 'List[int]', target: 'int') -> 'int':
        nums.sort()
        n, diff = len(nums), float("inf")
        for i in range(n):
            l, r, tar = i+1, n-1, target-nums[i]
            while l < r:
                s = nums[l]+nums[r]
                if s == tar:
                    return target
                if abs(tar-s) < abs(diff):
                    diff = tar - s
                if s <= tar:
                    l += 1
                else:
                    r -= 1
        return target-diff
```

### 17. Letter Combinations of a Phone Number

Medium

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```



```python
class Solution:
    def letterCombinations(self, digits: 'str') -> 'List[str]':
        maps = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'}
        if len(digits) == 0: return []
        if len(digits) == 1: return list(maps[digits[0]])
        
        res = ['']
        for digit in digits:
            new_res = []
            for ch in maps[digit]:
                new_res.extend([ele + ch for ele in res])
            res = new_res
        return res
```

### 18. 4Sum

Medium

Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums`such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

字典查询，排序加速

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        dic={}
        if len(nums)<4:
            return []
        out=set()
        for i,x in enumerate(nums):
            # if x==nums[-1]:
                # break
            for j,y in enumerate(nums[i+1:],i+1):
                dic={}
                for k,z in enumerate(nums[j+1:],j+1):
                    if z in dic:
                        out.add((x,y,target-x-y-z,z))
                    else:
                        dic[target-x-y-z]=1
        return list(out)
                        
```

### 19. Remove Nth Node From End of List

Medium

Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

**Follow up:**

Could you do this in one pass?

快慢指针

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:

        p1=p2=head
        for i in range(n):
            p2=p2.next
        if p2==None:
            return head.next
        p2=p2.next
        while(p2!=None):
            p1=p1.next
            p2=p2.next
        p1.next=p1.next.next
        return head
```

### 20. Valid Parentheses

Easy

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

堆栈的运用

```python
class Solution:
    def isValid(self, st: str) -> bool:
        stack=[]
        lquad='([{'
        rquad=')]}'
        for s in st:
            if s in lquad:
                stack.append(s)
            else:
                if len(stack)==0:return False
                ind=lquad.find(stack[-1])
                if s==rquad[ind]:
                    stack.pop()
                else:
                    return False
        if len(stack)==0:
            return True
        return False
```

