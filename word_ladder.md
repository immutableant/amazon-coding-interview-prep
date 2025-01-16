# Word Ladder: Quick Reference

## Problem Description

A transformation sequence from `beginWord` to `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

1. Every adjacent pair of words differs by a single letter.
2. Every `si` for `1 <= i <= k` is in `wordList` (note that `beginWord` does not need to be in `wordList`).
3. `sk == endWord`

**Goal:** Return the number of words in the shortest transformation sequence from `beginWord` to `endWord`, or `0` if no such sequence exists.

### Example 1:

```plaintext
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> "cog".
```

### Example 2:

```plaintext
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, so no valid transformation exists.
```

### Constraints:

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All words in `wordList` are unique.

---

## Code Implementation

### Python Solution (Using Breadth-First Search - BFS):

```python
from collections import deque
from typing import List

class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        word_set = set(wordList)
        if endWord not in word_set:
            return 0

        queue = deque([(beginWord, 1)])  # (current_word, transformation_steps)

        while queue:
            current_word, steps = queue.popleft()
            if current_word == endWord:
                return steps

            for i in range(len(current_word)):
                for c in 'abcdefghijklmnopqrstuvwxyz':
                    next_word = current_word[:i] + c + current_word[i+1:]
                    if next_word in word_set:
                        word_set.remove(next_word)
                        queue.append((next_word, steps + 1))

        return 0
```

### Time Complexity:
- **O(N × M^2)**:  
  - `N` = number of words in `wordList`  
  - `M` = length of each word  
  - For each word, we check all possible one-letter transformations.

### Space Complexity:
- **O(N × M)**: Space for the `queue` and `word_set`.

---

## Alternative Approaches

### Bidirectional BFS (Optimized Search)
Search from both `beginWord` and `endWord` to reduce search space.

#### Code:
```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        word_set = set(wordList)
        if endWord not in word_set:
            return 0

        begin_set = {beginWord}
        end_set = {endWord}
        visited = set()
        steps = 1

        while begin_set and end_set:
            if len(begin_set) > len(end_set):
                begin_set, end_set = end_set, begin_set

            next_set = set()
            for word in begin_set:
                for i in range(len(word)):
                    for c in 'abcdefghijklmnopqrstuvwxyz':
                        next_word = word[:i] + c + word[i+1:]
                        if next_word in end_set:
                            return steps + 1
                        if next_word in word_set and next_word not in visited:
                            visited.add(next_word)
                            next_set.add(next_word)

            begin_set = next_set
            steps += 1

        return 0
```

#### Time Complexity:
- **O(N × M^2)**, but faster in practice due to reduced search space.

#### Space Complexity:
- **O(N × M)**: Space for `visited` and frontier sets.

---

## Interview Tips

- Emphasize why BFS is used to find the **shortest** path (guaranteed by level-wise traversal).
- Discuss why Bidirectional BFS improves performance by cutting the search space in half.
- Explain how transforming one letter at a time allows building neighbors dynamically.
- Handle edge cases:
  - `endWord` not in `wordList` → return `0`
  - No possible transformations → return `0`
- Highlight how using a `set` optimizes lookups for word transformations.

