[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 647\. Palindromic Substrings

Medium

Given a string `s`, return _the number of **palindromic substrings** in it_.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"

**Output:** 3

**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

**Input:** s = "aaa"

**Output:** 6

**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of lowercase English letters.

## Solution

```golang
func expand(a []rune, l int, r int, res *int) {
	for l >= 0 && r < len(a) {
		if a[l] != a[r] {
			return
		} else {
			*res++
			l--
			r++
		}
	}
}

func countSubstrings(s string) int {
	a := []rune(s)
	res := 0
	for i := 0; i < len(a); i++ {
		expand(a, i, i, &res)
		expand(a, i, i+1, &res)
	}
	return res
}
```