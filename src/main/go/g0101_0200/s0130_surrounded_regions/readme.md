[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 130\. Surrounded Regions

Medium

Given an `m x n` matrix `board` containing `'X'` and `'O'`, _capture all regions that are 4-directionally surrounded by_ `'X'`.

A region is **captured** by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

**Input:** board = \[\["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:** Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically. 

**Example 2:**

**Input:** board = \[\["X"]]

**Output:** [["X"]] 

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 200`
*   `board[i][j]` is `'X'` or `'O'`.

## Solution

```golang
func solve(board [][]byte) {
	if len(board) == 0 {
		return
	}
	for i := 0; i < len(board[0]); i++ {
		if board[0][i] == 'O' {
			dfs(board, 0, i)
		}
		if board[len(board)-1][i] == 'O' {
			dfs(board, len(board)-1, i)
		}
	}
	for i := 0; i < len(board); i++ {
		if board[i][0] == 'O' {
			dfs(board, i, 0)
		}
		if board[i][len(board[0])-1] == 'O' {
			dfs(board, i, len(board[0])-1)
		}
	}
	for i := 0; i < len(board); i++ {
		for j := 0; j < len(board[0]); j++ {
			if board[i][j] == 'O' {
				board[i][j] = 'X'
			}
			if board[i][j] == '#' {
				board[i][j] = 'O'
			}
		}
	}
}

func dfs(board [][]byte, row, column int) {
	if row < 0 || row >= len(board) || column < 0 || column >= len(board[0]) || board[row][column] != 'O' {
		return
	}
	board[row][column] = '#'
	dfs(board, row+1, column)
	dfs(board, row-1, column)
	dfs(board, row, column+1)
	dfs(board, row, column-1)
}
```