---
layout: post
title:  "leetcode"
date:   2019-4-9
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array,back tracing]
icon: icon-html
---

### 40. Combination Sum II

Medium

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```



```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort(reverse=True)
        def dfs(bg,target):
            if target<0:
                return 0
            if target==0:
                return [[]]
            out=[]
            num=bg+1
            while num <len(candidates):
            # for i in range(bg+1,):
                re=dfs(num,target-candidates[num])
                if isinstance(re,list):
                    for r in re:
                        r.append(candidates[num])
                        out.append(r)
                q = num
                while q + 1 < len(candidates) and candidates[num] == candidates[q+1]:
                    q += 1
                if q>num:
                    num=q+1
                else:
                    num+=1
            return out
        out=dfs(-1,target)
        return out
        outt=set()
        for o in out:
            outt.add(tuple(o))
        return [list(o) for o in outt]
            
```

### 41. First Missing Positive

Hard

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

最直接的思路就是排序，然后滤掉负值，进行判断

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        if len(nums)<1:
            return 1
        nums.sort()
        for j in range(len(nums)):
            if nums[j]>0:
                break
        nums=nums[j:]
        if len(nums)<1:
            return 1
        if nums[0]>1:return 1
        for i in range(len(nums)-1):
            if nums[i+1]!=nums[i]+1 and nums[i+1]!=nums[i]:
                return max(nums[i]+1,1)
        if len(nums)==1:
            if nums[-1]>0: return nums[-1]+1
            return 1
        if nums[-1]==nums[-2]+1:
            return max(nums[-1]+1,1)
        return max(nums[-2]+1,1)
```

这种思路很直接，但是由于排序算法还要自己实现的话很麻烦。

另一种思路就更简单了，我们直接按照正整数排列

对于-1,2,4,1,3，第0位必须是1，以此类推，因此有效的第i项需要放在nums[i]-1项，那么交换之后的第i项同样需要换位置，直到他是一个无效值或者处于正确的位置

```python

```

或者用字典处理

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        dic = {}
        for i,item in enumerate(nums):
            if item not in dic:
                dic[item] = 1
             
        j = 1
        while True:
            if j not in dic:
                return j
            j+=1
```

