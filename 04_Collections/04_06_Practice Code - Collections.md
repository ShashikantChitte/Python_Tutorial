# Practice Code – Collections (Lists, Tuples, Sets & Dictionaries)

> [← Course index](../README.md)

Problems grouped by collection type: lists first, then tuples, sets and dictionaries. Each problem is independent and runnable.

**23 problems** · 12 Easy · 10 Medium · 1 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Remove duplicates from a list while preserving order](#1-remove-duplicates-from-a-list-while-preserving-order) | Easy |
| 2 | [Find the missing number in a sequence of 1..n](#2-find-the-missing-number-in-a-sequence-of-1n) | Easy |
| 3 | [Find the second largest element in a list](#3-find-the-second-largest-element-in-a-list) | Medium |
| 4 | [Find the two numbers that sum to a target (Two Sum)](#4-find-the-two-numbers-that-sum-to-a-target-two-sum) | Medium |
| 5 | [Rotate a list by k positions](#5-rotate-a-list-by-k-positions) | Medium |
| 6 | [Merge two sorted lists](#6-merge-two-sorted-lists) | Medium |
| 7 | [Move all zeros to the end of a list](#7-move-all-zeros-to-the-end-of-a-list) | Medium |
| 8 | [Flatten a nested list](#8-flatten-a-nested-list) | Hard |
| 9 | [Unpack a tuple using the star operator](#9-unpack-a-tuple-using-the-star-operator) | Easy |
| 10 | [Return multiple values from a function](#10-return-multiple-values-from-a-function) | Easy |
| 11 | [Use tuples as dictionary keys](#11-use-tuples-as-dictionary-keys) | Easy |
| 12 | [Show that tuples are immutable](#12-show-that-tuples-are-immutable) | Easy |
| 13 | [Sort a list of tuples by score, then by name](#13-sort-a-list-of-tuples-by-score-then-by-name) | Medium |
| 14 | [Create a readable record with namedtuple](#14-create-a-readable-record-with-namedtuple) | Medium |
| 15 | [Find the intersection and union of two lists](#15-find-the-intersection-and-union-of-two-lists) | Easy |
| 16 | [Find the items that are in only one of two lists](#16-find-the-items-that-are-in-only-one-of-two-lists) | Easy |
| 17 | [Check whether a list contains duplicates](#17-check-whether-a-list-contains-duplicates) | Easy |
| 18 | [Find the most frequent element](#18-find-the-most-frequent-element) | Easy |
| 19 | [Count word frequency with a dictionary](#19-count-word-frequency-with-a-dictionary) | Easy |
| 20 | [Merge two dictionaries, summing values for shared keys](#20-merge-two-dictionaries-summing-values-for-shared-keys) | Easy |
| 21 | [Invert a dictionary (keys ↔ values)](#21-invert-a-dictionary-keys--values) | Medium |
| 22 | [Group a list of words by anagram](#22-group-a-list-of-words-by-anagram) | Medium |
| 23 | [Sort a list of dictionaries by multiple keys](#23-sort-a-list-of-dictionaries-by-multiple-keys) | Medium |

---

## 1. Remove duplicates from a list while preserving order

**Level:** Easy

**Problem:** Turn `[3, 1, 3, 2, 1]` into `[3, 1, 2]`.

**Explanation:** `dict.fromkeys()` keeps only the first occurrence of each item and remembers insertion order, so converting its keys back to a list removes duplicates in order.

```python
def dedupe(items: list) -> list:
    return list(dict.fromkeys(items))

print(dedupe([3, 1, 3, 2, 1]))  # [3, 1, 2]

# ------------Expected Output---------

# [3, 1, 2]
```

---

## 2. Find the missing number in a sequence of 1..n

**Level:** Easy

**Problem:** One number from 1 to `n` is missing from the list – find it.

**Explanation:** The sum of 1..n is `n(n+1)/2`. Subtracting the sum of the given numbers leaves exactly the missing one.

```python
def missing_number(nums: list[int], n: int) -> int:
    return n * (n + 1) // 2 - sum(nums)

print(missing_number([1, 2, 4, 5], 5))  # 3

# ------------Expected Output---------

# 3
```

---

## 3. Find the second largest element in a list

**Level:** Medium

**Problem:** Return the second largest distinct number in one pass.

**Explanation:** Convert to a set to ignore duplicates, then keep track of the largest and second-largest while looping. If fewer than two distinct values exist, return `None`.

```python
def second_largest(nums: list[int]):
    uniq = set(nums)
    if len(uniq) < 2:
        return None
    first = second = float("-inf")
    for n in uniq:
        if n > first:
            first, second = n, first
        elif n > second:
            second = n
    return second

print(second_largest([10, 20, 4, 45, 99, 99]))  # 45

# ------------Expected Output---------

# 45
```

---

## 4. Find the two numbers that sum to a target (Two Sum)

**Level:** Medium

**Problem:** Return the indices of two numbers that add up to a target.

**Explanation:** Store each number’s index in a dictionary. For every number, check whether `target - number` has already been seen – a single pass, O(n).

```python
def two_sum(nums: list[int], target: int):
    seen = {}
    for i, n in enumerate(nums):
        if target - n in seen:
            return seen[target - n], i
        seen[n] = i
    return None

print(two_sum([2, 7, 11, 15], 9))  # (0, 1)

# ------------Expected Output---------

# (0, 1)
```

---

## 5. Rotate a list by k positions

**Level:** Medium

**Problem:** Rotate `[1, 2, 3, 4, 5]` to the right by 2 positions.

**Explanation:** Use `k % len(nums)` to handle large `k`, then join the last `k` items with the rest using slicing.

```python
def rotate_right(nums: list, k: int) -> list:
    if not nums:
        return nums
    k %= len(nums)
    return nums[-k:] + nums[:-k] if k else nums[:]

print(rotate_right([1, 2, 3, 4, 5], 2))  # [4, 5, 1, 2, 3]

# ------------Expected Output---------

# [4, 5, 1, 2, 3]
```

---

## 6. Merge two sorted lists

**Level:** Medium

**Problem:** Merge two already sorted lists into one sorted list without re-sorting.

**Explanation:** Keep one index in each list and always append the smaller current item. When one list runs out, append the remainder of the other.

```python
def merge_sorted(a: list, b: list) -> list:
    i = j = 0
    out = []
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            out.append(a[i]); i += 1
        else:
            out.append(b[j]); j += 1
    return out + a[i:] + b[j:]

print(merge_sorted([1, 3, 5], [2, 4, 6]))

# ------------Expected Output---------

# [1, 2, 3, 4, 5, 6]
```

---

## 7. Move all zeros to the end of a list

**Level:** Medium

**Problem:** Move all zeros to the end while keeping the order of the other items.

**Explanation:** Copy each non-zero item forward to the next write position, then fill the remaining positions with zeros – done in place.

```python
def move_zeros(nums: list[int]) -> list[int]:
    write = 0
    for n in nums:
        if n != 0:
            nums[write] = n
            write += 1
    for i in range(write, len(nums)):
        nums[i] = 0
    return nums

print(move_zeros([0, 1, 0, 3, 12]))  # [1, 3, 12, 0, 0]

# ------------Expected Output---------

# [1, 3, 12, 0, 0]
```

---

## 8. Flatten a nested list

**Level:** Hard

**Problem:** Flatten `[1, [2, [3, [4]], 5]]` to `[1, 2, 3, 4, 5]`.

**Explanation:** A recursive generator yields plain items directly and uses `yield from` to delegate to itself for nested lists or tuples.

```python
def flatten(lst):
    for item in lst:
        if isinstance(item, (list, tuple)):
            yield from flatten(item)
        else:
            yield item

print(list(flatten([1, [2, [3, [4]], 5]])))  # [1, 2, 3, 4, 5]

# ------------Expected Output---------

# [1, 2, 3, 4, 5]
```

---

## 9. Unpack a tuple using the star operator

**Level:** Easy

**Problem:** Take the first two values of a record and collect the rest.

**Explanation:** Tuple unpacking assigns values by position, and `*details` collects every remaining value into a list.

```python
record = ("Ann", 30, "Pune", "QA Engineer", "Python")

name, age, *details = record
print(name, age)
print(details)

# ------------Expected Output---------

# Ann 30
# ['Pune', 'QA Engineer', 'Python']
```

---

## 10. Return multiple values from a function

**Level:** Easy

**Problem:** Return the minimum, maximum and average of a list in one call.

**Explanation:** Python functions can return several values separated by commas; they are packed into a tuple and can be unpacked straight into variables.

```python
def stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

low, high, average = stats([4, 8, 15, 16, 23, 42])
print(low, high, average)
print(stats([1, 2, 3]))

# ------------Expected Output---------

# 4 42 18.0
# (1, 3, 2.0)
```

---

## 11. Use tuples as dictionary keys

**Level:** Easy

**Problem:** Store values against grid coordinates.

**Explanation:** Tuples are hashable (as long as they contain hashable items), so they can be dictionary keys or set members – lists cannot.

```python
grid = {(0, 0): "start", (2, 3): "treasure"}

print(grid[(2, 3)])
print((1, 1) in grid)
print(grid.get((5, 5), "empty"))

# ------------Expected Output---------

# treasure
# False
# empty
```

---

## 12. Show that tuples are immutable

**Level:** Easy

**Problem:** Try to change a tuple element and show how to get an updated tuple anyway.

**Explanation:** Item assignment raises `TypeError`. To "change" a tuple, convert it to a list, modify the list and convert it back – you end up with a new tuple.

```python
point = (3, 4)

try:
    point[0] = 10
except TypeError as error:
    print("Error:", error)

as_list = list(point)
as_list[0] = 10
point = tuple(as_list)
print(point)

# ------------Expected Output---------

# Error: 'tuple' object does not support item assignment
# (10, 4)
```

---

## 13. Sort a list of tuples by score, then by name

**Level:** Medium

**Problem:** Sort students by marks (highest first) and alphabetically for equal marks.

**Explanation:** The sort key returns a tuple `(-marks, name)`. Negating the marks sorts them in descending order while names stay ascending.

```python
students = [("Ann", 82), ("Bob", 95), ("Cat", 82), ("Dan", 70)]

result = sorted(students, key=lambda s: (-s[1], s[0]))
print(result)

# ------------Expected Output---------

# [('Bob', 95), ('Ann', 82), ('Cat', 82), ('Dan', 70)]
```

---

## 14. Create a readable record with namedtuple

**Level:** Medium

**Problem:** Access fields by name instead of by index.

**Explanation:** `namedtuple` creates a tuple subclass with named fields, so the code reads like an object while remaining immutable and lightweight.

```python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])
p = Point(3, 4)

print(p)
print(p.x, p.y)
print(p._asdict())

# ------------Expected Output---------

# Point(x=3, y=4)
# 3 4
# {'x': 3, 'y': 4}
```

---

## 15. Find the intersection and union of two lists

**Level:** Easy

**Problem:** Find which items two lists share and all distinct items across both.

**Explanation:** Convert the lists to sets and use `&` for intersection and `|` for union. Sorting makes the output order predictable.

```python
def intersection_union(a: list, b: list):
    return sorted(set(a) & set(b)), sorted(set(a) | set(b))

print(intersection_union([1, 2, 3, 4], [3, 4, 5]))

# ------------Expected Output---------

# ([3, 4], [1, 2, 3, 4, 5])
```

---

## 16. Find the items that are in only one of two lists

**Level:** Easy

**Problem:** Find items that appear in exactly one of the two lists.

**Explanation:** The symmetric difference operator `^` returns items that are in either set but not in both.

```python
a = [1, 2, 3, 4]
b = [3, 4, 5, 6]

print(sorted(set(a) ^ set(b)))

# ------------Expected Output---------

# [1, 2, 5, 6]
```

---

## 17. Check whether a list contains duplicates

**Level:** Easy

**Problem:** Return `True` if any item appears more than once.

**Explanation:** A set removes duplicates, so if the set is shorter than the list, at least one item was repeated.

```python
def has_duplicates(items):
    return len(items) != len(set(items))

print(has_duplicates([1, 2, 3, 4]))
print(has_duplicates([1, 2, 3, 2]))

# ------------Expected Output---------

# False
# True
```

---

## 18. Find the most frequent element

**Level:** Easy

**Problem:** Return the element that appears most often in a list.

**Explanation:** `Counter(items).most_common(1)` returns the top element with its count as a list of one tuple; `[0][0]` extracts the element.

```python
from collections import Counter

def most_frequent(items: list):
    return Counter(items).most_common(1)[0][0]

print(most_frequent([1, 3, 3, 2, 3, 1]))  # 3

# ------------Expected Output---------

# 3
```

---

## 19. Count word frequency with a dictionary

**Level:** Easy

**Problem:** Count how many times each word appears in a sentence.

**Explanation:** `freq.get(word, 0) + 1` reads the current count (defaulting to 0) and adds one, so no key-existence check is needed.

```python
text = "to be or not to be that is the question to be"

freq = {}
for word in text.split():
    freq[word] = freq.get(word, 0) + 1

print(freq)

# ------------Expected Output---------

# {'to': 3, 'be': 3, 'or': 1, 'not': 1, 'that': 1, 'is': 1, 'the': 1, 'question': 1}
```

---

## 20. Merge two dictionaries, summing values for shared keys

**Level:** Easy

**Problem:** Merge `{"x": 1, "y": 2}` and `{"y": 3, "z": 4}` so that `y` becomes 5.

**Explanation:** Adding two `Counter` objects adds the values of matching keys and keeps the others.

```python
from collections import Counter

def merge_sum(a: dict, b: dict) -> dict:
    return dict(Counter(a) + Counter(b))

print(merge_sum({"x": 1, "y": 2}, {"y": 3, "z": 4}))  # {'x': 1, 'y': 5, 'z': 4}

# ------------Expected Output---------

# {'x': 1, 'y': 5, 'z': 4}
```

---

## 21. Invert a dictionary (keys ↔ values)

**Level:** Medium

**Problem:** Swap keys and values even when several keys share the same value.

**Explanation:** Use `defaultdict(list)` so each value maps to a list of the keys that had it – nothing is overwritten.

```python
from collections import defaultdict

def invert(d: dict) -> dict:
    out = defaultdict(list)
    for k, v in d.items():
        out[v].append(k)
    return dict(out)  # handles duplicate values safely

print(invert({"a": 1, "b": 2, "c": 1}))  # {1: ['a', 'c'], 2: ['b']}

# ------------Expected Output---------

# {1: ['a', 'c'], 2: ['b']}
```

---

## 22. Group a list of words by anagram

**Level:** Medium

**Problem:** Group words that are anagrams of each other.

**Explanation:** Words that are anagrams have the same letters when sorted, so the sorted letters make a perfect dictionary key.

```python
from collections import defaultdict

def group_anagrams(words: list[str]) -> list[list[str]]:
    groups = defaultdict(list)
    for w in words:
        groups["".join(sorted(w))].append(w)
    return list(groups.values())

print(group_anagrams(["eat", "tea", "tan", "ate", "nat", "bat"]))

# ------------Expected Output---------

# [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
```

---

## 23. Sort a list of dictionaries by multiple keys

**Level:** Medium

**Problem:** Sort people by age (descending) and then by name (ascending).

**Explanation:** `sorted()` with a key function returning `(-age, name)` handles both orders in one pass.

```python
people = [
    {"name": "Ann", "age": 30, "city": "Pune"},
    {"name": "Bob", "age": 25, "city": "Mumbai"},
    {"name": "Cat", "age": 30, "city": "Delhi"},
]
# age descending, then name ascending
result = sorted(people, key=lambda p: (-p["age"], p["name"]))
print([p["name"] for p in result])  # ['Ann', 'Cat', 'Bob']

# ------------Expected Output---------

# ['Ann', 'Cat', 'Bob']
```
