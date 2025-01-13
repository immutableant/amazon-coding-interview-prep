# Trapping Rain Water: Quick Reference

## Problem Description

Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

### Example 1:

```plaintext
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

### Example 2:

```plaintext
Input: height = [4,2,0,3,2,5]
Output: 9
```

### Constraints:

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

---

## Code Implementation

### Python Solution:
```python
from typing import List

def trap(self, height: List[int]) -> int:
    if not height:
        return 0

    n = len(height)
    left_max = [0] * n
    right_max = [0] * n

    # Fill left_max array
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i - 1], height[i])

    # Fill right_max array
    right_max[n - 1] = height[n - 1]
    for i in range(n - 2, -1, -1):
        right_max[i] = max(right_max[i + 1], height[i])

    # Calculate trapped water
    water_trapped = 0
    for i in range(n):
        water_trapped += min(left_max[i], right_max[i]) - height[i]

    return water_trapped
```

### Time Complexity:
- **O(n)**: Three passes are made over the `height` array (for `left_max`, `right_max`, and trapped water calculation).

### Space Complexity:
- **O(n)**: Additional space is used for the `left_max` and `right_max` arrays.

---

## Alternative Approaches

### Two-Pointer Technique
Instead of using `left_max` and `right_max` arrays, we can use two pointers and calculate water trapped in a single pass.

#### Code:
```python
def trap(self, height: List[int]) -> int:
    if not height:
        return 0

    left, right = 0, len(height) - 1
    left_max, right_max = 0, 0
    water_trapped = 0

    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water_trapped += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water_trapped += right_max - height[right]
            right -= 1

    return water_trapped
```

#### Time Complexity:
- **O(n)**: Single pass using two pointers.

#### Space Complexity:
- **O(1)**: No extra space is used.

---

## Interview Tips
- Clearly explain the problem and constraints before jumping into the solution.
- Start with the straightforward `left_max` and `right_max` approach, as it is easier to explain and implement.
- Mention the two-pointer optimization if time permits.
- Emphasize edge cases, such as empty input or flat surfaces with no trapped water.
- Highlight the importance of reducing space complexity in large-scale problems.

