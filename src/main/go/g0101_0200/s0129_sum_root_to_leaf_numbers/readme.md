[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 129\. Sum Root to Leaf Numbers

Medium

You are given the `root` of a binary tree containing digits from `0` to `9` only.

Each root-to-leaf path in the tree represents a number.

*   For example, the root-to-leaf path `1 -> 2 -> 3` represents the number `123`.

Return _the total sum of all root-to-leaf numbers_. Test cases are generated so that the answer will fit in a **32-bit** integer.

A **leaf** node is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/num1tree.jpg)

**Input:** root = [1,2,3]

**Output:** 25

**Explanation:** The root-to-leaf path `1->2` represents the number `12`. The root-to-leaf path `1->3` represents the number `13`. Therefore, sum = 12 + 13 = `25`. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/num2tree.jpg)

**Input:** root = [4,9,0,5,1]

**Output:** 1026

**Explanation:** The root-to-leaf path `4->9->5` represents the number 495. The root-to-leaf path `4->9->1` represents the number 491. The root-to-leaf path `4->0` represents the number 40. Therefore, sum = 495 + 491 + 40 = `1026`. 

**Constraints:**

*   The number of nodes in the tree is in the range `[1, 1000]`.
*   `0 <= Node.val <= 9`
*   The depth of the tree will not exceed `10`.

## Solution

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
type Solution struct {
	sum int
}

func sumNumbers(root *TreeNode) int {
	s := &Solution{sum: 0}
	s.recurseSum(root, 0)
	return s.sum
}

func (s *Solution) recurseSum(node *TreeNode, curNum int) {
	if node.Left == nil && node.Right == nil {
		s.sum += 10*curNum + node.Val
	} else {
		if node.Left != nil {
			s.recurseSum(node.Left, 10*curNum+node.Val)
		}
		if node.Right != nil {
			s.recurseSum(node.Right, 10*curNum+node.Val)
		}
	}
}
```