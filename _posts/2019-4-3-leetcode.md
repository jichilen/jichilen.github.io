---

layout: post
title:  "leetcode"
date:   2019-4-3
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array,binary search,hash table,backtracking,string]
icon: icon-html
---

problem 39

### 39. Combination Sum

Medium

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

```python
class Solution:
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        
        rests = [[] for _ in range(target)]
        
        for num in candidates:
            if num > target:
                continue
            else:
                rests[target - num].append([num])
            for cur_rest in range(target - 1, num - 1, -1):
                for result in rests[cur_rest]:
                    rests[cur_rest - num].append(result + [num])
        
        return rests[0]
        
```

