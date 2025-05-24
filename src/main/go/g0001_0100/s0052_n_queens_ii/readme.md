[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 52\. N-Queens II

Hard

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _the number of distinct solutions to the **n-queens puzzle**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4

**Output:** 2

**Explanation:** There are two distinct solutions to the 4-queens puzzle as shown.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Constraints:**

*   `1 <= n <= 9`

## Solution

```golang
func totalNQueens(n int) int {
	row := make([]bool, n)
	col := make([]bool, n)
	diagonal := make([]bool, 2*n-1)
	antiDiagonal := make([]bool, 2*n-1)
	return totalNQueensHelper(n, 0, row, col, diagonal, antiDiagonal)
}

func totalNQueensHelper(
	n, r int,
	row, col, diagonal, antiDiagonal []bool,
) int {
	if r == n {
		return 1
	}
	count := 0
	for c := 0; c < n; c++ {
		if !row[r] && !col[c] && !diagonal[r+c] && !antiDiagonal[r-c+n-1] {
			row[r], col[c], diagonal[r+c], antiDiagonal[r-c+n-1] = true, true, true, true
			count += totalNQueensHelper(n, r+1, row, col, diagonal, antiDiagonal)
			row[r], col[c], diagonal[r+c], antiDiagonal[r-c+n-1] = false, false, false, false
		}
	}
	return count
}
```