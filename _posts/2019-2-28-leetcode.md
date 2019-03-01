---
layout: post
title:  "leetcode"
date:   2019-2-28
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

#### 240. Search a 2D Matrix II

Medium

Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example:**

Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = `5`, return `true`.

Given target = `20`, return `false`.

```python
#o(m+n)  o(1)
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if len(matrix)==0:
            return False
        rows,cols=len(matrix)-1,len(matrix[0])-1
        row=rows,col=0
        while row>=rows and col<=cols:
            if matrix[row][col]==target:
                return True
            elif matrix[row][col]>target:
                row-=1
            else:
                col+=1
        return False
```

#### 241. Different Ways to Add Parentheses

Medium

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.

**Example 1:**

```
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```

**Example 2:**

```
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

```python
class Solution:
    def cal_three(self,n1,n2,token):
        if token=='-':
            return int(n1)-int(n2)
        if token=='+':
            return int(n1)+int(n2)
        if token=='*':
            return int(n1)*int(n2)
        
    def diffWaysToCompute(self, input: str) -> List[int]:
        if len(input)==1:
            return int(input)
        if len(input)==3:
            return self.cal_three(int(input[0]),int(input[2]),input[1])
        numcal=int((len(input)-1)/2)
        out=[]
        for i in range(numcal):
            ind=2*i+1
            out1=self.diffWaysToCompute(input[:ind])
            out2=self.diffWaysToCompute(input[ind+1:])
            if not isinstance(out1,list):
                out1=[out1]
            if not isinstance(out2,list):
                out2=[out2]
            for o1 in out1:
                for o2 in out2:
                    out.append(self.cal_three(o1,o2,input[ind]))
        return out
```

