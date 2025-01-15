# Course Schedule: Quick Reference

## Problem Description

There are a total of `numCourses` courses labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

Return `True` if it is possible to finish all courses. Otherwise, return `False`.

### Example 1:

```plaintext
Input: numCourses = 2, prerequisites = [[1, 0]]
Output: true
Explanation: You need to take course 0 before course 1. So, it is possible.
```

### Example 2:

```plaintext
Input: numCourses = 2, prerequisites = [[1, 0], [0, 1]]
Output: false
Explanation: There is a cycle between courses 0 and 1, making it impossible to finish all courses.
```

### Constraints:

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs `prerequisites[i]` are unique.

---

## Code Implementation

### Python Solution (Using Topological Sort - Kahn's Algorithm):
```python
from collections import defaultdict, deque
from typing import List

class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # Build the adjacency list and in-degree counter
        graph = defaultdict(list)
        in_degree = [0] * numCourses

        for dest, src in prerequisites:
            graph[src].append(dest)
            in_degree[dest] += 1

        # Initialize the queue with courses having no prerequisites
        queue = deque([i for i in range(numCourses) if in_degree[i] == 0])

        taken_courses = 0

        while queue:
            course = queue.popleft()
            taken_courses += 1

            for neighbor in graph[course]:
                in_degree[neighbor] -= 1
                if in_degree[neighbor] == 0:
                    queue.append(neighbor)

        return taken_courses == numCourses
```

### Time Complexity:
- **O(V + E)**: `V` is the number of courses, and `E` is the number of prerequisite pairs.

### Space Complexity:
- **O(V + E)**: Space for the adjacency list and in-degree array.

---

## Alternative Approaches

### Depth-First Search (DFS) for Cycle Detection
Use DFS to detect cycles in the graph. If a cycle is detected, it's impossible to finish all courses.

#### Code:
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = defaultdict(list)

        for dest, src in prerequisites:
            graph[src].append(dest)

        visited = [0] * numCourses  # 0 = unvisited, 1 = visiting, 2 = visited

        def dfs(course):
            if visited[course] == 1:  # Cycle detected
                return False
            if visited[course] == 2:
                return True

            visited[course] = 1
            for neighbor in graph[course]:
                if not dfs(neighbor):
                    return False

            visited[course] = 2
            return True

        for course in range(numCourses):
            if not dfs(course):
                return False

        return True
```

#### Time Complexity:
- **O(V + E)**: Visits every course and prerequisite pair.

#### Space Complexity:
- **O(V + E)**: Space for the adjacency list and recursion stack.

---

## Interview Tips

- Explain how course dependencies can be modeled as a **directed graph**.
- Clarify the difference between using **BFS (Topological Sort)** and **DFS (Cycle Detection)** approaches.
- Discuss how cycles in the graph make it impossible to complete all courses.
- Highlight edge cases like:
  - No prerequisites (`prerequisites = []` → return `True`)
  - Self-dependency (`[0, 0]` → return `False`)
- Emphasize the importance of efficient traversal in large graphs.

