[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 205\. Isomorphic Strings

Easy

Given two strings `s` and `t`, _determine if they are isomorphic_.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

**Example 1:**

**Input:** s = "egg", t = "add"

**Output:** true

**Example 2:**

**Input:** s = "foo", t = "bar"

**Output:** false

**Example 3:**

**Input:** s = "paper", t = "title"

**Output:** true

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `t.length == s.length`
*   `s` and `t` consist of any valid ascii character.

## Solution

```golang
func isIsomorphic(s string, t string) bool {
	charMap := make([]int, 128)
	str := []rune(s)
	tar := []rune(t)
	n := len(str)
	for i := 0; i < n; i++ {
		if charMap[tar[i]] == 0 {
			if search(charMap, str[i], tar[i]) != -1 {
				return false
			}
			charMap[tar[i]] = int(str[i])
		} else {
			if charMap[tar[i]] != int(str[i]) {
				return false
			}
		}
	}
	return true
}

func search(charMap []int, tar rune, skip rune) int {
	for i := 0; i < 128; i++ {
		if i == int(skip) {
			continue
		}
		if charMap[i] != 0 && charMap[i] == int(tar) {
			return i
		}
	}
	return -1
}
```