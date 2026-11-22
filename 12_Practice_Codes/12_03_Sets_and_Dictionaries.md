# Sets and Dictionaries

> [← Practice Codes index](README.md)

Hash-based problems: set operations, counting with `Counter`, merging, inverting, sorting and grouping dictionaries.

**See also:** For tuple fundamentals see the course notebook [04_03 Tuples](../04_Collections/04_03_Python_Collections_Tuple.md) and the [Collections practice](../04_Collections/04_05_Practice%20code-%20Collections.md).

**7 problems** · 3 Easy · 3 Medium · 1 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Find the intersection and union of two lists](#1-find-the-intersection-and-union-of-two-lists) | Easy |
| 2 | [Find the most frequent element](#2-find-the-most-frequent-element) | Easy |
| 3 | [Merge two dictionaries, summing values for shared keys](#3-merge-two-dictionaries-summing-values-for-shared-keys) | Easy |
| 4 | [Invert a dictionary (keys <-> values)](#4-invert-a-dictionary-keys---values) | Medium |
| 5 | [Sort a list of dictionaries by multiple keys](#5-sort-a-list-of-dictionaries-by-multiple-keys) | Medium |
| 6 | [Find the top K frequent elements](#6-find-the-top-k-frequent-elements) | Medium |
| 7 | [Group a list of words by anagram](#7-group-a-list-of-words-by-anagram) | Hard |

---

## Easy

### 1. Find the intersection and union of two lists

```python
def intersection_union(a: list, b: list):
    return sorted(set(a) & set(b)), sorted(set(a) | set(b))

print(intersection_union([1, 2, 3, 4], [3, 4, 5]))

# ------------Expected Output---------

# ([3, 4], [1, 2, 3, 4, 5])
```

### 2. Find the most frequent element

```python
from collections import Counter

def most_frequent(items: list):
    return Counter(items).most_common(1)[0][0]

print(most_frequent([1, 3, 3, 2, 3, 1]))  # 3

# ------------Expected Output---------

# 3
```

### 3. Merge two dictionaries, summing values for shared keys

```python
from collections import Counter

def merge_sum(a: dict, b: dict) -> dict:
    return dict(Counter(a) + Counter(b))

print(merge_sum({"x": 1, "y": 2}, {"y": 3, "z": 4}))  # {'x': 1, 'y': 5, 'z': 4}

# ------------Expected Output---------

# {'x': 1, 'y': 5, 'z': 4}
```

---

## Medium

### 4. Invert a dictionary (keys <-> values)

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

### 5. Sort a list of dictionaries by multiple keys

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

### 6. Find the top K frequent elements

```python
from collections import Counter

def top_k_frequent(arr, k):
    return [num for num, _ in Counter(arr).most_common(k)]

print("Top K Frequent    :", top_k_frequent([1, 1, 1, 2, 2, 3], 2))

# ------------Expected Output---------

# Top K Frequent    : [1, 2]
```

---

## Hard

### 7. Group a list of words by anagram

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
