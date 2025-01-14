# Longest Substring Without Repeating Characters: Quick Reference

## Problem Description

Given a string `s`, find the length of the longest substring without repeating characters.

### Example 1:

```plaintext
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

### Example 2:

```plaintext
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### Example 3:

```plaintext
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Constraints:

- `0 <= s.length <= 5 * 10^4`
- `s` consists of English letters, digits, symbols, and spaces.

---

## Code Implementation

### Python Solution:
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_set = set()
        left = 0
        max_length = 0

        for right in range(len(s)):
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1
            char_set.add(s[right])
            max_length = max(max_length, right - left + 1)

        return max_length
```

### Time Complexity:
- **O(n)**: The `right` pointer traverses the string once, and the `left` pointer only moves forward.

### Space Complexity:
- **O(k)**: Where `k` is the number of unique characters in the string.

---

## Alternative Approaches

### Using a Hash Map to Track Character Indices
Instead of a set, use a hash map to track the last seen index of each character.

#### Code:
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_index = {}
        left = 0
        max_length = 0

        for right, char in enumerate(s):
            if char in char_index and char_index[char] >= left:
                left = char_index[char] + 1
            char_index[char] = right
            max_length = max(max_length, right - left + 1)

        return max_length
```

#### Time Complexity:
- **O(n)**: Each character is visited at most twice.

#### Space Complexity:
- **O(k)**: Where `k` is the number of unique characters in the string.

---

## Interview Tips

- Clearly explain the sliding window approach and how it avoids rechecking characters by dynamically adjusting the window size.
- Be prepared to discuss edge cases, such as an empty string, a string with all identical characters, or a string with no repeating characters.
- Emphasize why the hash map approach is efficient and how it optimizes lookups and updates.
- Mention that the hash map version can handle a wider range of scenarios due to its ability to track indices.

