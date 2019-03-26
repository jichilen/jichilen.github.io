---
layout: post
title:  "leetcode"
date:   2019-3-26
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array]
icon: icon-html
---

problem 33

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

