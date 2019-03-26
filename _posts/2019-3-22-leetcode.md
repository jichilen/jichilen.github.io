---
layout: post
title:  "leetcode"
date:   2019-3-22
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array]
icon: icon-html
---

problem 31,32

### 31. Next Permutation

Medium

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **in-place** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1
```

这个题的关键在于判断数组尾部的逆序

> 1 3 5 4 4 2 1
>
> 调换次大数字
>
> 1 4 5 4 3 2 1
>
> 结尾倒序排列
>
> 1 4 1 2 3 4 5
>
> 边界问题：
>
> 如果没有找到 如 5 4 3 2 1 和4 5 3 2 1
>
> 如果次大字符没有找到 1 5 4 3
>
> 所以这里最好用while循环

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if len(nums)<=1:return
        i=len(nums)-1
        while i>=1:
            if nums[i]>nums[i-1]:
                break
            i-=1
        if i==0:
            nums[:]=nums[::-1]
            return
        i-=1
        j=i+1
        while j<len(nums):
            if nums[j]<=nums[i]:
                break
            j+=1
        j-=1
        nums[i],nums[j]=nums[j],nums[i]
        nums[i+1:]=nums[i+1:][::-1]
```

### 32. Longest Valid Parentheses

Hard

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

**Example 2:**

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

有效括号的解决思路是使用栈

要求最长有效括号，需要考虑中间所有的配对情况

```python
class Solution:
    def longestValidParentheses(self, s):
        """
        :type s: str
        :rtype: int
        """
        stack = []  
        start = 0   
        res = 0    
        for i in range(len(s)):
            if s[i] == "(":
                stack.append(i)
            else:
                if not stack:
                    res = max(res, i-start)
                    start = i+1
                    continue
                else:
                    stack.pop()
                    if not stack:
                        res = max(res, i-start+1)
                    if stack:
                        res = max(res, i-stack[-1])
        return res
```

