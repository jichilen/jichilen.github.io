---
layout: post
title:  "leetcode"
date:   2019-3-7
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

### 8. String to Integer (atoi)

Medium

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

- Only the space character `' '` is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

**Example 1:**

```
Input: "42"
Output: 42
```

**Example 2:**

```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**

```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**

```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−2^31) is returned.
```

这个题目无法吐槽

```python
class Solution:
    def valid(self,cs):
        out=0
        for c in cs:
            if c>='0' and c<='9':
                out=out*10+int(c)
            else:
                break
        return out
        
    def myAtoi(self, str: str) -> int:
        INT_MAX=pow(2,31)-1
        INT_MIN=-pow(2,31)
        str=str.strip()
        if len(str)==0:
            return 0
        out=0
        pre=1
        if str[0]=='-':
            pre=-1
            out=self.valid(str[1:])
        elif str[0]=='+':
            pre=1
            out=self.valid(str[1:])
        else:
            out=self.valid(str)
        out*=pre
        if out>INT_MAX: return INT_MAX
        if out<INT_MIN: return INT_MIN 
        return out
```

### 9. Palindrome Number

Easy

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

和字符回文一模一样

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x<0:return False
        y=x
        out=0
        while(y>0):
            out=out*10+y%10
            y=y//10
        if out==x:
            return True
        return False
```

### 10. Regular Expression Matching

Hard

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'`and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

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
p = "a*"
Output: true
Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

**Example 5:**

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

这个题要求做简单的正则匹配

正常的思路是一个递归问题，不过这个递归会有四种情况，有一种情况会出现两个分支

于是就写出了我自己的弟弟代码

```python
class Solution:
    def findMatch(self,s,p,b1,b2):
        if b2>=len(p) and b1>=len(s):return True
        elif b1>=len(s):
            for i in range(b2,len(p),2):
                if i+1>=len(p) or p[i+1]!='*': return False
            return True
        elif b2>=len(p):return False
        if p[b2]=='*':return False
        
        if b2+2<=len(p) and p[b2+1]=='*':
            if s[b1]==p[b2] or p[b2]=='.':
                return self.findMatch(s,p,b1+1,b2) or self.findMatch(s,p,b1,b2+2)
            else:
                return self.findMatch(s,p,b1,b2+2)
        else:
            if s[b1]==p[b2] or p[b2]=='.':
                return self.findMatch(s,p,b1+1,b2+1)
            else:
                return False
            
    def isMatch(self, s: str, p: str) -> bool:
        return self.findMatch(s,p,0,0)
```

虽然看起来还行，但是在递归里面都是垫底的性能

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        lenp = len(p)
        if not (s or p):
            return True
        elif s and (not p):
            return False
        elif (not s) and p:
            if lenp % 2 == 0 and p[1::2] == (lenp//2) * "*":
                return True
            else:
                return False
        if p[-1] == s[-1] or p[-1] == ".":
            return Solution.isMatch(None, s[:-1], p[:-1])
        elif p[-1] == "*":
            if p[-2] == s[-1] or p[-2] == ".":
                return Solution.isMatch(None, s[:-1], p) or Solution.isMatch(None, s, p[:-2])
            else:
                return Solution.isMatch(None, s, p[:-2])
        else:
            return False
```

优化之后的递归，第一个是长度减少了，索引起来快很多，第二个充分考虑到了*不会单独出现，所以倒向匹配。

```python
import unittest


class Solution(object):
    def isMatch(self, s, p):
        # The DP table and the string s and p use the same indexes i and j, but
        # table[i][j] means the match status between p[:i] and s[:j], i.e.
        # table[0][0] means the match status of two empty strings, and
        # table[1][1] means the match status of p[0] and s[0]. Therefore, when
        # refering to the i-th and the j-th characters of p and s for updating
        # table[i][j], we use p[i - 1] and s[j - 1].

        # Initialize the table with False. The first row is satisfied.
        table = [[False] * (len(s) + 1) for _ in range(len(p) + 1)]

        # Update the corner case of matching two empty strings.
        table[0][0] = True

        # Update the corner case of when s is an empty string but p is not.
        # Since each '*' can eliminate the charter before it, the table is
        # vertically updated by the one before previous. [test_symbol_0]
        for i in range(2, len(p) + 1):
            table[i][0] = table[i - 2][0] and p[i - 1] == '*'

        for i in range(1, len(p) + 1):
            for j in range(1, len(s) + 1):
                if p[i - 1] != "*":
                    # Update the table by referring the diagonal element.
                    table[i][j] = table[i - 1][j - 1] and \
                                  (p[i - 1] == s[j - 1] or p[i - 1] == '.')
                else:
                    # Eliminations (referring to the vertical element)
                    # Either refer to the one before previous or the previous.
                    # I.e. * eliminate the previous or count the previous.
                    # [test_symbol_1]
                    table[i][j] = table[i - 2][j] or table[i - 1][j]

                    # Propagations (referring to the horizontal element)
                    # If p's previous one is equal to the current s, with
                    # helps of *, the status can be propagated from the left.
                    # [test_symbol_2]
                    if p[i - 2] == s[j - 1] or p[i - 2] == '.':
                        table[i][j] |= table[i][j - 1]

        return table[-1][-1]
```

不使用递归，使用动态规划，这要求更多的内存，来存储所有的状态