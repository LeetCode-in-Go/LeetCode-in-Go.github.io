[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 202\. Happy Number

Easy

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

*   Starting with any positive integer, replace the number by the sum of the squares of its digits.
*   Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
*   Those numbers for which this process **ends in 1** are happy.

Return `true` _if_ `n` _is a happy number, and_ `false` _if not_.

**Example 1:**

**Input:** n = 19

**Output:** true

**Explanation:** 1<sup>2</sup> + 9<sup>2</sup> = 82 8<sup>2</sup> + 2<sup>2</sup> = 68 6<sup>2</sup> + 8<sup>2</sup> = 100 1<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> = 1

**Example 2:**

**Input:** n = 2

**Output:** false

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```golang
func isHappy(n int) bool {
	if n == 1 || n == 7 {
		return true
	}
	if n > 1 && n < 10 {
		return false
	}
	sum := 0
	a := n
	for a != 0 {
		rem := a % 10
		sum += rem * rem
		a /= 10
	}
	if sum != 1 {
		return isHappy(sum)
	}
	return true
}
```