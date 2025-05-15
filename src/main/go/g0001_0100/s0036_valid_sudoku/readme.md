[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 36\. Valid Sudoku

Medium

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1.  Each row must contain the digits `1-9` without repetition.
2.  Each column must contain the digits `1-9` without repetition.
3.  Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

*   A Sudoku board (partially filled) could be valid but is not necessarily solvable.
*   Only the filled cells need to be validated according to the mentioned rules.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:**

    board =
    [["5","3",".",".","7",".",".",".","."]
    ,["6",".",".","1","9","5",".",".","."]
    ,[".","9","8",".",".",".",".","6","."]
    ,["8",".",".",".","6",".",".",".","3"]
    ,["4",".",".","8",".","3",".",".","1"]
    ,["7",".",".",".","2",".",".",".","6"]
    ,[".","6",".",".",".",".","2","8","."]
    ,[".",".",".","4","1","9",".",".","5"]
    ,[".",".",".",".","8",".",".","7","9"]]

**Output:** true 

**Example 2:**

**Input:**

    board =
    [["8","3",".",".","7",".",".",".","."]
    ,["6",".",".","1","9","5",".",".","."]
    ,[".","9","8",".",".",".",".","6","."]
    ,["8",".",".",".","6",".",".",".","3"]
    ,["4",".",".","8",".","3",".",".","1"]
    ,["7",".",".",".","2",".",".",".","6"]
    ,[".","6",".",".",".",".","2","8","."]
    ,[".",".",".","4","1","9",".",".","5"]
    ,[".",".",".",".","8",".",".","7","9"]]

**Output:** false

**Explanation:** Same as Example 1, except with the **5** in the top left corner being modified to **8**. Since there are two 8's in the top left 3x3 sub-box, it is invalid. 

**Constraints:**

*   `board.length == 9`
*   `board[i].length == 9`
*   `board[i][j]` is a digit `1-9` or `'.'`.

## Solution

```golang
type Solution struct {
	j1 int
	i1 [9]int
	b1 [9]int
}

func isValidSudoku(board [][]byte) bool {
	s := &Solution{}
	for i := 0; i < 9; i++ {
		for j := 0; j < 9; j++ {
			if !s.checkValid(board, i, j) {
				return false
			}
		}
	}
	return true
}

func (s *Solution) checkValid(board [][]byte, i, j int) bool {
	if j == 0 {
		s.j1 = 0
	}
	if board[i][j] == '.' {
		return true
	}
	val := int(board[i][j] - '0')
	if s.j1 == (s.j1 | (1 << (val - 1))) {
		return false
	}
	s.j1 |= 1 << (val - 1)
	if s.i1[j] == (s.i1[j] | (1 << (val - 1))) {
		return false
	}
	s.i1[j] |= 1 << (val - 1)
	b := (i/3)*3 + j/3
	if s.b1[b] == (s.b1[b] | (1 << (val - 1))) {
		return false
	}
	s.b1[b] |= 1 << (val - 1)
	return true
}
```