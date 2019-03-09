---
layout: post
title:  "leetcode"
date:   2019-3-9
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

### 15. 3Sum

Medium

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

这个题可以说是在2sum上面的扩展。

对于twosum，我们一次遍历通过查找表来解决，同理这次3sum也是一样

不过这里面有很多tricks没有注意到会影响很多的速度

排序在这里很有必要，能减少很多不必要的计算量，首先排序之后，如果能在表里找到自己说明自己不用入表

那么自己后面的数也不用入表，因为后面的数比他大。

`if i >= 1 and v == nums[i-1]:`这句话能提高很多速度，这是因为大量重复的数据会出现，如果最后一个数出现多次，就会很影响速度

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
        nums.sort()
        res = set()
        for i, v in enumerate(nums):
            if i >= 1 and v == nums[i-1]:
                continue
            d = {}
            flag=0
            for x in nums[i+1:]:
                if -x in d:
                    flag=1
                    res.add((v, -v-x, x))
                elif flag==0 :
                    d[v+x] = 1
                    
        return list(res)
```

