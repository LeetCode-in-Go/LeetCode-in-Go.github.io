[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 125\. Valid Palindrome

Easy

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` _if it is a **palindrome**, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "A man, a plan, a canal: Panama"

**Output:** true

**Explanation:** "amanaplanacanalpanama" is a palindrome.

**Example 2:**

**Input:** s = "race a car"

**Output:** false

**Explanation:** "raceacar" is not a palindrome.

**Example 3:**

**Input:** s = " "

**Output:** true

**Explanation:** s is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome.

**Constraints:**

*   <code>1 <= s.length <= 2 * 10<sup>5</sup></code>
*   `s` consists only of printable ASCII characters.

## Solution

```golang
func isPalindrome(s string) bool {
	i := 0
	j := len(s) - 1
	res := true
	for res {
		for i < j && isNotAlphaNumeric(s[i]) {
			i++
		}
		for i < j && isNotAlphaNumeric(s[j]) {
			j--
		}
		if i >= j {
			break
		}
		left := upperToLower(s[i])
		right := upperToLower(s[j])
		if left != right {
			res = false
		}
		i++
		j--
	}
	return res
}

func isNotAlphaNumeric(c byte) bool {
	return (c < 'a' || c > 'z') && (c < 'A' || c > 'Z') && (c < '0' || c > '9')
}

func isUpper(c byte) bool {
	return c >= 'A' && c <= 'Z'
}

func upperToLower(c byte) byte {
	if isUpper(c) {
		c += 32
	}
	return c
}
```