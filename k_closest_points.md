# K Closest Points to Origin: Quick Reference

## Problem Description

Given an array of points where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points `(x1, y1)` and `(x2, y2)` is the **Euclidean distance**:

```
√((x1 - x2)^2 + (y1 - y2)^2)
```

You may return the answer in **any order**, and the answer is guaranteed to be **unique** (except for the order).

### Example 1:

```plaintext
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
- Distance from (1,3) to origin: √(1² + 3²) = √10 ≈ 3.16
- Distance from (-2,2) to origin: √((-2)² + 2²) = √8 ≈ 2.83
- Closest point: [-2,2]
```

### Example 2:

```plaintext
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: [[-2,4],[3,3]] is also valid.
```

### Constraints:

- `1 <= k <= points.length <= 10^4`
- `-10^4 <= xi, yi <= 10^4`

---

## Code Implementation

### Python Solution (Using Min-Heap):

```python
from typing import List
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        # Min-heap to store (distance, point)
        heap = []

        for x, y in points:
            distance = x ** 2 + y ** 2  # Avoid sqrt for efficiency
            heapq.heappush(heap, (distance, [x, y]))
        
        # Extract k closest points
        result = [heapq.heappop(heap)[1] for _ in range(k)]
        
        return result
```

### Time Complexity:
- **O(n log n)**: Inserting all points into the heap takes O(log n) time each.

### Space Complexity:
- **O(n)**: Space used by the heap.

---

## Alternative Approaches

### Max-Heap (Optimized for Large `n`)
Keep only the `k` closest points in the heap.

#### Code:
```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        # Max-heap to keep track of k closest points
        heap = []
        
        for x, y in points:
            distance = -(x ** 2 + y ** 2)  # Negate for max-heap behavior
            heapq.heappush(heap, (distance, [x, y]))
            if len(heap) > k:
                heapq.heappop(heap)
        
        return [point for (_, point) in heap]
```

#### Time Complexity:
- **O(n log k)**: More efficient for large `n` when `k` is much smaller than `n`.

#### Space Complexity:
- **O(k)**: Space for storing only `k` points.

---

## Interview Tips

- Explain why using a **heap** optimizes the solution for this problem.
- Discuss how the **Euclidean distance** simplifies to `x² + y²` when comparing relative distances (no need for `sqrt`).
- Emphasize edge cases:
  - `k = points.length` → return all points.
  - Multiple points at the same distance.
- Compare the trade-offs between the **min-heap** and **max-heap** approaches, especially for large datasets.

