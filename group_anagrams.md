# Group Anagrams: Quick Reference

## Problem Description

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

### Example 1:

```plaintext
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
Explanation:
- There is no string in `strs` that can be rearranged to form "bat".
- The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
- The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.
```

### Example 2:

```plaintext
Input: strs = [""]
Output: [[""]]
```

### Example 3:

```plaintext
Input: strs = ["a"]
Output: [["a"]]
```

### Constraints:

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

---

## Code Implementation

### Python Solution:
```python
from typing import List
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        anagrams = defaultdict(list)

        for word in strs:
            # Sort the word to use as a key
            sorted_word = "".join(sorted(word))
            anagrams[sorted_word].append(word)

        return list(anagrams.values())
```

### Time Complexity:
- **O(n * k log k)**:
  - `n`: Number of strings in `strs`.
  - `k`: Average length of a string. Sorting each string takes O(k log k).

### Space Complexity:
- **O(n * k)**: Space required to store the grouped anagrams in the hash map.

---

## Alternative Approaches

### Character Count as Key
Instead of sorting, use a tuple of character counts as the hash map key.

#### Code:
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        anagrams = defaultdict(list)

        for word in strs:
            # Create a count of each character as a tuple
            char_count = [0] * 26
            for char in word:
                char_count[ord(char) - ord('a')] += 1

            anagrams[tuple(char_count)].append(word)

        return list(anagrams.values())
```

#### Time Complexity:
- **O(n * k)**: Counting characters takes O(k) for each string.

#### Space Complexity:
- **O(n * k)**: Space required for the hash map and results.

---

## Interview Tips

- Clearly explain the choice of using a sorted string or character count as the key for grouping anagrams.
- Highlight the trade-off between simplicity (sorting) and efficiency (character counts).
- Be prepared to explain edge cases, such as an empty string or single-character strings.
- Emphasize that the hash map efficiently groups anagrams and allows linear aggregation of results.
- Mention the ability to return results in any order due to hash map behavior.

