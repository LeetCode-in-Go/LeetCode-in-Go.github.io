[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 150\. Evaluate Reverse Polish Notation

Medium

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

**Example 1:**

**Input:** tokens = ["2","1","+","3","\*"]

**Output:** 9

**Explanation:** ((2 + 1) \* 3) = 9

**Example 2:**

**Input:** tokens = ["4","13","5","/","+"]

**Output:** 6

**Explanation:** (4 + (13 / 5)) = 6

**Example 3:**

**Input:** tokens = ["10","6","9","3","+","-11","\*","/","\*","17","+","5","+"]

**Output:** 22

**Explanation:** ((10 \* (6 / ((9 + 3) \* -11))) + 17) + 5 

= ((10 \* (6 / (12 \* -11))) + 17) + 5 

= ((10 \* (6 / -132)) + 17) + 5 

= ((10 \* 0) + 17) + 5 

= (0 + 17) + 5 

= 17 + 5 

= 22

**Constraints:**

*   <code>1 <= tokens.length <= 10<sup>4</sup></code>
*   `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.

## Solution

```golang
func evalRPN(tokens []string) int {
	stack := make([]int, 0)
	for _, token := range tokens {
		if len(token) == 1 && !isDigit(token[0]) {
			second := stack[len(stack)-1]
			first := stack[len(stack)-2]
			stack = stack[:len(stack)-2]
			stack = append(stack, eval(first, second, token))
		} else {
			num := 0
			sign := 1
			i := 0
			if token[0] == '-' {
				sign = -1
				i = 1
			}
			for ; i < len(token); i++ {
				num = num*10 + int(token[i]-'0')
			}
			stack = append(stack, num*sign)
		}
	}
	return stack[0]
}

func eval(first, second int, operator string) int {
	switch operator {
	case "+":
		return first + second
	case "-":
		return first - second
	case "*":
		return first * second
	default:
		return first / second
	}
}

func isDigit(c byte) bool {
	return c >= '0' && c <= '9'
}
```