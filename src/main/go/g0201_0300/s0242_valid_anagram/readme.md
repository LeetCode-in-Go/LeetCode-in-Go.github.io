[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 242\. Valid Anagram

Easy

Given two strings `s` and `t`, return `true` _if_ `t` _is an anagram of_ `s`_, and_ `false` _otherwise_.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** s = "anagram", t = "nagaram"

**Output:** true

**Example 2:**

**Input:** s = "rat", t = "car"

**Output:** false

**Constraints:**

*   <code>1 <= s.length, t.length <= 5 * 10<sup>4</sup></code>
*   `s` and `t` consist of lowercase English letters.

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

## Solution

```golang
func isAnagram(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}
	charFreqMap := make([]int, 26)
	for _, c := range s {
		charFreqMap[c-'a']++
	}
	for _, c := range t {
		if charFreqMap[c-'a'] == 0 {
			return false
		}
		charFreqMap[c-'a']--
	}
	return true
}
```