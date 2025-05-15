[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 30\. Substring with Concatenation of All Words

Hard

You are given a string `s` and an array of strings `words` of **the same length**. Return all starting indices of substring(s) in `s` that is a concatenation of each word in `words` **exactly once**, **in any order**, and **without any intervening characters**.

You can return the answer in **any order**.

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:** Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively. The output order does not matter, returning [9,0] is fine too. 

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** [] 

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12] 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lower-case English letters.
*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 30`
*   `words[i]` consists of lower-case English letters.

## Solution

```golang
func findSubstring(s string, words []string) []int {
	ans := make([]int, 0)
	n1 := len(words[0])
	n2 := len(s)
	map1 := make(map[string]int)
	for _, ch := range words {
		map1[ch]++
	}
	for i := 0; i < n1; i++ {
		left := i
		j := i
		c := 0
		map2 := make(map[string]int)
		for j+n1 <= n2 {
			word1 := s[j : j+n1]
			j += n1
			if _, ok := map1[word1]; ok {
				map2[word1]++
				c++
				for map2[word1] > map1[word1] {
					word2 := s[left : left+n1]
					map2[word2]--
					left += n1
					c--
				}
				if c == len(words) {
					ans = append(ans, left)
				}
			} else {
				map2 = make(map[string]int)
				c = 0
				left = j
			}
		}
	}
	return ans
}
```