```
layout: post
title:  "leetcode"
date:   2019-5-18
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array，dynamic programing]
icon: icon-html
```

50. Pow(x, n)

Medium

Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (x^n).

**Example 1:**

```
Input: 2.00000, 10
Output: 1024.00000
```

**Example 2:**

```
Input: 2.10000, 3
Output: 9.26100
```

**Example 3:**

Input: 2.00000, -2
Output: 0.25000
Explanation:$2^{-2}=1/2^2=1/4=0.25$

**Note:**

- -100.0 < *x* < 100.0
- *n* is a 32-bit signed integer, within the range [−2^31, 2^31 − 1]

这是一个二分法问题，主要思路是计算 x 的(1,2,4,8...)次方

然后n次方可以看成是一个二进制转换问题

比如10次方是[0,1,0,1]对应(2,8)

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n<0:
            f=-1
            n=-n
        else: f=1
        out=[x]
        res=[]
        while n>1:
            out.append(out[-1]*out[-1])
            res.append(n%2)
            n=n//2
        res.append(n)
        outnum=1
        for i,re in enumerate(res):
            if re>0:
                outnum*=out[i]
        if f>0:
            return outnum
        else:
            return 1/outnum
```



