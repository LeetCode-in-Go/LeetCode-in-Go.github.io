[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 199\. Binary Tree Right Side View

Medium

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

**Input:** root = [1,2,3,null,5,null,4]

**Output:** [1,3,4]

**Example 2:**

**Input:** root = [1,null,3]

**Output:** [1,3]

**Example 3:**

**Input:** root = []

**Output:** []

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 100]`.
*   `-100 <= Node.val <= 100`

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
func rightSideView(root *TreeNode) []int {
	var list []int
	recurse(root, 0, &list)
	return list
}

func recurse(node *TreeNode, level int, list *[]int) {
	if node != nil {
		if len(*list) < level+1 {
			*list = append(*list, node.Val)
		}
		recurse(node.Right, level+1, list)
		recurse(node.Left, level+1, list)
	}
}
```