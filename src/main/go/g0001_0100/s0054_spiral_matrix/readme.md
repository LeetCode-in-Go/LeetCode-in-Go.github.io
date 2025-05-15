[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 54\. Spiral Matrix

Medium

Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

**Input:** matrix = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,2,3,6,9,8,7,4,5] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

**Input:** matrix = \[\[1,2,3,4],[5,6,7,8],[9,10,11,12]]

**Output:** [1,2,3,4,8,12,11,10,9,5,6,7] 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 10`
*   `-100 <= matrix[i][j] <= 100`

## Solution

```golang
func spiralOrder(matrix [][]int) []int {
	list := make([]int, 0)
	r := 0
	c := 0
	bigR := len(matrix) - 1
	bigC := len(matrix[0]) - 1
	for r <= bigR && c <= bigC {
		for i := c; i <= bigC; i++ {
			list = append(list, matrix[r][i])
		}
		r++
		for i := r; i <= bigR; i++ {
			list = append(list, matrix[i][bigC])
		}
		bigC--
		for i := bigC; i >= c && r <= bigR; i-- {
			list = append(list, matrix[bigR][i])
		}
		bigR--
		for i := bigR; i >= r && c <= bigC; i-- {
			list = append(list, matrix[i][c])
		}
		c++
	}
	return list
}
```