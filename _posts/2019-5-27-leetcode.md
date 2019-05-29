```
layout: post
title:  "leetcode"
date:   2019-5-27
desc: "leetcode problem"
keywords: "python"
categories: [PYTHON]
tags: [python,coding,leetcode,array，dynamic programing]
icon: icon-html
```

\51. N-Queens

Hard

The *n*-queens puzzle is the problem of placing *n* queens on an *n*×*n* chessboard such that no two queens attack each other.

Given an integer *n*, return all distinct solutions to the *n*-queens puzzle.

Each solution contains a distinct board configuration of the *n*-queens' placement, where `'Q'`and `'.'` both indicate a queen and an empty space respectively.

**Example:**

```
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```



```
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        def forbid_board(r,c,board):
            l = len(board)
            for i in range(l):
                board[r][i]=1
                board[i][c]=1
                diag1r=r-c+i
                diag2r=r+c-i
                if diag1r>=0 and diag1r<l:
                	board[diag1r][i]=1
                if diag2r>=0 and diag2r<l:
                	board[diag2r][i]=1
        board=[[0 for _ in range(n)] for _ in range(n)]
        board[0][0]=1
        print(board)
        forbid_board(2,3,board)
        print(board)
```

