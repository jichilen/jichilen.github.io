---
layout: post
title:  "leetcode"
date:   2019-4-13
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array，dynamic programing]
icon: icon-html
---

problem 42

### 42. Trapping Rain Water

Hard

Given *n* non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

**Example:**

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

这个题主要考虑几种情况，

第一种是最简单的，能找到边界，如 [1,0,2] [ 2,1,0,1,3]，这种情况要求起始指针能找到一个大于等于他的指针，这是优先考虑的情况。

如果找不到上面的情况依旧能够装水，只需要有一个gap就能装水，如[3,2,1,2]，当然这个用上面的思路也能解决，但是也有解决不了的，如[3,1,2]，这种情况需要记录两个指针，

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        i=0
        out=0
        while i <len(height):
            gap=0
            flag=0
            tmax=0
            mind=0
            tgap=0
            for j in range(i+1,len(height)):
                if height[j]<height[i]:
                    gap+=height[i]-height[j]
                    if height[j]>tmax:
                        tmax=height[j]
                        mind=j
                        tgap=gap
                else:
                    flag=1
                    break
            if flag==1:
                i=j
                out+=gap
            elif mind>i:
                out=out+tgap-(height[i]-height[mind])*(mind-i)
                i=mind
            else:
                i+=1
        return out
```

### 43. Multiply Strings

Medium

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Note:**

1. The length of both `num1` and `num2` is < 110.
2. Both `num1` and `num2` contain only digits `0-9`.
3. Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer**directly.

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        out=0
        for i in range(len(num2)):
            p1=ord(num2[-1-i])-ord('0')
            frac1=pow(10,i)
            addn=0
            for j in range(len(num1)):
                p2=ord(num1[-1-j])-ord('0')
                frac2=pow(10,j)
                addn+=p1*p2*frac2
            out+=addn*frac1
        return str(out)
```



### 44. Wildcard Matching

Hard

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

**Example 1:**

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```

**Example 3:**

```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**

```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**

```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

动态规划

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        m=len(s)
        n=len(p)
        dp = [[bool(0) for i in range(n+1)] for j in range(m+1)]
        dp[0][0] = bool(1)
        # 开始初始化填充,如果匹配的串s是空的的话，只有模式是*才能匹配
        for i in range(n):
            if dp[0][i] and p[i] == '*':
                dp[0][i + 1] = bool(1)

        ## 开始动态规划
        for i in range(m):
            for j in range(n):
                if p[j] == '*':
                    dp[i + 1][j + 1] = dp[i][j+1] or dp[i+1][j]
                elif p[j] == '?' or s[i] == p[j]:
                    dp[i+1][j + 1] = dp[i][j]
        return dp[m][n]
```

