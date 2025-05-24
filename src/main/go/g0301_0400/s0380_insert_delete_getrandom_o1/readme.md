[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 380\. Insert Delete GetRandom O(1)

Medium

Implement the `RandomizedSet` class:

*   `RandomizedSet()` Initializes the `RandomizedSet` object.
*   `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
*   `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
*   `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.

You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

**Example 1:**

**Input** 

    ["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"] 
    [[], [1], [2], [2], [], [1], [2], []]

**Output:** [null, true, false, true, 2, true, false, 2]

**Explanation:** 

    RandomizedSet randomizedSet = new RandomizedSet(); 
    randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully. 
    randomizedSet.remove(2); // Returns false as 2 does not exist in the set. 
    randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2]. 
    randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly. 
    randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2]. 
    randomizedSet.insert(2); // 2 was already in the set, so return false. 
    randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.

**Constraints:**

*   <code>-2<sup>31</sup> <= val <= 2<sup>31</sup> - 1</code>
*   At most `2 * `<code>10<sup>5</sup></code> calls will be made to `insert`, `remove`, and `getRandom`.
*   There will be **at least one** element in the data structure when `getRandom` is called.

## Solution

```golang
import (
	"math/rand"
)

type RandomizedSet struct {
	list []int
	dict map[int]int
}

func Constructor() RandomizedSet {
	return RandomizedSet{
		list: make([]int, 0),
		dict: make(map[int]int),
	}
}

func (this *RandomizedSet) Insert(val int) bool {
	if _, exists := this.dict[val]; exists {
		return false
	}
	this.dict[val] = len(this.list)
	this.list = append(this.list, val)
	return true
}

func (this *RandomizedSet) Remove(val int) bool {
	if _, exists := this.dict[val]; !exists {
		return false
	}
	last := len(this.list) - 1
	lastVal := this.list[last]
	idx := this.dict[val]
	this.list[idx] = lastVal
	this.dict[lastVal] = idx
	this.list = this.list[:last]
	delete(this.dict, val)
	return true
}

func (this *RandomizedSet) GetRandom() int {
	return this.list[rand.Intn(len(this.list))] // NOSONAR
}
```