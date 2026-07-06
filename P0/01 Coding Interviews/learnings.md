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

# Group Anagrams (LeetCode #49)

## Pattern
Hash Map + Frequency Counting

## Recognition Triggers
- Group strings by similarity
- Find strings with identical character counts
- Anagrams
- Lowercase English letters
- Return groups instead of True/False

## Core Insight
All anagrams have the **same character frequency**.

Instead of sorting every string, create a 26-character frequency array and use it as a unique key for each group.

## Thinking Process
1. Create an empty hash map.
2. For each word:
   - Build a 26-length frequency array.
   - Convert it to a tuple (hashable).
   - Use the tuple as the dictionary key.
3. Append the word to its corresponding group.
4. Return all groups.

## Generic Template

```python
groups = defaultdict(list)

for word in words:
    count = [0] * 26

    for c in word:
        count[ord(c) - ord('a')] += 1

    groups[tuple(count)].append(word)

return list(groups.values())
```

## Complexity
- Time: O(n × k)
- Space: O(n × k)

where:
- n = number of strings
- k = maximum string length

## Common Mistakes
- Using a list as a dictionary key (lists are not hashable).
- Converting the frequency array into a plain string without separators, which can create collisions.
- Creating a frequency array of size 27 instead of 26.
- Using list concatenation instead of append().

## Related Problems
- Valid Anagram
- Find All Anagrams in a String
- Ransom Note
- Permutation in String
- Top K Frequent Elements

## Pattern Memory
Group items by a **signature**.

Possible signatures:
- Sorted string → O(k log k)
- Frequency tuple → O(k)

When the alphabet size is fixed (26 lowercase letters), frequency counting is usually the optimal signature.

## 30-Second Interview Explanation
Anagrams are determined by character frequencies, not character order. For each word, I build a 26-element frequency array representing the count of each letter. I convert this array into a tuple so it can be used as a hash map key, then group all words with the same frequency signature. This avoids sorting each string and runs in O(n × k) time.

- Dictionary keys must be hashable (tuple works, list doesn't).
- ord(c) - ord('a') maps letters to indices 0–25.
- Frequency arrays are a reusable pattern for character-count problems.
- Grouping problems often reduce to creating a unique signature for each item.
