[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 452\. Minimum Number of Arrows to Burst Balloons

Medium

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where <code>points[i] = [x<sub>start</sub>, x<sub>end</sub>]</code> denotes a balloon whose **horizontal diameter** stretches between <code>x<sub>start</sub></code> and <code>x<sub>end</sub></code>. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with <code>x<sub>start</sub></code> and <code>x<sub>end</sub></code> is **burst** by an arrow shot at `x` if <code>x<sub>start</sub> <= x <= x<sub>end</sub></code>. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return _the **minimum** number of arrows that must be shot to burst all balloons_.

**Example 1:**

**Input:** points = \[\[10,16],[2,8],[1,6],[7,12]]

**Output:** 2

**Explanation:** The balloons can be burst by 2 arrows:

- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].

- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].

**Example 2:**

**Input:** points = \[\[1,2],[3,4],[5,6],[7,8]]

**Output:** 4

**Explanation:** One arrow needs to be shot for each balloon for a total of 4 arrows.

**Example 3:**

**Input:** points = \[\[1,2],[2,3],[3,4],[4,5]]

**Output:** 2

**Explanation:** The balloons can be burst by 2 arrows:

- Shoot an arrow at x = 2, bursting the balloons [1,2] and [2,3].

- Shoot an arrow at x = 4, bursting the balloons [3,4] and [4,5].

**Constraints:**

*   <code>1 <= points.length <= 10<sup>5</sup></code>
*   `points[i].length == 2`
*   <code>-2<sup>31</sup> <= x<sub>start</sub> < x<sub>end</sub> <= 2<sup>31</sup> - 1</code>

## Solution

```golang
import "slices"

func findMinArrowShots(points [][]int) int {
	slices.SortFunc(points, func(acc []int, b []int) int {
		if acc[0] < b[0] {
			return -1
		} else if acc[0] > b[0] {
			return +1
		}
		return 0
	})
	openInterval := points[0]
	runningArrows := 1
	for i := 1; i < len(points); i++ {
		curr := points[i]
		// fmt.Printf("current: %v |  open interval %v\n", curr, openInterval)
		// intersects with the running interval?
		if curr[0] <= openInterval[1] {
			// we can use the same arrow!
			// let's adjust the running interval
			openInterval[0] = max(openInterval[0], curr[0])
			openInterval[1] = min(openInterval[1], curr[1])
		} else {
			runningArrows++
			openInterval = curr
		}
	}
	return runningArrows
}
```