# Merge k Sorted Lists: Quick Reference

## Problem Description

You are given an array of `k` linked-lists, where each linked list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

### Example 1:

```plaintext
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1 -> 4 -> 5,
  1 -> 3 -> 4,
  2 -> 6
]
Merging them results in:
1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

### Example 2:

```plaintext
Input: lists = []
Output: []
```

### Example 3:

```plaintext
Input: lists = [[]]
Output: []
```

### Constraints:

- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`
- `lists[i]` is sorted in **ascending order**.
- The total number of nodes in all linked lists is less than `10^4`.

---

## Code Implementation

### Python Solution (Using Min-Heap):

```python
from typing import List, Optional
import heapq

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        min_heap = []
        
        # Initialize the heap with the first node of each list
        for idx, node in enumerate(lists):
            if node:
                heapq.heappush(min_heap, (node.val, idx, node))
        
        dummy = ListNode(0)
        current = dummy
        
        while min_heap:
            val, idx, node = heapq.heappop(min_heap)
            current.next = node
            current = current.next
            if node.next:
                heapq.heappush(min_heap, (node.next.val, idx, node.next))
                
        return dummy.next
```

### Time Complexity:
- **O(N log k)**:  
  - `N` is the total number of nodes across all lists.  
  - `k` is the number of linked lists.  
  - Each insertion/removal from the heap takes O(log k).

### Space Complexity:
- **O(k)**: Space used by the heap to store the head nodes of each list.

---

## Alternative Approaches

### Divide and Conquer
Repeatedly merge pairs of lists until one remains.

#### Code:
```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None
        
        while len(lists) > 1:
            merged_lists = []
            for i in range(0, len(lists), 2):
                l1 = lists[i]
                l2 = lists[i + 1] if (i + 1) < len(lists) else None
                merged_lists.append(self.mergeTwoLists(l1, l2))
            lists = merged_lists
        return lists[0]
    
    def mergeTwoLists(self, l1, l2):
        dummy = ListNode(0)
        current = dummy
        while l1 and l2:
            if l1.val < l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next
        current.next = l1 or l2
        return dummy.next
```

#### Time Complexity:
- **O(N log k)**: Similar to the heap approach but without extra heap overhead.

#### Space Complexity:
- **O(1)**: No extra space beyond pointers (excluding recursion stack).

---

## Interview Tips

- Explain why using a **min-heap** ensures efficient merging by always choosing the smallest current node.
- Discuss the **divide-and-conquer** approach and how it reduces the problem size by half in each step.
- Highlight edge cases:
  - When the input list is empty (`lists = []`).
  - When all lists are empty (`lists = [[]]`).
- Clarify how the heap stores `(node value, index, node)` to avoid comparison errors when node values are equal.
- Be prepared to compare both approaches in terms of performance and space efficiency.

