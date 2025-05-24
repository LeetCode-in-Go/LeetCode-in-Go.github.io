[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 399\. Evaluate Division

Medium

You are given an array of variable pairs `equations` and an array of real numbers `values`, where <code>equations[i] = [A<sub>i</sub>, B<sub>i</sub>]</code> and `values[i]` represent the equation <code>A<sub>i</sub> / B<sub>i</sub> = values[i]</code>. Each <code>A<sub>i</sub></code> or <code>B<sub>i</sub></code> is a string that represents a single variable.

You are also given some `queries`, where <code>queries[j] = [C<sub>j</sub>, D<sub>j</sub>]</code> represents the <code>j<sup>th</sup></code> query where you must find the answer for <code>C<sub>j</sub> / D<sub>j</sub> = ?</code>.

Return _the answers to all queries_. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Example 1:**

**Input:** equations = \[\["a","b"],["b","c"]], values = [2.0,3.0], queries = \[\["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]

**Output:** [6.00000,0.50000,-1.00000,1.00000,-1.00000]

**Explanation:** Given: _a / b = 2.0_, _b / c = 3.0_ queries are: _a / c = ?_, _b / a = ?_, _a / e = ?_, _a / a = ?_, _x / x = ?_ return: [6.0, 0.5, -1.0, 1.0, -1.0 ]

**Example 2:**

**Input:** equations = \[\["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = \[\["a","c"],["c","b"],["bc","cd"],["cd","bc"]]

**Output:** [3.75000,0.40000,5.00000,0.20000]

**Example 3:**

**Input:** equations = \[\["a","b"]], values = [0.5], queries = \[\["a","b"],["b","a"],["a","c"],["x","y"]]

**Output:** [0.50000,2.00000,-1.00000,-1.00000]

**Constraints:**

*   `1 <= equations.length <= 20`
*   `equations[i].length == 2`
*   <code>1 <= A<sub>i</sub>.length, B<sub>i</sub>.length <= 5</code>
*   `values.length == equations.length`
*   `0.0 < values[i] <= 20.0`
*   `1 <= queries.length <= 20`
*   `queries[i].length == 2`
*   <code>1 <= C<sub>j</sub>.length, D<sub>j</sub>.length <= 5</code>
*   <code>A<sub>i</sub>, B<sub>i</sub>, C<sub>j</sub>, D<sub>j</sub></code> consist of lower case English letters and digits.

## Solution

```golang
type Solution struct {
	root map[string]string
	rate map[string]float64
}

func calcEquation(equations [][]string, values []float64, queries [][]string) []float64 {
	sol := &Solution{
		root: make(map[string]string),
		rate: make(map[string]float64),
	}
	n := len(equations)
	for _, equation := range equations {
		x := equation[0]
		y := equation[1]
		sol.root[x] = x
		sol.root[y] = y
		sol.rate[x] = 1.0
		sol.rate[y] = 1.0
	}
	for i := 0; i < n; i++ {
		x := equations[i][0]
		y := equations[i][1]
		sol.union(x, y, values[i])
	}
	result := make([]float64, len(queries))
	for i, query := range queries {
		x := query[0]
		y := query[1]
		if _, exists := sol.root[x]; !exists {
			result[i] = -1.0
			continue
		}
		if _, exists := sol.root[y]; !exists {
			result[i] = -1.0
			continue
		}
		rootX := sol.findRoot(x, x, 1.0)
		rootY := sol.findRoot(y, y, 1.0)
		if rootX == rootY {
			result[i] = sol.rate[x] / sol.rate[y]
		} else {
			result[i] = -1.0
		}
	}
	return result
}

func (sol *Solution) union(x, y string, v float64) {
	rootX := sol.findRoot(x, x, 1.0)
	rootY := sol.findRoot(y, y, 1.0)
	sol.root[rootX] = rootY
	r1 := sol.rate[x]
	r2 := sol.rate[y]
	sol.rate[rootX] = v * r2 / r1
}

func (sol *Solution) findRoot(originalX, x string, r float64) string {
	if sol.root[x] == x {
		sol.root[originalX] = x
		sol.rate[originalX] = r * sol.rate[x]
		return x
	}
	return sol.findRoot(originalX, sol.root[x], r*sol.rate[x])
}
```