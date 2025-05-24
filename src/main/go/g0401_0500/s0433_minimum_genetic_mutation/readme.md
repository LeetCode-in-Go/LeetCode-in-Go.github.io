[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 433\. Minimum Genetic Mutation

Medium

A gene string can be represented by an 8-character long string, with choices from `'A'`, `'C'`, `'G'`, and `'T'`.

Suppose we need to investigate a mutation from a gene string `startGene` to a gene string `endGene` where one mutation is defined as one single character changed in the gene string.

*   For example, `"AACCGGTT" --> "AACCGGTA"` is one mutation.

There is also a gene bank `bank` that records all the valid gene mutations. A gene must be in `bank` to make it a valid gene string.

Given the two gene strings `startGene` and `endGene` and the gene bank `bank`, return _the minimum number of mutations needed to mutate from_ `startGene` _to_ `endGene`. If there is no such a mutation, return `-1`.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

**Example 1:**

**Input:** startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]

**Output:** 1

**Example 2:**

**Input:** startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]

**Output:** 2

**Constraints:**

*   `0 <= bank.length <= 10`
*   `startGene.length == endGene.length == bank[i].length == 8`
*   `startGene`, `endGene`, and `bank[i]` consist of only the characters `['A', 'C', 'G', 'T']`.

## Solution

```golang
func isInBank(set map[string]bool, cur string) []string {
	var res []string
	for each := range set {
		diff := 0
		for i := 0; i < len(each); i++ {
			if each[i] != cur[i] {
				diff++
				if diff > 1 {
					break
				}
			}
		}
		if diff == 1 {
			res = append(res, each)
		}
	}
	return res
}

func minMutation(start string, end string, bank []string) int {
	set := make(map[string]bool)
	for _, s := range bank {
		set[s] = true
	}
	queue := []string{start}
	step := 0
	for len(queue) > 0 {
		curSize := len(queue)
		for curSize > 0 {
			cur := queue[0]
			queue = queue[1:]
			if cur == end {
				return step
			}
			for _, next := range isInBank(set, cur) {
				queue = append(queue, next)
				delete(set, next)
			}
			curSize--
		}
		step++
	}
	return -1
}
```