---
layout: post
title:  "leetcode"
date:   2019-3-19
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode ]
icon: icon-html
---

problem 30

### 30. Substring with Concatenation of All Words

Hard

You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring(s) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

**Example 1:**

```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**

```
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```

这个题关键的一点在于所有的单词长度一样，如果不一样，复杂度感觉爆炸

既然长度一样，就可以采用滑窗的思路来解题

假设单词的长度为N，一共n的单词，则一个窗口长度为n*N

然后及时如何滑窗的问题了

> 假设4个单词，长度为4，初始窗口如下
>
> \|----\|----\|----\|----\|
>
> 第一种滑窗的思路
>
> .\|----\|----\|----\|----\|
>
> ..\|----\|----\|----\|----\|
>
> 这种滑窗会导致重复计算
>
> 第二种思路
>
> 先每次滑动N，这样可以继承前面计算的结果

```python
from collections import Counter
from collections import defaultdict
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        if len(words)==0:return []
        slid_w=len(words)*len(words[0])
        i=0
        out=[]
        pred=Counter(words)
        while i+slid_w<=len(s):
            dic=defaultdict(int)
            s_=s[i:i+slid_w]
            for k in range(len(words)):
                p_w=s_[k*len(words[0]):(k+1)*len(words[0])]
                if p_w in words:
                    dic[p_w]+=1
                else:
                    break
            if dic==pred:
                out.append(i)
            i+=1
        return out

```

第二种滑窗

```python
from collections import Counter
from collections import defaultdict
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        if len(words)==0:return []
        n=len(words)
        N=len(words[0])
        out=[]
        pred=Counter(words)
        for i in range(N):
            dic=defaultdict(int)
            left=i
            cal=0
            for j in range(i,len(s),N):
                cal+=1
                pre=s[j:j+N]
                if pre in words:
                    dic[pre]+=1
                if cal>=n:
                    if dic==pred:
                        out.append(left)
                    if s[left:left+N] in dic:
                        dic[s[left:left+N]]-=1
                    # print(dic)
                    left+=N
        return out
```

1664 ms ->304 ms