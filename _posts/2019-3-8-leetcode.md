---
layout: post
title:  "leetcode"
date:   2019-3-8
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

problem 11,12,13,14

### 11. Container With Most Water

Medium

Given *n* non-negative integers *a1*, *a2*, ..., *an* , where each represents a point at coordinate (*i*, *ai*). *n*vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and *n* is at least 2.

 

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Example:**

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

这个题是个数学问题，最简单的思路就是暴力破解法，运算复杂度为$O(n ^2)$

更简单的可以用两个指针来解决

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        maxA=0
        i=0;j=len(height)-1
        while(j>i):
            maxA=max(maxA,min(height[i],height[j])*(j-i))
            if height[i]<height[j]:
                i+=1
            else:
                j-=1
        return maxA
```

### 12. Integer to Roman

Medium

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: 3
Output: "III"
```

**Example 2:**

```
Input: 4
Output: "IV"
```

**Example 3:**

```
Input: 9
Output: "IX"
```

**Example 4:**

```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 5:**

```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

解决这个问题要做分解，有几个比较重要的界限

1000,900,500,400,100,90,50,40,10,9,5,4

'M','CM','D','CD','C','XC','L','XL','X','IX','V','IV'

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        a=[1000,900,500,400,100,90,50,40,10,9,5,4,1]
        b=['M','CM','D','CD','C','XC','L','XL','X','IX','V','IV','I']
        i=0
        out=''
        while(num>0):
            if num>=a[i]:
                out+=b[i]
                num-=a[i]
            else:
                i+=1
        return out
        
```

### 13. Roman to Integer

Easy

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: "III"
Output: 3
```

**Example 2:**

```
Input: "IV"
Output: 4
```

**Example 3:**

```
Input: "IX"
Output: 9
```

**Example 4:**

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5:**

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

这一题是上一个的反过程，个人觉得比上一个难，不过也是一个查字典的过程

主要的难点在于一个小字符出现在大字符之前

```python
class Solution:
    def romanToInt(self, s: 'str') -> 'int':
        dic = {'I':1,'V':5,'X':10,'L':50,'C':100,'D':500,'M':1000}
        num = 0
        i = 0
        while(i<len(s)):
            if i+1<len(s) and dic[s[i+1]]>dic[s[i]]:
                num += dic[s[i+1]]-dic[s[i]]
                i += 2
            else:
                num += dic[s[i]]
                i += 1
            #print(num)
        return num
```

### 14. Longest Common Prefix

Easy

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

下面这个做法是很蠢的做法，可读性很差，设置了两个flag，一个负责判断是否全匹配，另一个负责判断是否匹配到最后一位

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if len(strs)==0:return ""
        for s in strs:
            if len(s)==0:
                return ''
        i=0
        fl=0;fl1=0
        while True:
            cs=strs[0][i]
            for s in strs:
                if s[i]!=cs:
                    fl=1
                    break
                if i==len(s)-1:
                    fl1=1
            if fl==1:
                break
            i+=1
            if fl1==1:
                break
        return strs[0][:i]
```

下面这个想法是利用排序解决问题，如果有共有pre，那么对pre进行排序结果不变，所以没有公共pre的必然是最小值或者最大值中的一个

```python
class Solution:
    def longestCommonPrefix(self, m):
        if not m: return ''
        s1 = min(m)
        s2 = max(m)

        for i, c in enumerate(s1):
            if c != s2[i]:
                return s1[:i]
        return s1
```

这样还不用疲于判断是否有空字符存在在list中