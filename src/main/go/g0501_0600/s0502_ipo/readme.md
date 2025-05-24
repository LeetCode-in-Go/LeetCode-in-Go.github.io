[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 502\. IPO

Hard

Suppose LeetCode will start its **IPO** soon. In order to sell a good price of its shares to Venture Capital, LeetCode would like to work on some projects to increase its capital before the **IPO**. Since it has limited resources, it can only finish at most `k` distinct projects before the **IPO**. Help LeetCode design the best way to maximize its total capital after finishing at most `k` distinct projects.

You are given `n` projects where the <code>i<sup>th</sup></code> project has a pure profit `profits[i]` and a minimum capital of `capital[i]` is needed to start it.

Initially, you have `w` capital. When you finish a project, you will obtain its pure profit and the profit will be added to your total capital.

Pick a list of **at most** `k` distinct projects from given projects to **maximize your final capital**, and return _the final maximized capital_.

The answer is guaranteed to fit in a 32-bit signed integer.

**Example 1:**

**Input:** k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]

**Output:** 4

**Explanation:** Since your initial capital is 0, you can only start the project indexed 0. After finishing it you will obtain profit 1 and your capital becomes 1. With capital 1, you can either start the project indexed 1 or the project indexed 2. Since you can choose at most 2 projects, you need to finish the project indexed 2 to get the maximum capital. Therefore, output the final maximized capital, which is 0 + 1 + 3 = 4.

**Example 2:**

**Input:** k = 3, w = 0, profits = [1,2,3], capital = [0,1,2]

**Output:** 6

**Constraints:**

*   <code>1 <= k <= 10<sup>5</sup></code>
*   <code>0 <= w <= 10<sup>9</sup></code>
*   `n == profits.length`
*   `n == capital.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= profits[i] <= 10<sup>4</sup></code>
*   <code>0 <= capital[i] <= 10<sup>9</sup></code>

## Solution

```golang
import (
	"container/heap"
)

type profitAndCapital struct {
	profit, capital int
}

type maxHeap []int

func (h maxHeap) Len() int           { return len(h) }
func (h maxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h maxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *maxHeap) Push(x any) {
	*h = append(*h, x.(int))
}

func (h *maxHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

type minHeap []profitAndCapital

func (h minHeap) Len() int           { return len(h) }
func (h minHeap) Less(i, j int) bool { return h[i].capital < h[j].capital }
func (h minHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *minHeap) Push(x any) {
	*h = append(*h, x.(profitAndCapital))
}

func (h *minHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func findMaximizedCapital(k int, currentCapital int, profits []int, capital []int) int {
	n := len(profits)
	profitHeap := &maxHeap{}
	capitalHeap := &minHeap{}
	// we gonna to a maxHeap per capital needed
	// in the max heap only store the project that we can do with the current capital
	// and in the minHeap we store the project that we can't do with the current capital
	for i := 0; i < n; i++ {
		if capital[i] <= currentCapital {
			heap.Push(profitHeap, profits[i])
		} else {
			heap.Push(capitalHeap, profitAndCapital{capital: capital[i], profit: profits[i]})
		}
	}
	// now what we need is to find a project that will maximise our profit
	// so depending on the curCapital we have we need to find the projet with the biggest profit until with reached k
	for k > 0 {
		// find the project that we can do with the current capital
		if profitHeap.Len() == 0 {
			break
		}
		currentCapital += heap.Pop(profitHeap).(int)
		for capitalHeap.Len() > 0 && (*capitalHeap)[0].capital <= currentCapital {
			x := heap.Pop(capitalHeap).(profitAndCapital)
			heap.Push(profitHeap, x.profit)
		}
		k--
	}
	return currentCapital
}
```