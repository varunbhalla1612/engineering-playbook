# Two Sum (LeetCode #1)

## Pattern

**Hash Map (Complement Lookup)**

## Recognition Triggers

* Find a pair of numbers
* Target sum (`a + b = target`)
* Need an `O(n)` solution
* Return indices

## Core Insight

For each number, calculate the complement (`target - current`). If the complement has already been seen, return the two indices. Otherwise, store the current number and its index in a hash map.

## Generic Template

```python
hashmap = {}

for i, value in enumerate(nums):
    complement = target - value

    if complement in hashmap:
        return [hashmap[complement], i]

    hashmap[value] = i
```

## Complexity

* **Time:** `O(n)`
* **Space:** `O(n)`

## Common Mistakes

* Using nested loops (`O(n²)`)
* Storing the value instead of its index
* Storing before checking (can cause self-match issues in some variants)
* Not considering duplicate values

## Related Problems

* Two Sum II
* Three Sum
* Four Sum
* Contains Duplicate
* Subarray Sum Equals K

## 30-Second Interview Explanation

Use a hash map to store previously seen numbers and their indices. For each element, compute its complement (`target - current`). If the complement already exists in the map, return the indices; otherwise, store the current element and continue. This achieves `O(n)` time with `O(n)` extra space.
