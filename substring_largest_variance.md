# Substring With Largest Variance: Quick Reference

## Problem Description

The **variance** of a string is defined as the largest difference between the number of occurrences of any two characters present in the string. Note that the two characters may or may not be the same.

Given a string `s` consisting of lowercase English letters only, return the **largest variance** possible among all substrings of `s`.

A **substring** is a contiguous sequence of characters within a string.

### Example 1:

```plaintext
Input: s = "aababbb"
Output: 3
Explanation:
The substring "babbb" has the largest variance of 3.
```

### Example 2:

```plaintext
Input: s = "abcde"
Output: 0
Explanation:
No letter occurs more than once in `s`, so the variance of every substring is 0.
```

### Constraints:

- `1 <= s.length <= 10^4`
- `s` consists of lowercase English letters.

---

## Optimized Code Implementation

### Python Solution (Efficient Kadane’s Algorithm Inspired Approach)

```python
class Solution:
    def largestVariance(self, s: str) -> int:
        # Extract unique characters from the string
        unique_chars = set(s)
        
        # Early return if fewer than 2 unique characters
        if len(unique_chars) < 2:
            return 0

        # Kadane-like function to calculate variance between two characters
        def kadane(a: str, b: str) -> int:
            ans = 0
            countA = 0
            countB = 0
            canExtendPrevB = False

            for c in s:
                if c != a and c != b:
                    continue
                if c == a:
                    countA += 1
                else:
                    countB += 1
                if countB > 0:
                    ans = max(ans, countA - countB)
                elif countB == 0 and canExtendPrevB:
                    ans = max(ans, countA - 1)
                if countB > countA:
                    countA = 0
                    countB = 0
                    canExtendPrevB = True

            return ans

        # Check variance for all pairs of distinct characters
        return max(
            kadane(a, b)
            for a in unique_chars
            for b in unique_chars
            if a != b
        )
```

### Explanation:

1. **Unique Character Extraction:**
   - `unique_chars = set(s)` extracts all distinct characters to limit unnecessary comparisons.

2. **Early Termination:**
   - If fewer than two unique characters exist, return 0 since variance requires at least two characters.

3. **Kadane-like Function:**
   - Iterates through the string, tracking counts of two chosen characters `a` and `b`.
   - Resets counts if the count of `b` exceeds `a` to allow new variance calculations.
   - `canExtendPrevB` handles edge cases where previous `b` occurrences could extend the variance.

4. **Final Computation:**
   - Computes variance for every unique pair `(a, b)` and returns the maximum result.

### Time Complexity:
- **O(26² * n)** → Iterates through all pairs of distinct characters and scans the string.

### Space Complexity:
- **O(1)** → Constant space for tracking counts (ignores input size).

---

## Interview Tips

- Explain how the solution efficiently checks all character pairs using a **Kadane's algorithm-inspired approach**.
- Highlight the role of **resetting counts** to handle negative balances and maximize variance.
- Discuss why handling **edge cases** with `canExtendPrevB` is critical.
- Emphasize the difference between this optimized solution and brute-force approaches.

