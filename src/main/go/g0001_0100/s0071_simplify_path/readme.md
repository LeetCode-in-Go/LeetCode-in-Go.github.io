[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 71\. Simplify Path

Medium

Given a string `path`, which is an **absolute path** (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified **canonical path**.

In a Unix-style file system, a period `'.'` refers to the current directory, a double period `'..'` refers to the directory up a level, and any multiple consecutive slashes (i.e. `'//'`) are treated as a single slash `'/'`. For this problem, any other format of periods such as `'...'` are treated as file/directory names.

The **canonical path** should have the following format:

*   The path starts with a single slash `'/'`.
*   Any two directories are separated by a single slash `'/'`.
*   The path does not end with a trailing `'/'`.
*   The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `'.'` or double period `'..'`)

Return _the simplified **canonical path**_.

**Example 1:**

**Input:** path = "/home/"

**Output:** "/home"

**Explanation:** Note that there is no trailing slash after the last directory name. 

**Example 2:**

**Input:** path = "/../"

**Output:** "/"

**Explanation:** Going one level up from the root directory is a no-op, as the root level is the highest level you can go. 

**Example 3:**

**Input:** path = "/home//foo/"

**Output:** "/home/foo"

**Explanation:** In the canonical path, multiple consecutive slashes are replaced by a single one. 

**Example 4:**

**Input:** path = "/a/./b/../../c/"

**Output:** "/c" 

**Constraints:**

*   `1 <= path.length <= 3000`
*   `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
*   `path` is a valid absolute Unix path.

## Solution

```golang
func simplifyPath(path string) string {
	stk := make([]string, 0)
	start := 0
	for start < len(path) {
		for start < len(path) && path[start] == '/' {
			start++
		}
		end := start
		for end < len(path) && path[end] != '/' {
			end++
		}
		s := path[start:end]
		if s == ".." {
			if len(stk) > 0 {
				stk = stk[:len(stk)-1]
			}
		} else if s != "." && s != "" {
			stk = append(stk, s)
		}
		start = end + 1
	}
	if len(stk) == 0 {
		return "/"
	}
	ans := ""
	for _, s := range stk {
		ans += "/" + s
	}
	return ans
}
```