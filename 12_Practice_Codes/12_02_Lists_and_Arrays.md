# Lists and Arrays

> [← Practice Codes index](README.md)

Working with Python lists as arrays: scanning, in-place updates, two pointers, hashing, prefix sums, sliding windows and interval problems.

**See also:** Binary-search and sorting problems on arrays are in [Searching and Sorting](12_05_Searching_and_Sorting.md). Counting and grouping with `Counter` / `dict` is in [Sets and Dictionaries](12_03_Sets_and_Dictionaries.md).

**28 problems** · 9 Easy · 14 Medium · 5 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Find the largest element in an array](#1-find-the-largest-element-in-an-array) | Easy |
| 2 | [Find the second largest element (sorting approach)](#2-find-the-second-largest-element-sorting-approach) | Easy |
| 3 | [Find the third largest element](#3-find-the-third-largest-element) | Easy |
| 4 | [Find the minimum and maximum of an array](#4-find-the-minimum-and-maximum-of-an-array) | Easy |
| 5 | [Check whether an array contains duplicates](#5-check-whether-an-array-contains-duplicates) | Easy |
| 6 | [Find the first repeating element](#6-find-the-first-repeating-element) | Easy |
| 7 | [Create a wave array and find array leaders](#7-create-a-wave-array-and-find-array-leaders) | Easy |
| 8 | [Remove duplicates while preserving order](#8-remove-duplicates-while-preserving-order) | Easy |
| 9 | [Find the missing number in a sequence of 1..n](#9-find-the-missing-number-in-a-sequence-of-1n) | Easy |
| 10 | [Find the second largest element in a list (single pass)](#10-find-the-second-largest-element-in-a-list-single-pass) | Medium |
| 11 | [Find the two numbers that sum to a target (Two Sum)](#11-find-the-two-numbers-that-sum-to-a-target-two-sum) | Medium |
| 12 | [Rotate a list by k positions](#12-rotate-a-list-by-k-positions) | Medium |
| 13 | [Merge two sorted lists](#13-merge-two-sorted-lists) | Medium |
| 14 | [Move all zeros to the end of a list](#14-move-all-zeros-to-the-end-of-a-list) | Medium |
| 15 | [Find all triplets with a given sum (3-Sum)](#15-find-all-triplets-with-a-given-sum-3-sum) | Medium |
| 16 | [Container with the most water](#16-container-with-the-most-water) | Medium |
| 17 | [Sort 0s, 1s and 2s (Dutch National Flag)](#17-sort-0s-1s-and-2s-dutch-national-flag) | Medium |
| 18 | [Maximum product subarray](#18-maximum-product-subarray) | Medium |
| 19 | [Longest subarray with sum K](#19-longest-subarray-with-sum-k) | Medium |
| 20 | [Product of array except self (product array puzzle)](#20-product-of-array-except-self-product-array-puzzle) | Medium |
| 21 | [Majority element (Boyer–Moore voting)](#21-majority-element-boyermoore-voting) | Medium |
| 22 | [Merge overlapping intervals](#22-merge-overlapping-intervals) | Medium |
| 23 | [Minimum number of platforms](#23-minimum-number-of-platforms) | Medium |
| 24 | [Flatten a nested list](#24-flatten-a-nested-list) | Hard |
| 25 | [Find the maximum subarray sum (Kadane's algorithm)](#25-find-the-maximum-subarray-sum-kadanes-algorithm) | Hard |
| 26 | [First missing positive and duplicates in a limited-range array](#26-first-missing-positive-and-duplicates-in-a-limited-range-array) | Hard |
| 27 | [Sliding window maximum, sum of subarray minimums and max-sum subarray of size at least K](#27-sliding-window-maximum-sum-of-subarray-minimums-and-max-sum-subarray-of-size-at-least-k) | Hard |
| 28 | [Form the largest number from a list of integers](#28-form-the-largest-number-from-a-list-of-integers) | Hard |

---

## Easy

### 1. Find the largest element in an array

```python
def largest_in_array(arr):
    return max(arr) 

print("Largest:", largest_in_array([3, 7, 2, 9, 5]))

#------------Expected Output---------

# Largest: 9
```

### 2. Find the second largest element (sorting approach)

```python
def second_largest(arr):
    unique = sorted(set(arr), reverse=True)
    return unique[1] if len(unique) > 1 else None

print("Second Largest:", second_largest([3, 7, 2, 9, 5]))

# ------------Expected Output---------

# Second Largest: 7
```

### 3. Find the third largest element

```python
def third_largest(arr):
    unique = sorted(set(arr), reverse=True)
    return unique[2] if len(unique) >= 3 else None
print("Third Largest :", third_largest([12, 13, 1, 10, 34, 1]))

# ------------Expected Output---------

# Third Largest : 12
```

### 4. Find the minimum and maximum of an array

```python
def min_max(arr):
    return min(arr), max(arr)
print("Min & Max     :", min_max([3, 7, 2, 9, 5]))

# ------------Expected Output---------

# Min & Max     : (2, 9)
```

### 5. Check whether an array contains duplicates

```python
def contains_duplicates(arr):
    return len(arr) != len(set(arr))


print("Has Duplicates  :", contains_duplicates([1, 2, 3, 1]))

# ------------Expected Output---------

# Has Duplicates  : True
```

### 6. Find the first repeating element

```python
def first_repeating(arr):
    seen = set()
    for num in arr:
        if num in seen:
            return num
        seen.add(num)
    return None

print("First Repeating :", first_repeating([10, 5, 3, 4, 3, 5, 6]))

# ------------Expected Output---------

# First Repeating : 3
```

### 7. Create a wave array and find array leaders

```python
def wave_array(arr):
    arr.sort()
    for i in range(0, len(arr) - 1, 2):
        arr[i], arr[i + 1] = arr[i + 1], arr[i]
    return arr


def array_leaders(arr):
    leaders = []
    max_right = float("-inf")
    for num in reversed(arr):
        if num > max_right:
            leaders.append(num)
            max_right = num
    return leaders[::-1]

print("Wave Array      :", wave_array([1, 2, 3, 4, 5, 6]))
print("Array Leaders   :", array_leaders([16, 17, 4, 3, 5, 2]))

# ------------Expected Output---------

# Wave Array      : [2, 1, 4, 3, 6, 5]
# Array Leaders   : [17, 5, 2]
```

### 8. Remove duplicates while preserving order

```python
def dedupe(items: list) -> list:
    return list(dict.fromkeys(items))

print(dedupe([3, 1, 3, 2, 1]))  # [3, 1, 2]

# ------------Expected Output---------

# [3, 1, 2]
```

### 9. Find the missing number in a sequence of 1..n

```python
def missing_number(nums: list[int], n: int) -> int:
    return n * (n + 1) // 2 - sum(nums)

print(missing_number([1, 2, 4, 5], 5))  # 3

# ------------Expected Output---------

# 3
```

---

## Medium

### 10. Find the second largest element in a list (single pass)

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

### 11. Find the two numbers that sum to a target (Two Sum)

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

### 12. Rotate a list by k positions

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

### 13. Merge two sorted lists

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

### 14. Move all zeros to the end of a list

> **Note:** The second approach comes from the Easy notebook, where it was listed under the heading "Reverse Array (In-place)".

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

**Second approach – swap non-zeros forward**

```python
def move_zeroes(arr):
    j = 0
    for i in range(len(arr)):
        if arr[i] != 0:
            arr[i], arr[j] = arr[j], arr[i]
            j += 1
    return arr

print("Move Zeroes   :", move_zeroes([0, 1, 0, 3, 12]))

# ------------Expected Output---------

# Move Zeroes   : [1, 3, 12, 0, 0]
```

### 15. Find all triplets with a given sum (3-Sum)

```python
def triplet_sum(arr, target):
    arr.sort()
    res = []
    for i in range(len(arr) - 2):
        l, r = i + 1, len(arr) - 1
        while l < r:
            s = arr[i] + arr[l] + arr[r]
            if s == target:
                res.append((arr[i], arr[l], arr[r]))
                l += 1
                r -= 1
            elif s < target:
                l += 1
            else:
                r -= 1
    return res

print("3-Sum          :", triplet_sum([1, 2, -1, 0, -2, 1], 0))

# ------------Expected Output---------

# 3-Sum          : [(-2, 0, 2), (-2, 1, 1), (-1, 0, 1)]
```

### 16. Container with the most water

```python
def max_area(height):
    l, r, best = 0, len(height) - 1, 0
    while l < r:
        best = max(best, min(height[l], height[r]) * (r - l))
        if height[l] < height[r]:
            l += 1
        else:
            r -= 1
    return best

print("Max Water Area :", max_area([1, 8, 6, 2, 5, 4, 8, 3, 7]))

# ------------Expected Output---------

# Max Water Area : 49
```

### 17. Sort 0s, 1s and 2s (Dutch National Flag)

```python
def sort_colors(arr):
    low, mid, high = 0, 0, len(arr) - 1
    while mid <= high:
        if arr[mid] == 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
            mid += 1
        elif arr[mid] == 1:
            mid += 1
        else:
            arr[mid], arr[high] = arr[high], arr[mid]
            high -= 1
    return arr

print("Sort Colors    :", sort_colors([2, 0, 2, 1, 1, 0]))

# ------------Expected Output---------

# Sort Colors    : [0, 0, 1, 1, 2, 2]
```

### 18. Maximum product subarray

```python
def max_product_subarray(arr):
    max_p = min_p = res = arr[0]
    for x in arr[1:]:
        if x < 0:
            max_p, min_p = min_p, max_p
        max_p = max(x, max_p * x)
        min_p = min(x, min_p * x)
        res = max(res, max_p)
    return res

print("Max Product Sub   :", max_product_subarray([2, 3, -2, 4]))

# ------------Expected Output---------

# Max Product Sub   : 6
```

### 19. Longest subarray with sum K

```python
def longest_subarray_sum_k(arr, k):
    prefix, s, max_len = {}, 0, 0
    for i, val in enumerate(arr):
        s += val
        if s == k:
            max_len = i + 1
        if s - k in prefix:
            max_len = max(max_len, i - prefix[s - k])
        if s not in prefix:
            prefix[s] = i
    return max_len

print("Longest Sum K     :", longest_subarray_sum_k([10, 5, 2, 7, 1, 9], 15))

# ------------Expected Output---------

# Longest Sum K     : 4
```

### 20. Product of array except self (product array puzzle)

```python
def product_array_puzzle(arr):
    n = len(arr)
    left, right = [1] * n, [1] * n
    for i in range(1, n):
        left[i] = left[i - 1] * arr[i - 1]
    for i in range(n - 2, -1, -1):
        right[i] = right[i + 1] * arr[i + 1]
    return [left[i] * right[i] for i in range(n)]

print("Product Puzzle    :", product_array_puzzle([10, 3, 5, 6, 2]))

# ------------Expected Output---------

# Product Puzzle    : [180, 600, 360, 300, 900]
```

### 21. Majority element (Boyer–Moore voting)

```python
def majority_element(arr):
    cand, count = None, 0
    for num in arr:
        if count == 0:
            cand = num
        count += 1 if num == cand else -1
    return cand

print("Majority Element  :", majority_element([3, 3, 4, 2, 3, 3, 5]))

# ------------Expected Output---------

# Majority Element  : 3
```

### 22. Merge overlapping intervals

```python
def merge_intervals(intervals):
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    for cur in intervals[1:]:
        if cur[0] <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], cur[1])
        else:
            merged.append(cur)
    return merged

print("Merged Intervals  :", merge_intervals([[1, 3], [2, 6], [8, 10], [15, 18]]))

# ------------Expected Output---------

# Merged Intervals  : [[1, 6], [8, 10], [15, 18]]
```

### 23. Minimum number of platforms

```python
def minimum_platforms(arrivals, departures):
    arrivals.sort()
    departures.sort()
    i = j = platforms = max_plat = 0
    while i < len(arrivals) and j < len(departures):
        if arrivals[i] <= departures[j]:
            platforms += 1
            max_plat = max(max_plat, platforms)
            i += 1
        else:
            platforms -= 1
            j += 1
    return max_plat

print("Min Platforms     :", minimum_platforms([900, 940, 950, 1100, 1500, 1800], [910, 1200, 1120, 1130, 1900, 2000]))

# ------------Expected Output---------

# Min Platforms     : 3
```

---

## Hard

### 24. Flatten a nested list

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

### 25. Find the maximum subarray sum (Kadane's algorithm)

```python
def max_subarray(nums: list[int]) -> int:
    best = cur = nums[0]
    for n in nums[1:]:
        cur = max(n, cur + n)
        best = max(best, cur)
    return best

print(max_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # 6

# ------------Expected Output---------

# 6
```

### 26. First missing positive and duplicates in a limited-range array

```python
def first_missing_positive(arr):
    n = len(arr)
    for i in range(n):
        while 1 <= arr[i] <= n and arr[arr[i] - 1] != arr[i]:
            arr[arr[i] - 1], arr[i] = arr[i], arr[arr[i] - 1]
    for i in range(n):
        if arr[i] != i + 1:
            return i + 1
    return n + 1

def find_duplicates(arr):
    res = []
    for i in range(len(arr)):
        idx = abs(arr[i]) - 1
        if arr[idx] < 0:
            res.append(abs(arr[i]))
        else:
            arr[idx] = -arr[idx]
    return res

print(first_missing_positive([3, 4, -1, 1]), find_duplicates([4, 3, 2, 7, 8, 2, 3, 1]))

# ------------Expected Output---------

# 2 [2, 3]
```

### 27. Sliding window maximum, sum of subarray minimums and max-sum subarray of size at least K

```python
from collections import deque

def sliding_window_maximum(arr, k):
    dq, res = deque(), []
    for i, val in enumerate(arr):
        while dq and dq[0] <= i - k: dq.popleft()
        while dq and arr[dq[-1]] < val: dq.pop()
        dq.append(i)
        if i >= k - 1: res.append(arr[dq[0]])
    return res

def sum_subarray_mins(arr):
    MOD, n = 10**9 + 7, len(arr)
    stack, left, right = [], [0] * n, [0] * n
    for i in range(n):
        count = 1
        while stack and stack[-1][0] > arr[i]:
            _, cnt = stack.pop(); count += cnt
        left[i] = count; stack.append((arr[i], count))
    stack = []
    for i in range(n - 1, -1, -1):
        count = 1
        while stack and stack[-1][0] >= arr[i]:
            _, cnt = stack.pop(); count += cnt
        right[i] = count; stack.append((arr[i], count))
    return sum(arr[i] * left[i] * right[i] for i in range(n)) % MOD

def max_sum_subarray_at_least_k(arr, k):
    n = len(arr)
    prefix = [0] * n
    prefix[0] = arr[0]
    for i in range(1, n): prefix[i] = max(arr[i], prefix[i - 1] + arr[i])
    s = res = sum(arr[:k])
    for i in range(k, n):
        s += arr[i] - arr[i - k]
        res = max(res, s, s + prefix[i - k])
    return res

print(sliding_window_maximum([1, 3, -1, -3, 5, 3, 6, 7], 3), sum_subarray_mins([3, 1, 2, 4]), max_sum_subarray_at_least_k([1, 2, 3, -10, 4, 2, 3], 2))

# ------------Expected Output---------

# [3, 3, 5, 5, 6, 7] 17 9
```

### 28. Form the largest number from a list of integers

```python
from functools import cmp_to_key

def largest_number(nums):
    def compare(x, y):
        return -1 if x + y > y + x else 1
    strs = sorted(map(str, nums), key=cmp_to_key(compare))
    res = ''.join(strs)
    return '0' if res[0] == '0' else res

print(largest_number([3, 30, 34, 5, 9]))

# ------------Expected Output---------

# 9534330
```
