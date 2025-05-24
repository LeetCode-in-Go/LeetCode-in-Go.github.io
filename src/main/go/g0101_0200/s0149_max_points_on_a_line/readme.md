[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 149\. Max Points on a Line

Hard

Given an array of `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents a point on the **X-Y** plane, return _the maximum number of points that lie on the same straight line_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/25/plane1.jpg)

**Input:** points = \[\[1,1],[2,2],[3,3]]

**Output:** 3

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/25/plane2.jpg)

**Input:** points = \[\[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]

**Output:** 4

**Constraints:**

*   `1 <= points.length <= 300`
*   `points[i].length == 2`
*   <code>-10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>4</sup></code>
*   All the `points` are **unique**.

## Solution

```golang
func maxPoints(points [][]int) int {
	if len(points) < 2 {
		return len(points)
	}
	max := 0
	for i := 0; i < len(points); i++ {
		for j := i + 1; j < len(points); j++ {
			x := points[j][0] - points[i][0]
			y := points[j][1] - points[i][1]
			c := x*points[j][1] - y*points[j][0]
			count := 2
			for k := j + 1; k < len(points); k++ {
				if c == x*points[k][1]-y*points[k][0] {
					count++
				}
			}
			if count > max {
				max = count
			}
		}
	}
	return max
}
```