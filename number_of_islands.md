# Number of Islands: Quick Reference

## Problem Description

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are surrounded by water.

### Example 1:

```plaintext
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

### Example 2:

```plaintext
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

### Constraints:

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

---

## Code Implementation

### Python Solution:
```python
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        visited = set()

        def dfs(r, c):
            if (
                r < 0 or c < 0 or
                r >= rows or c >= cols or
                grid[r][c] == "0" or
                (r, c) in visited
            ):
                return

            visited.add((r, c))

            # Explore neighbors
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        island_count = 0

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r, c) not in visited:
                    dfs(r, c)
                    island_count += 1

        return island_count
```

### Time Complexity:
- **O(m × n)**: Each cell is visited once.

### Space Complexity:
- **O(m × n)**: In the worst case, the recursion stack or the visited set grows to the size of the grid.

---

## Alternative Approaches

### Breadth-First Search (BFS)
Instead of depth-first search, use a queue to iteratively explore neighbors.

#### Code:
```python
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        visited = set()

        def bfs(r, c):
            queue = deque([(r, c)])
            while queue:
                row, col = queue.popleft()
                for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                    nr, nc = row + dr, col + dc
                    if (
                        0 <= nr < rows and 0 <= nc < cols and
                        grid[nr][nc] == "1" and (nr, nc) not in visited
                    ):
                        visited.add((nr, nc))
                        queue.append((nr, nc))

        island_count = 0

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r, c) not in visited:
                    visited.add((r, c))
                    bfs(r, c)
                    island_count += 1

        return island_count
```

#### Time Complexity:
- **O(m × n)**: Each cell is visited once.

#### Space Complexity:
- **O(min(m, n))**: The queue size is proportional to the smaller of the grid dimensions in the worst case.

---

## Interview Tips
- Clearly explain the concept of connected components and how DFS or BFS explores them.
- Discuss edge cases, such as an empty grid, a grid with all water (`0`s), or a grid with all land (`1`s).
- Be prepared to compare DFS and BFS approaches, highlighting their trade-offs.
- Emphasize the use of a `visited` set to avoid revisiting cells and causing infinite loops.

