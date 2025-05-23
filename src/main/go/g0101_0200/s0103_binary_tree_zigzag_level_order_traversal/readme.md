[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 103\. Binary Tree Zigzag Level Order Traversal

Medium

Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]

**Output:** [[3],[20,9],[15,7]] 

**Example 2:**

**Input:** root = [1]

**Output:** [[1]] 

**Example 3:**

**Input:** root = []

**Output:** [] 

**Constraints:**

*   The number of nodes in the tree is in the range `[0, 2000]`.
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
func zigzagLevelOrder(root *TreeNode) [][]int {
	var result [][]int
	if root == nil {
		return result
	}
	queue := []*TreeNode{root, nil}
	zig := true
	var level []int
	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		for node != nil {
			if zig {
				level = append(level, node.Val)
			} else {
				level = append([]int{node.Val}, level...)
			}
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
			if len(queue) > 0 {
				node = queue[0]
				queue = queue[1:]
			} else {
				node = nil
			}
		}
		result = append(result, level)
		zig = !zig
		level = []int{}
		if len(queue) > 0 {
			queue = append(queue, nil)
		}
	}
	return result
}
```