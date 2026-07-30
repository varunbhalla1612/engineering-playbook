# Heap in Python

## Pattern

Priority Queue + Complete Binary Tree

## What Is a Heap?

A heap is a **complete binary tree** that follows a specific ordering rule.

### Min-Heap

Every parent is smaller than or equal to its children.

```text
        1
       / \
      3   2
     / \
    7   6
```

The smallest element is always at the root.

### Max-Heap

Every parent is greater than or equal to its children.

```text
        9
       / \
      7   8
     / \
    2   4
```

The largest element is always at the root.

Python’s `heapq` module implements a **min-heap** by default.

---

## Complete Binary Tree

A complete binary tree has:

* Every level fully filled except possibly the last.
* The last level filled from left to right.
* At most two children per node.

This structure allows a heap to be stored efficiently in a Python list.

---

## List Representation

```python
heap = [1, 3, 2, 7, 6]
```

Tree representation:

```text
        1          index 0
       / \
      3   2        index 1, 2
     / \
    7   6          index 3, 4
```

For a node at index `i`:

```python
parent = (i - 1) // 2
left_child = 2 * i + 1
right_child = 2 * i + 2
```

Example:

```python
heap = [1, 3, 2, 7, 6]

# Node at index 1
parent = (1 - 1) // 2       # 0
left = 2 * 1 + 1            # 3
right = 2 * 1 + 2           # 4
```

Therefore, the node `3` has:

```text
Parent: 1
Left child: 7
Right child: 6
```

---

## Important Property

A heap is **not a sorted list**.

```python
heap = [1, 3, 2, 7, 6, 5]
```

This is a valid min-heap because every parent is smaller than its children.

The heap only guarantees:

```text
parent <= children
```

It does not guarantee:

```text
heap[0] <= heap[1] <= heap[2] ...
```

---

## Insert: Sift Up

To insert a value:

1. Add it to the end of the list.
2. Compare it with its parent.
3. Swap while it is smaller than its parent.
4. Stop when the heap property is restored.

Example: insert `1`.

```text
Initial heap:

        3
       / \
      5   8

List: [3, 5, 8]
```

Append `1`:

```text
        3
       / \
      5   8
     /
    1

List: [3, 5, 8, 1]
```

Swap `1` with `5`:

```python
[3, 1, 8, 5]
```

Swap `1` with `3`:

```python
[1, 3, 8, 5]
```

### Implementation From Scratch

```python
def heappush(heap: list[int], value: int) -> None:
    heap.append(value)

    current = len(heap) - 1

    while current > 0:
        parent = (current - 1) // 2

        if heap[parent] <= heap[current]:
            break

        heap[parent], heap[current] = heap[current], heap[parent]
        current = parent
```

---

## Remove Minimum: Sift Down

In a min-heap, the minimum value is always:

```python
heap[0]
```

To remove it:

1. Save the root value.
2. Move the last element to the root.
3. Remove the last position.
4. Compare the new root with its children.
5. Swap it with the smaller child.
6. Continue until the heap property is restored.

Example:

```python
heap = [1, 3, 2, 7, 6, 5]
```

Remove `1` and move `5` to the root:

```python
[5, 3, 2, 7, 6]
```

The smaller child is `2`, so swap:

```python
[2, 3, 5, 7, 6]
```

### Implementation From Scratch

```python
def heappop(heap: list[int]) -> int:
    if not heap:
        raise IndexError("Cannot pop from an empty heap")

    minimum = heap[0]
    last_value = heap.pop()

    if not heap:
        return minimum

    heap[0] = last_value
    current = 0

    while True:
        left = 2 * current + 1
        right = 2 * current + 2
        smallest = current

        if left < len(heap) and heap[left] < heap[smallest]:
            smallest = left

        if right < len(heap) and heap[right] < heap[smallest]:
            smallest = right

        if smallest == current:
            break

        heap[current], heap[smallest] = heap[smallest], heap[current]
        current = smallest

    return minimum
```

---

## Python `heapq`

```python
import heapq
```

### Create a Heap

```python
heap = []
```

### Insert

```python
heapq.heappush(heap, 5)
heapq.heappush(heap, 2)
heapq.heappush(heap, 8)
heapq.heappush(heap, 1)

print(heap)
# [1, 2, 8, 5]
```

### View the Minimum

```python
minimum = heap[0]
```

### Remove the Minimum

```python
minimum = heapq.heappop(heap)
```

### Convert a List Into a Heap

```python
numbers = [8, 3, 5, 1, 9, 2]

heapq.heapify(numbers)

print(numbers)
# Valid min-heap
```

### Push and Pop Together

```python
value = heapq.heappushpop(heap, new_value)
```

This pushes the new value and then removes the smallest value.

```python
value = heapq.heapreplace(heap, new_value)
```

This removes the smallest value first and then pushes the new value.

---

## Max-Heap in Python

A common technique is to store negative values.

```python
import heapq

max_heap = []

heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -2)
heapq.heappush(max_heap, -8)

largest = -heapq.heappop(max_heap)

print(largest)
# 8
```

Mental model:

```text
Original values:  5, 2, 8
Stored values:   -5, -2, -8

Smallest negative value = largest original value
```

---

## Heap With Tuples

Python compares tuple values from left to right.

```python
heap = []

heapq.heappush(heap, (2, "Task B"))
heapq.heappush(heap, (1, "Task A"))
heapq.heappush(heap, (3, "Task C"))

priority, task = heapq.heappop(heap)

print(priority, task)
# 1 Task A
```

Useful pattern:

```python
(priority, value)
```

For a max-heap:

```python
(-priority, value)
```

---

## Time Complexity

| Operation              | Complexity |
| ---------------------- | ---------: |
| Get minimum            |     `O(1)` |
| Insert                 | `O(log n)` |
| Remove minimum         | `O(log n)` |
| Heapify entire list    |     `O(n)` |
| Search arbitrary value |     `O(n)` |

The tree height is `O(log n)` because the number of nodes approximately doubles at every level.

```text
Level 0: 1 node
Level 1: 2 nodes
Level 2: 4 nodes
Level 3: 8 nodes
```

After height `h`:

```text
n ≈ 2^h
h ≈ log₂(n)
```

Insert and remove operations move through at most one root-to-leaf path, so they take `O(log n)`.

---

## Recognition Triggers

Consider using a heap when the problem asks for:

* Top `K` largest or smallest elements.
* `K` closest points.
* `K` most frequent elements.
* Continuously finding the minimum or maximum.
* Processing tasks by priority.
* Merging sorted lists.
* Finding the median from a data stream.
* Scheduling jobs or events.

Common phrases:

```text
Top K
Kth largest
Kth smallest
Most frequent
Closest
Highest priority
Minimum cost available
Process in order
```

---

## Common Top-K Pattern

### Find K Largest Elements

Maintain a min-heap of size `k`.

```python
import heapq

def k_largest(nums: list[int], k: int) -> list[int]:
    heap = []

    for num in nums:
        heapq.heappush(heap, num)

        if len(heap) > k:
            heapq.heappop(heap)

    return heap
```

Why a min-heap?

The root contains the smallest element among the current top `k` elements.

When the heap grows beyond size `k`, remove that smallest element.

---

## Common Mistakes

### Assuming the Heap Is Sorted

```python
heap = [1, 3, 2, 8, 7]
```

This is valid even though `3 > 2`.

Only the parent-child relationship is guaranteed.

### Removing `heap[0]` Directly

Avoid:

```python
heap.pop(0)
```

This takes `O(n)` and does not restore the heap property.

Use:

```python
heapq.heappop(heap)
```

### Forgetting to Heapify

This is only a normal list:

```python
nums = [5, 1, 8, 2]
```

Convert it before using heap operations:

```python
heapq.heapify(nums)
```

### Using the Wrong Heap Size for Top K

For top `k` largest elements:

```text
Use a min-heap of size k.
```

For top `k` smallest elements:

```text
Use a max-heap of size k.
```

---

## Core Mental Model

```text
Heap = Complete Binary Tree + Heap Order
```

For a min-heap:

```text
Root contains the minimum.
Parent is smaller than its children.
Insert uses sift up.
Remove uses sift down.
```

Python representation:

```text
Parent:      (i - 1) // 2
Left child:  2 * i + 1
Right child: 2 * i + 2
```

Main operations:

```python
heapq.heapify(nums)
heapq.heappush(heap, value)
heapq.heappop(heap)
heap[0]
```
