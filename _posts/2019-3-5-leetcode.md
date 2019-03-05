---
layout: post
title:  "leetcode"
date:   2019-3-5
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

problem 5,6,7

###5. Longest Palindromic Substring

Medium

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```

要实现回文，最简单的思路就是从中间向两边走，

这个问题的难点在于如何定义中点来处理奇数和偶数回文问题

> "ababa"
>
> "cccababa"

```python
class Solution(object):
    def longestPalindrome(self, s):
        res = ""
        for i in xrange(len(s)):
            # odd case, like "aba"
            tmp = self.helper(s, i, i)
            if len(tmp) > len(res):
                res = tmp
            # even case, like "abba"
            tmp = self.helper(s, i, i+1)
            if len(tmp) > len(res):
                res = tmp
        return res

    # get the longest palindrome, l, r are the middle indexes   
    # from inner to outer
    def helper(self, s, l, r):
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1; r += 1
        return s[l+1:r]
        
```

###6. ZigZag Conversion

Medium

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

这个问题的思路在于分组处理，如何寻找分组的规律是重点

> N=3    4个一循环
>
> 4n+1 4n+3一组
>
> N=4    6个一循环
>
> 6n+1 6n+5一组  6n+2  6n+4
>
> N=5    8个一循环
>
> +1 +7 |+2 +6 |+3 +5

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        cur=2*numRows-2
        n=len(s)
        if n==0:
            return ""
        if numRows==1:
            return s
        out=""
        for i in range(numRows):#begin at 0
            frac=0
            while frac*cur+i<n:
                out+=s[frac*cur+i]
                if i!=0 and i!=numRows-1 and cur-i+frac*cur<n:
                    out+=s[cur-i+frac*cur]
                frac+=1
        return out
```

### 7. Reverse Integer

Easy

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

```python
In [6]: def inverse(num):
   ...:     if num==0:
   ...:         return 0
   ...:     if num>0:
   ...:         flag=1
   ...:     else:
   ...:         flag=-1
   ...:     num=num*flag
   ...:     out=0
   ...:     while(num>0):
   ...:         out=out*10+num%10
   ...:         num=int(num/10)
   ...:     return out*flag
```

