---
layout: post
title:  "leetcode"
date:   2019-3-11
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

### 16. 3Sum Closest

Medium

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

```
class Solution:
    def threeSumClosest(self, nums: 'List[int]', target: 'int') -> 'int':
        nums.sort()
        n, diff = len(nums), float("inf")
        for i in range(n):
            l, r, tar = i+1, n-1, target-nums[i]
            while l < r:
                s = nums[l]+nums[r]
                if s == tar:
                    return target
                if abs(tar-s) < abs(diff):
                    diff = tar - s
                if s <= tar:
                    l += 1
                else:
                    r -= 1
        return target-diff
```

