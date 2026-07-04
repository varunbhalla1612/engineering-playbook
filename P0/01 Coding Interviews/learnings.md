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



# Valid Anagram (LeetCode #242)

## Pattern
Frequency Counting (Hash Map)

## Recognition Triggers
- Compare two strings
- Same characters
- Same frequencies
- Order does NOT matter
- Rearrangement / permutation problems

## Core Insight
An anagram is determined by **character frequency**, not character order.

Increase the count for each character in `s` and decrease the count for each character in `t`. If every character's final count is `0`, the strings are anagrams.

## Thinking Process
1. If lengths differ → immediately return `False`.
2. Count occurrences of characters in `s`.
3. Remove occurrences using `t`.
4. Every frequency should end at `0`.

## Generic Template

```python
freq = {}

for each character in s:
    freq[ch] += 1

for each character in t:
    freq[ch] -= 1

return all(count == 0 for count in freq.values())
```

## Complexity
- Time: O(n)
- Space: O(k), where k = number of unique characters
  (O(1) for lowercase English letters)

## Common Mistakes
- Checking if a character exists instead of counting frequencies.
- Forgetting the length check.
- Comparing order instead of counts.
- Forgetting to verify all final counts are zero.

## Related Problems
- Group Anagrams
- Find All Anagrams in a String
- Ransom Note
- Top K Frequent Elements
- First Unique Character in a String

## Pattern Memory
Hash Map = **Frequency Counter**

Whenever a problem asks:
- "same characters"
- "same elements"
- "same occurrences"

Think:
> Count frequencies and compare them.

## 30-Second Interview Explanation
Since order doesn't matter, I compare character frequencies instead of positions. I increment counts for characters in the first string and decrement them for the second. If every final count is zero, both strings contain exactly the same characters with the same frequencies.
