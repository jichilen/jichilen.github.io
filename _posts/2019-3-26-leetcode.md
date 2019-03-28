---
layout: post
title:  "leetcode"
date:   2019-3-26
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array,binary search,hash table,backtracking,string]
icon: icon-html
---

problem 33,34,35,36,37,38

### 33. Search in Rotated Sorted Array

Medium

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

从算法复杂度就能看出来是一个二分法

唯一的问题就在于，二分过程中只有一半肯定是顺序的

所以需要加入严谨的判断来解决这种情况

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if len(nums)==0:
            return -1
        def sort(nums,target,low,high):
            if low>high:
                return -1
            mid=int((low+high)/2)
            if nums[mid]==target:
                return mid
            if (nums[mid] < nums[high]) :
                if (nums[mid] < target and target <= nums[high]):
                    return sort(nums, target, mid + 1, high);
                else:
                    return sort(nums, target, low, mid - 1);
            else :
                if (nums[low] <= target and target < nums[mid]):
                    return sort(nums, target, low, mid - 1);
                else:
                    return sort(nums, target, mid + 1, high);
            
        return sort(nums,target,0,len(nums)-1)
```

### 34. Find First and Last Position of Element in Sorted Array

Medium

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```



```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def search(nums,target,fl):
            low=0
            high=len(nums)
            while low<high:
                mid=int((low+high)/2)
                if nums[mid]>target or (nums[mid]==target and fl):
                    high=mid
                else:
                    low=mid+1
            return low
        l=search(nums,target,1)
        r=search(nums,target,0)-1
        if(l>r):return [-1,-1]
        return [l,r]
```

这里要注意边界的判断，mid=int((low+high)/2)，所以mid会接近low bound，所以high=mid是可以的，但是low=mid是不行的，会进入死循环

### 35. Search Insert Position

Easy

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```



```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        if target>nums[-1]:
            return len(nums)
        for i,n in enumerate(nums):
            if n>=target:
                break
        
        return i
```

### 36. Valid Sudoku

Medium

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.

A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

**Example 1:**

```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

这个题在于判断是否有重复

```python
from collections import defaultdict
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        def valid_9(ls):
            val=defaultdict(int)
            for l in ls:
                if l != '.':
                    if val[l]==1:
                        return False
                    val[l]+=1
            return True
        out=True
        for i in range(9):
            out&=valid_9(board[i])
            if not out:
                return out
            ls=[b[i] for b in board]
            out&=valid_9(ls)
            if not out:
                return out
            r=i//3*3
            c=i%3*3
            ls=[board[i][j] for i in range(r,r+3) for j in range(c,c+3)]
            out&=valid_9(ls)
            if not out:
                return out
        return out
```





### 37. Sudoku Solver

Hard

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

Empty cells are indicated by the character `'.'`.

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        def valid_9(ls):
            val=defaultdict(int)
            for l in ls:
                if l != '.':
                    if val[l]==1:
                        return False
                    val[l]+=1
            return True
        def find_9(board):
            row=[[False for i in range(9)] for j in range(9)]
            col=[[False for i in range(9)] for j in range(9)]
            qub=[[False for i in range(9)] for j in range(9)]
            for i in range(9):
                for j in range(9):
                    if board[i][j]!='.':
                        num=ord(board[i][j])-ord('1')
                        row[i][num]=True
                        col[j][num]=True
                        qub[i//3*3+j//3][num]=True                        
            return row,col,qub
        def dfs(i,j):
            while board[i][j]!='.':
                j+=1
                if j>8:
                    j=0
                    i+=1
                if i>8:
                    return True
            for nu in range(9):
                if row[i][nu] or col[j][nu] or qub[i//3*3+j//3][nu]:
                    # print(nu)
                    continue
                row[i][nu]=True
                col[j][nu]=True
                qub[i//3*3+j//3][nu]=True
                board[i][j]=chr(ord('1')+nu)
                # print(i,j,nu)
                if dfs(i,j):
                    return True
                else:
                    row[i][nu]=False
                    col[j][nu]=False
                    qub[i//3*3+j//3][nu]=False
                    board[i][j]='.'
            return False
        row,col,qub=find_9(board)
        dfs(0,0)
```





### 38. Count and Say

Easy

The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2`, then `one 1"` or `1211`.

Given an integer *n* where 1 ≤ *n* ≤ 30, generate the *n*th term of the count-and-say sequence.

Note: Each term of the sequence of integers will be represented as a string.

 

**Example 1:**

```
Input: 1
Output: "1"
```

**Example 2:**

```
Input: 4
Output: "1211"
```



```python
class Solution:
    rm=["1"]
    def countAndSay(self, n: int) -> str:
        if n==0:return ""
        out="1"
        if n<len(Solution.rm):
            return Solution.rm[n-1]
        for i in range(2,n+1):
            s=out+"0"
            lt=s[0]
            out=""
            cal=0
            for t in s:
                if lt!=t:
                    out=out+str(cal)+lt
                    lt=t
                    cal=1
                else:
                    cal+=1
            if i>=len(Solution.rm):
                Solution.rm.append(out)
        return out
```

