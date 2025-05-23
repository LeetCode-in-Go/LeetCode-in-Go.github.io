[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 120\. Triangle

Medium

Given a `triangle` array, return _the minimum path sum from top to bottom_.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**

**Input:** triangle = \[\[2],[3,4],[6,5,7],[4,1,8,3]]

**Output:** 11

**Explanation:** The triangle looks like: 
    
<ins>2</ins> 

<ins>3</ins> 4 

6 <ins>5</ins> 7 

4 <ins>1</ins> 8 3 

The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

**Example 2:**

**Input:** triangle = \[\[-10]]

**Output:** -10

**Constraints:**

*   `1 <= triangle.length <= 200`
*   `triangle[0].length == 1`
*   `triangle[i].length == triangle[i - 1].length + 1`
*   <code>-10<sup>4</sup> <= triangle[i][j] <= 10<sup>4</sup></code>

**Follow up:** Could you do this using only `O(n)` extra space, where `n` is the total number of rows in the triangle?

## Solution

```golang
func minimumTotal(triangle [][]int) int {
	if len(triangle) == 0 {
		return 0
	}
	dp := make([][]int, len(triangle))
	for i := range dp {
		dp[i] = make([]int, len(triangle[len(triangle)-1]))
		for j := range dp[i] {
			dp[i][j] = -10001
		}
	}
	return dfs(triangle, dp, 0, 0)
}

func dfs(triangle [][]int, dp [][]int, row, col int) int {
	if row >= len(triangle) {
		return 0
	}
	if dp[row][col] != -10001 {
		return dp[row][col]
	}
	sum := triangle[row][col] + min(dfs(triangle, dp, row+1, col), dfs(triangle, dp, row+1, col+1))
	dp[row][col] = sum
	return sum
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```