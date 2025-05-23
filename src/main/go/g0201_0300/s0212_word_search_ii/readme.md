[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 212\. Word Search II

Hard

Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

**Input:** board = \[\["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]

**Output:** ["eat","oath"] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

**Input:** board = \[\["a","b"],["c","d"]], words = ["abcb"]

**Output:** [] 

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 12`
*   `board[i][j]` is a lowercase English letter.
*   <code>1 <= words.length <= 3 * 10<sup>4</sup></code>
*   `1 <= words[i].length <= 10`
*   `words[i]` consists of lowercase English letters.
*   All the strings of `words` are unique.

## Solution

```golang
var root *Tree

func (t *Tree) len() int {
	return len(t.children)
}

func (t *Tree) getChild(c byte) *Tree {
	return t.children[c]
}

func addWord(root *Tree, word string) {
	cur := root
	for i := 0; i < len(word); i++ {
		c := word[i]
		if cur.children == nil {
			cur.children = make(map[byte]*Tree)
		}
		if cur.children[c] == nil {
			cur.children[c] = &Tree{}
		}
		cur = cur.children[c]
	}
	cur.end = word
}

func deleteWord(root *Tree, word string) {
	cur := root
	for i := 0; i < len(word); i++ {
		c := word[i]
		if cur.children == nil {
			return
		}
		next := cur.children[c]
		if next == nil {
			return
		}
		if i == len(word)-1 {
			delete(cur.children, c)
		}
		cur = next
	}
}

func findWords(board [][]byte, words []string) []string {
	if len(board) < 1 || len(board[0]) < 1 {
		return []string{}
	}
	root = &Tree{}
	for _, word := range words {
		addWord(root, word)
	}
	collected := make([]string, 0)
	for i := 0; i < len(board); i++ {
		for j := 0; j < len(board[0]); j++ {
			dfs(board, i, j, root, &collected)
		}
	}
	return collected
}

func dfs(board [][]byte, i, j int, cur *Tree, collected *[]string) {
	c := board[i][j]
	if c == '-' {
		return
	}
	cur = cur.getChild(c)
	if cur == nil {
		return
	}
	if cur.end != "" {
		s := cur.end
		*collected = append(*collected, s)
		cur.end = ""
		if cur.len() == 0 {
			deleteWord(root, s)
		}
	}
	board[i][j] = '-'
	if i > 0 {
		dfs(board, i-1, j, cur, collected)
	}
	if i+1 < len(board) {
		dfs(board, i+1, j, cur, collected)
	}
	if j > 0 {
		dfs(board, i, j-1, cur, collected)
	}
	if j+1 < len(board[0]) {
		dfs(board, i, j+1, cur, collected)
	}
	board[i][j] = c
}
```