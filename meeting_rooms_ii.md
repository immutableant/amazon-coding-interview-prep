# Meeting Rooms II: Quick Reference

## Problem Description

Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return the **minimum number of conference rooms** required.

### Example 1:

```plaintext
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
Explanation:
- Meeting 1: [0,30]
- Meeting 2: [5,10]
- Meeting 3: [15,20]

Meetings 1 and 2 overlap, so at least two rooms are needed.
```

### Example 2:

```plaintext
Input: intervals = [[7,10],[2,4]]
Output: 1
Explanation:
- Meeting 1: [7,10]
- Meeting 2: [2,4]

Since these meetings do not overlap, only one room is required.
```

### Constraints:

- `1 <= intervals.length <= 10^4`
- `0 <= starti < endi <= 10^6`

---

## Code Implementation

### Python Solution (Using Min-Heap):

```python
from typing import List
import heapq

class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0

        # Sort meetings by start time
        intervals.sort(key=lambda x: x[0])
        
        # Min-heap to track end times of meetings
        min_heap = []
        
        # Add the first meeting's end time
        heapq.heappush(min_heap, intervals[0][1])
        
        for meeting in intervals[1:]:
            # If the earliest meeting has ended, free up a room
            if meeting[0] >= min_heap[0]:
                heapq.heappop(min_heap)
            
            # Allocate a room for the current meeting
            heapq.heappush(min_heap, meeting[1])
        
        # The heap size is the number of rooms needed
        return len(min_heap)
```

### Time Complexity:
- **O(n log n)**: Sorting the intervals takes O(n log n), and the heap operations take O(log n) for each interval.

### Space Complexity:
- **O(n)**: Space for the heap to store end times.

---

## Alternative Approaches

### Two Pointers (Start and End Times)

#### Code:
```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0
        
        # Separate and sort start and end times
        start_times = sorted([i[0] for i in intervals])
        end_times = sorted([i[1] for i in intervals])
        
        start_ptr = end_ptr = 0
        used_rooms = 0
        
        while start_ptr < len(intervals):
            # A meeting has ended, free up a room
            if start_times[start_ptr] >= end_times[end_ptr]:
                used_rooms -= 1
                end_ptr += 1
            
            # Allocate a new room
            used_rooms += 1
            start_ptr += 1
        
        return used_rooms
```

#### Time Complexity:
- **O(n log n)**: Due to sorting of start and end times.

#### Space Complexity:
- **O(n)**: For the sorted start and end arrays.

---

## Interview Tips

- Explain why sorting the start and end times helps in efficiently tracking room usage.
- Emphasize the efficiency of using a **min-heap** to track the earliest meeting end time.
- Discuss edge cases:
  - Single meeting → return `1`
  - Fully overlapping meetings → need as many rooms as meetings.
- Compare **Min-Heap** and **Two-Pointer** approaches in terms of implementation simplicity and performance.

