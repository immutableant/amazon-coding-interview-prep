# Maximum Subarray: Quick Reference

## Problem Description

Given an integer array `nums`, find the **subarray** with the largest sum and return its sum.

### Example 1:

```plaintext
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```

### Example 2:

```plaintext
Input: nums = [1]
Output: 1
Explanation: The subarray [1] has the largest sum 1.
```

### Example 3:

```plaintext
Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.
```

### Constraints:

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

**Follow-up:** Implement a solution using the **Divide and Conquer** approach.

---

## Code Implementation

### Python Solution (Kadane's Algorithm - O(n)):

```python
from typing import List

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = current_sum = nums[0]

        for num in nums[1:]:
            current_sum = max(num, current_sum + num)
            max_sum = max(max_sum, current_sum)
        
        return max_sum
```

### Time Complexity:
- **O(n)**: Iterates through the array once.

### Space Complexity:
- **O(1)**: No extra space is used beyond variables.

---

## Alternative Approaches

### Divide and Conquer (O(n log n))

Split the array into halves, recursively solve for left and right halves, and find the maximum sum crossing the middle.

#### Code:
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        def helper(left, right):
            if left == right:
                return nums[left]

            mid = (left + right) // 2

            left_max = helper(left, mid)
            right_max = helper(mid + 1, right)
            cross_max = self.crossSum(nums, left, mid, right)

            return max(left_max, right_max, cross_max)

        return helper(0, len(nums) - 1)

    def crossSum(self, nums, left, mid, right):
        left_sum = float('-inf')
        curr_sum = 0
        for i in range(mid, left - 1, -1):
            curr_sum += nums[i]
            left_sum = max(left_sum, curr_sum)

        right_sum = float('-inf')
        curr_sum = 0
        for i in range(mid + 1, right + 1):
            curr_sum += nums[i]
            right_sum = max(right_sum, curr_sum)

        return left_sum + right_sum
```

#### Time Complexity:
- **O(n log n)**: Due to recursive splitting and combining.

#### Space Complexity:
- **O(log n)**: Space for the recursion stack.

---

## Interview Tips

- Start with **Kadane's Algorithm** as it's optimal with **O(n)** complexity.
- If asked for a more advanced solution, explain the **Divide and Conquer** strategy.
- Emphasize how Kadane's Algorithm decides to **extend** or **restart** the subarray at each step.
- Discuss edge cases:
  - All negative numbers → pick the least negative number.
  - Single-element arrays → return the only element.
- Be prepared to explain the trade-offs between **time** and **space complexity** in both approaches.

