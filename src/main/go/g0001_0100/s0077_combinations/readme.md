[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 77\. Combinations

Medium

Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers out of the range_ `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

**Input:** n = 4, k = 2

**Output:** [ [2,4], [3,4], [2,3], [1,2], [1,3], [1,4], ] 

**Example 2:**

**Input:** n = 1, k = 1

**Output:** [[1]] 

**Constraints:**

*   `1 <= n <= 20`
*   `1 <= k <= n`

## Solution

```golang
func combine(n int, k int) [][]int {
	ans := make([][]int, 0)
	if n > 20 || k < 1 || k > n {
		return ans
	}
	backtrack(&ans, n, k, 1, make([]int, 0))
	return ans
}

func backtrack(ans *[][]int, n, k, s int, stack []int) {
	if k == 0 {
		*ans = append(*ans, append([]int{}, stack...))
		return
	}
	for i := s; i <= (n-k)+1; i++ {
		stack = append(stack, i)
		backtrack(ans, n, k-1, i+1, stack)
		stack = stack[:len(stack)-1]
	}
}
```