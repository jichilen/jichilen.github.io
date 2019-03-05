---
layout: post
title:  "leetcode"
date:   2019-3-3
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

problem 4

#### 4. Median of Two Sorted Arrays

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be $O(log (m+n))$.

You may assume **nums1** and **nums2** cannot be both empty.

**Example 1:**

```python
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```python
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

答案要求复杂度为$O(log (m+n))$很容易可以想到我们需要用二分法来解决这个问题

其次问题是找中值，所以我们需要将数据切成两份

> nums1:   1 2 3 4 5 6 7 8 9   (9)
>
> ​            :                $\uparrow$ 
>
> nums2:   2 3 5 8 9                (5)
>
> ​            :      $\uparrow$  
>
> nums1:   1 2 3 4 5 6 7 8 9   (9)
>
> ​            :          $\uparrow$ 
>
> nums2:   2 3 5 8 9                (5)
>
> ​            :             $\uparrow$ 
>
> nums1:   1 2 3 4 5 6 7 8 9   (9)
>
> ​            :             $\uparrow$ 
>
> nums2:   2 3 5 8 9                (5)
>
> ​            :          $\uparrow$ 

大致的问题解决了，下面需要思考的问题是奇偶数的问题，下标越界的问题

> condition 1:
>
> nums1:   1 2 3 4 5 6 7 8 9   (9)
>
> ​            :                             $\uparrow$ 
>
> nums2:   2 3 5 8 9                (5)
>
>
>
> condition 2:
>
> nums1:   1 2 3 4 5 6 7 8   (8)
>
> ​            :             $\uparrow$ 
>
> nums2:   2 3 5 8 9             (5)
>
> ​            :          $\uparrow$ 
>
>

```python
class Solution:
    def findMedianSortedArrays(self,A, B):
        m, n = len(A), len(B)
        if m > n:
            A, B, m, n = B, A, n, m
        if n == 0:
            raise ValueError

        imin, imax, half_len = 0, m, int((m + n + 1) / 2)
        while imin <= imax:
            i = int((imin + imax) / 2)
            j = half_len - i
            if i < m and B[j-1] > A[i]:
                # i is too small, must increase it
                imin = i + 1
            elif i > 0 and A[i-1] > B[j]:
                # i is too big, must decrease it
                imax = i - 1
            else:
                # i is perfect

                if i == 0: max_of_left = B[j-1]
                elif j == 0: max_of_left = A[i-1]
                else: max_of_left = max(A[i-1], B[j-1])

                if (m + n) % 2 == 1:
                    return max_of_left

                if i == m: min_of_right = B[j]
                elif j == n: min_of_right = A[i]
                else: min_of_right = min(A[i], B[j])

                return (max_of_left + min_of_right) / 2.0
        
```

