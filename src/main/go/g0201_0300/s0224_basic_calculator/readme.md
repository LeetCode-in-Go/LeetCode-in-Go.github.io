[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 224\. Basic Calculator

Hard

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return _the result of the evaluation_.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

**Input:** s = "1 + 1"

**Output:** 2 

**Example 2:**

**Input:** s = " 2-1 + 2 "

**Output:** 3 

**Example 3:**

**Input:** s = "(1+(4+5+2)-3)+(6+8)"

**Output:** 23 

**Constraints:**

*   <code>1 <= s.length <= 3 * 10<sup>5</sup></code>
*   `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
*   `s` represents a valid expression.
*   `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
*   `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
*   There will be no two consecutive operators in the input.
*   Every number and running calculation will fit in a signed 32-bit integer.

## Solution

```golang
type Solution struct {
	i int
}

func calculate(s string) int {
	sol := &Solution{}
	return sol.helper([]rune(s))
}

func (sol *Solution) helper(ca []rune) int {
	num := 0
	prenum := 0
	isPlus := true
	for ; sol.i < len(ca); sol.i++ {
		c := ca[sol.i]
		if c != ' ' {
			if c >= '0' && c <= '9' {
				if num == 0 {
					num = int(c - '0')
				} else {
					num = num*10 + int(c-'0')
				}
			} else if c == '+' {
				prenum += num * sol.boolToInt(isPlus)
				isPlus = true
				num = 0
			} else if c == '-' {
				prenum += num * sol.boolToInt(isPlus)
				num = 0
				isPlus = false
			} else if c == '(' {
				sol.i++
				prenum += sol.helper(ca) * sol.boolToInt(isPlus)
				isPlus = true
				num = 0
			} else if c == ')' {
				return prenum + num*sol.boolToInt(isPlus)
			}
		}
	}
	return prenum + num*sol.boolToInt(isPlus)
}

func (sol *Solution) boolToInt(b bool) int {
	if b {
		return 1
	}
	return -1
}
```