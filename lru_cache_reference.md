# LRU Cache: Quick Reference

## Problem Description

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the `LRUCache` class:

- `LRUCache(int capacity)`: Initialize the LRU cache with positive size `capacity`.
- `int get(int key)`: Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)`: Update the value of the `key` if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity, evict the least recently used key.

**Constraints**:
- 1 <= capacity <= 3000
- 0 <= key <= 10^4
- 0 <= value <= 10^5
- At most 2 * 10^5 calls will be made to `get` and `put`.

**Example Usage**:
```plaintext
Input:
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

Output:
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation:
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

---

## Implementation with `OrderedDict`

### Code:
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key in self.cache:
            # Move the accessed item to the end to mark it as recently used
            self.cache.move_to_end(key)
            return self.cache[key]
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            # Update the key's value and mark it as recently used
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            # Pop the first item (the least recently used item)
            self.cache.popitem(last=False)
```

### Time Complexity:
- **`get`:** O(1) (amortized)
- **`put`:** O(1) (amortized)

### Space Complexity:
- O(capacity) for storing the cache.

---

## Custom Implementation (Without `OrderedDict`)

### Explanation:
If `OrderedDict` is unavailable, we can use a hash map and a doubly linked list to manage the cache manually. The hash map provides O(1) access, while the doubly linked list keeps track of the order of usage.

### Code:
```python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}  # Maps key -> Node
        self.head = Node(0, 0)  # Dummy head
        self.tail = Node(0, 0)  # Dummy tail
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node: Node):
        """Remove a node from the doubly linked list."""
        prev_node = node.prev
        next_node = node.next
        prev_node.next = next_node
        next_node.prev = prev_node

    def _add_to_end(self, node: Node):
        """Add a node to the end of the doubly linked list."""
        prev_node = self.tail.prev
        prev_node.next = node
        node.prev = prev_node
        node.next = self.tail
        self.tail.prev = node

    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            # Move the accessed node to the end
            self._remove(node)
            self._add_to_end(node)
            return node.value
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            # Update the value and move the node to the end
            node = self.cache[key]
            node.value = value
            self._remove(node)
            self._add_to_end(node)
        else:
            if len(self.cache) >= self.capacity:
                # Remove the least recently used item
                lru_node = self.head.next
                self._remove(lru_node)
                del self.cache[lru_node.key]
            # Add the new node
            new_node = Node(key, value)
            self.cache[key] = new_node
            self._add_to_end(new_node)
```

### Time Complexity:
- **`get`:** O(1)
- **`put`:** O(1)

### Space Complexity:
- O(capacity) for hash map and doubly linked list.

---

## Interview Tips
- Be prepared to explain both implementations.
- Emphasize the importance of O(1) operations and how `OrderedDict` or the combination of a hash map and doubly linked list achieves this.
- If asked about trade-offs, mention that the `OrderedDict` approach is simpler and less error-prone, while the custom implementation offers more control and flexibility at the cost of complexity.

