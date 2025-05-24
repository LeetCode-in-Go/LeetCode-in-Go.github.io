[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 222\. Count Complete Tree Nodes

Medium

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and <code>2<sup>h</sup></code> nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

**Input:** root = [1,2,3,4,5,6]

**Output:** 6

**Example 2:**

**Input:** root = []

**Output:** 0

**Example 3:**

**Input:** root = [1]

**Output:** 1

**Constraints:**

*   The number of nodes in the tree is in the range <code>[0, 5 * 10<sup>4</sup>]</code>.
*   <code>0 <= Node.val <= 5 * 10<sup>4</sup></code>
*   The tree is guaranteed to be **complete**.

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
func countNodes(root *TreeNode) int {
	if root == nil {
		return 0
	}
	leftHeight := leftHeight(root)
	rightHeight := rightHeight(root)
	// case 1: When Height(Left sub-tree) = Height(right sub-tree) 2^h - 1
	if leftHeight == rightHeight {
		return (1 << leftHeight) - 1
	} else {
		return 1 + countNodes(root.Left) + countNodes(root.Right)
	}
}

func leftHeight(root *TreeNode) int {
	if root == nil {
		return 0
	}
	return 1 + leftHeight(root.Left)
}

func rightHeight(root *TreeNode) int {
	if root == nil {
		return 0
	}
	return 1 + rightHeight(root.Right)
}
```