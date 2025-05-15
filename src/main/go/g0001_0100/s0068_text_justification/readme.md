[![](https://img.shields.io/github/stars/LeetCode-in-Go/LeetCode-in-Go?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go)
[![](https://img.shields.io/github/forks/LeetCode-in-Go/LeetCode-in-Go?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Go/LeetCode-in-Go/fork)

## 68\. Text Justification

Hard

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified and no extra space is inserted between words.

**Note:**

*   A word is defined as a character sequence consisting of non-space characters only.
*   Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
*   The input array `words` contains at least one word.

**Example 1:**

**Input:** words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16

**Output:** [ "This is an", "example of text", "justification. " ]

**Example 2:**

**Input:** words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16

**Output:** [ "What must be", "acknowledgment ", "shall be " ]

**Explanation:** Note that the last line is "shall be " instead of "shall be", because the last line must be left-justified instead of fully-justified. Note that the second line is also left-justified becase it contains only one word.

**Example 3:**

**Input:** words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20

**Output:** [ "Science is what we", "understand well", "enough to explain to", "a computer. Art is", "everything else we", "do " ]

**Constraints:**

*   `1 <= words.length <= 300`
*   `1 <= words[i].length <= 20`
*   `words[i]` consists of only English letters and symbols.
*   `1 <= maxWidth <= 100`
*   `words[i].length <= maxWidth`

## Solution

```golang
func fullJustify(words []string, maxWidth int) []string {
	output := make([]string, 0, (len(words)+1)/(1+maxWidth/7))
	sb := make([]byte, 0, maxWidth)
	lineTotal := 0
	numWordsOnLine := 0
	startWord := 0

	for i := 0; i < len(words)-1; i++ {
		lineTotal += len(words[i])
		numWordsOnLine++
		if lineTotal+numWordsOnLine+len(words[i+1]) > maxWidth {
			if numWordsOnLine == 1 {
				sb = append(sb, words[i]...)
				for lineTotal < maxWidth {
					sb = append(sb, ' ')
					lineTotal++
				}
			} else {
				extraSp := (maxWidth - lineTotal) % (numWordsOnLine - 1)
				for j := startWord; j < startWord+numWordsOnLine-1; j++ {
					sb = append(sb, words[j]...)
					if extraSp > 0 {
						sb = append(sb, ' ')
						extraSp--
					}
					for k := 0; k < (maxWidth-lineTotal)/(numWordsOnLine-1); k++ {
						sb = append(sb, ' ')
					}
				}
				sb = append(sb, words[startWord+numWordsOnLine-1]...)
			}
			output = append(output, string(sb))
			startWord = i + 1
			numWordsOnLine = 0
			lineTotal = 0
			sb = make([]byte, 0, maxWidth)
		}
	}

	lineTotal = 0
	for i := startWord; i < len(words); i++ {
		lineTotal += len(words[i])
		sb = append(sb, words[i]...)
		if lineTotal < maxWidth {
			sb = append(sb, ' ')
			lineTotal++
		}
	}
	for lineTotal < maxWidth {
		sb = append(sb, ' ')
		lineTotal++
	}
	output = append(output, string(sb))
	return output
}
```