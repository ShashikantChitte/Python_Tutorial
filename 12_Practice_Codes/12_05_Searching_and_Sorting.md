# Searching and Sorting

> [← Practice Codes index](README.md)

Binary search and its variations, classic sorting algorithms, quickselect, merging sorted data and binary search on the answer.

**11 problems** · 0 Easy · 5 Medium · 6 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Implement binary search (iterative and recursive)](#1-implement-binary-search-iterative-and-recursive) | Medium |
| 2 | [First and last position of a target in a sorted array](#2-first-and-last-position-of-a-target-in-a-sorted-array) | Medium |
| 3 | [Search in a rotated sorted array (with duplicates)](#3-search-in-a-rotated-sorted-array-with-duplicates) | Medium |
| 4 | [Find a peak element](#4-find-a-peak-element) | Medium |
| 5 | [Search in a row-wise and column-wise sorted matrix](#5-search-in-a-row-wise-and-column-wise-sorted-matrix) | Medium |
| 6 | [Implement bubble sort, merge sort, and quick sort](#6-implement-bubble-sort-merge-sort-and-quick-sort) | Hard |
| 7 | [Quickselect: find the K-th smallest element](#7-quickselect-find-the-k-th-smallest-element) | Hard |
| 8 | [Median of two sorted arrays of different sizes – O(log(min(m, n)))](#8-median-of-two-sorted-arrays-of-different-sizes--ologminm-n) | Hard |
| 9 | [K-th element of two sorted arrays and merge K sorted arrays (min-heap)](#9-k-th-element-of-two-sorted-arrays-and-merge-k-sorted-arrays-min-heap) | Hard |
| 10 | [Merge without extra space, minimum swaps to sort and count inversions](#10-merge-without-extra-space-minimum-swaps-to-sort-and-count-inversions) | Hard |
| 11 | [Binary search on the answer: Koko eating bananas, ship within days, split array, bouquets](#11-binary-search-on-the-answer-koko-eating-bananas-ship-within-days-split-array-bouquets) | Hard |

---

## Medium

### 1. Implement binary search (iterative and recursive)

```python
def bsearch_iter(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1

def bsearch_rec(arr, target, lo=0, hi=None):
    hi = len(arr) - 1 if hi is None else hi
    if lo > hi:
        return -1
    mid = (lo + hi) // 2
    if arr[mid] == target:
        return mid
    if arr[mid] < target:
        return bsearch_rec(arr, target, mid + 1, hi)
    return bsearch_rec(arr, target, lo, mid - 1)

print(bsearch_iter([1, 3, 5, 7, 9], 7), bsearch_rec([1, 3, 5, 7, 9], 4))  # 3 -1

# ------------Expected Output---------

# 3 -1
```

### 2. First and last position of a target in a sorted array

```python
def first_last_position(arr, target):
    def bound(is_first):
        l, r, pos = 0, len(arr) - 1, -1
        while l <= r:
            mid = (l + r) // 2
            if arr[mid] == target:
                pos = mid
                if is_first:
                    r = mid - 1
                else:
                    l = mid + 1
            elif arr[mid] < target:
                l = mid + 1
            else:
                r = mid - 1
        return pos
    return [bound(True), bound(False)]

print("First/Last Pos :", first_last_position([1, 2, 2, 2, 3, 4], 2))

# ------------Expected Output---------

# First/Last Pos : [1, 3]
```

### 3. Search in a rotated sorted array (with duplicates)

```python
def search_rotated_array(arr, target):
    l, r = 0, len(arr) - 1
    while l <= r:
        mid = (l + r) // 2
        if arr[mid] == target:
            return True
        if arr[l] == arr[mid] == arr[r]:
            l += 1
            r -= 1
        elif arr[l] <= arr[mid]:
            if arr[l] <= target < arr[mid]:
                r = mid - 1
            else:
                l = mid + 1
        else:
            if arr[mid] < target <= arr[r]:
                l = mid + 1
            else:
                r = mid - 1
    return False

print("Search Rotated :", search_rotated_array([2, 5, 6, 0, 0, 1, 2], 0))

# ------------Expected Output---------

# Search Rotated : True
```

### 4. Find a peak element

```python
def peak_element(arr):
    l, r = 0, len(arr) - 1
    while l < r:
        mid = (l + r) // 2
        if arr[mid] < arr[mid + 1]:
            l = mid + 1
        else:
            r = mid
    return l

print("Peak Element   :", peak_element([1, 2, 1, 3, 5, 6, 4]))

# ------------Expected Output---------

# Peak Element   : 5
```

### 5. Search in a row-wise and column-wise sorted matrix

```python
def search_matrix(matrix, target):
    if not matrix:
        return False
    rows, cols = len(matrix), len(matrix[0])
    r, c = 0, cols - 1
    while r < rows and c >= 0:
        if matrix[r][c] == target:
            return True
        elif matrix[r][c] > target:
            c -= 1
        else:
            r += 1
    return False

print("Search Matrix  :", search_matrix([[1, 4, 7, 11], [2, 5, 8, 12], [3, 6, 9, 16]], 5))

# ------------Expected Output---------

# Search Matrix  : True
```

---

## Hard

### 6. Implement bubble sort, merge sort, and quick sort

```python
def bubble_sort(a):
    a = a[:]
    for i in range(len(a)):
        swapped = False
        for j in range(len(a) - i - 1):
            if a[j] > a[j + 1]:
                a[j], a[j + 1] = a[j + 1], a[j]
                swapped = True
        if not swapped:
            break
    return a

def merge_sort(a):
    if len(a) <= 1:
        return a
    mid = len(a) // 2
    left, right = merge_sort(a[:mid]), merge_sort(a[mid:])
    out, i, j = [], 0, 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            out.append(left[i]); i += 1
        else:
            out.append(right[j]); j += 1
    return out + left[i:] + right[j:]

def quick_sort(a):
    if len(a) <= 1:
        return a
    pivot = a[len(a) // 2]
    return (quick_sort([x for x in a if x < pivot])
            + [x for x in a if x == pivot]
            + quick_sort([x for x in a if x > pivot]))

data = [5, 2, 9, 1, 5, 6]
print(bubble_sort(data), merge_sort(data), quick_sort(data))

# ------------Expected Output---------

# [1, 2, 5, 5, 6, 9] [1, 2, 5, 5, 6, 9] [1, 2, 5, 5, 6, 9]
```

### 7. Quickselect: find the K-th smallest element

```python
def kth_smallest(arr, k):
    def partition(l, r):
        pivot, i = arr[r], l
        for j in range(l, r):
            if arr[j] <= pivot:
                arr[i], arr[j] = arr[j], arr[i]; i += 1
        arr[i], arr[r] = arr[r], arr[i]
        return i
    l, r = 0, len(arr) - 1
    while True:
        pos = partition(l, r)
        if pos == k - 1: return arr[pos]
        elif pos > k - 1: r = pos - 1
        else: l = pos + 1

print(kth_smallest([7, 10, 4, 3, 20, 15], 3))

# ------------Expected Output---------

# 7
```

### 8. Median of two sorted arrays of different sizes – O(log(min(m, n)))

```python
def median_two_sorted_arrays(nums1, nums2):
    A, B = (nums1, nums2) if len(nums1) <= len(nums2) else (nums2, nums1)
    total = len(A) + len(B)
    half = total // 2
    l, r = 0, len(A) - 1
    while True:
        i = (l + r) // 2
        j = half - i - 2
        Aleft = A[i] if i >= 0 else float("-inf")
        Aright = A[i + 1] if i + 1 < len(A) else float("inf")
        Bleft = B[j] if j >= 0 else float("-inf")
        Bright = B[j + 1] if j + 1 < len(B) else float("inf")

        if Aleft <= Bright and Bleft <= Aright:
            if total % 2:
                return min(Aright, Bright)
            return (max(Aleft, Bleft) + min(Aright, Bright)) / 2
        elif Aleft > Bright:
            r = i - 1
        else:
            l = i + 1

print(median_two_sorted_arrays([1, 3], [2]))  # 2

# ------------Expected Output---------

# 2
```

### 9. K-th element of two sorted arrays and merge K sorted arrays (min-heap)

```python
import heapq

def kth_of_two_sorted_arrays(a, b, k):
    i = j = 0
    while True:
        if i == len(a): return b[j + k - 1]
        if j == len(b): return a[i + k - 1]
        if k == 1: return min(a[i], b[j])
        mid = k // 2
        ai = min(i + mid, len(a)) - 1
        bj = min(j + mid, len(b)) - 1
        if a[ai] <= b[bj]:
            k -= (ai - i + 1); i = ai + 1
        else:
            k -= (bj - j + 1); j = bj + 1

def merge_k_sorted_arrays(arrays):
    heap = [(arr[0], i, 0) for i, arr in enumerate(arrays) if arr]
    heapq.heapify(heap)
    res = []
    while heap:
        val, i, j = heapq.heappop(heap)
        res.append(val)
        if j + 1 < len(arrays[i]):
            heapq.heappush(heap, (arrays[i][j + 1], i, j + 1))
    return res

print(kth_of_two_sorted_arrays([2, 3, 6, 7, 9], [1, 4, 8, 10], 5), merge_k_sorted_arrays([[1, 4, 5], [1, 3, 4], [2, 6]]))

# ------------Expected Output---------

# 6 [1, 1, 2, 3, 4, 4, 5, 6]
```

### 10. Merge without extra space, minimum swaps to sort and count inversions

```python
import math

def merge_without_extra_space(a, b):
    n, m = len(a), len(b)
    gap = math.ceil((n + m) / 2)
    while gap > 0:
        i, j = 0, gap
        while j < n + m:
            if j < n:
                if a[i] > a[j]: a[i], a[j] = a[j], a[i]
            elif i < n and j >= n:
                if a[i] > b[j - n]: a[i], b[j - n] = b[j - n], a[i]
            else:
                if b[i - n] > b[j - n]: b[i - n], b[j - n] = b[j - n], b[i - n]
            i += 1; j += 1
        if gap == 1: break
        gap = math.ceil(gap / 2)
    return a, b

def min_swaps_to_sort(arr):
    n = len(arr)
    arrpos = sorted(enumerate(arr), key=lambda it: it[1])
    visited = [False] * n
    ans = 0
    for i in range(n):
        if visited[i] or arrpos[i][0] == i:
            continue
        cycle_size, j = 0, i
        while not visited[j]:
            visited[j] = True; j = arrpos[j][0]; cycle_size += 1
        if cycle_size > 0:
            ans += cycle_size - 1
    return ans

def count_inversions(arr):
    def merge_sort(nums):
        if len(nums) <= 1: return nums, 0
        mid = len(nums) // 2
        left, inv_l = merge_sort(nums[:mid])
        right, inv_r = merge_sort(nums[mid:])
        i = j = inv = 0
        merged = []
        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                merged.append(left[i]); i += 1
            else:
                merged.append(right[j]); j += 1; inv += len(left) - i
        merged.extend(left[i:]); merged.extend(right[j:])
        return merged, inv_l + inv_r + inv
    return merge_sort(arr)[1]

print(merge_without_extra_space([1, 4, 7, 8], [2, 3, 9]), min_swaps_to_sort([4, 3, 2, 1]), count_inversions([2, 4, 1, 3, 5]))

# ------------Expected Output---------

# ([1, 2, 3, 4], [7, 8, 9]) 2 3
```

### 11. Binary search on the answer: Koko eating bananas, ship within days, split array, bouquets

```python
def koko_eating_bananas(piles, h):
    can_eat = lambda speed: sum((p + speed - 1) // speed for p in piles) <= h
    l, r = 1, max(piles)
    while l < r:
        mid = (l + r) // 2
        if can_eat(mid): r = mid
        else: l = mid + 1
    return l

def ship_within_days(weights, days):
    def can_ship(cap):
        d, curr = 1, 0
        for w in weights:
            if curr + w > cap: d += 1; curr = 0
            curr += w
        return d <= days
    l, r = max(weights), sum(weights)
    while l < r:
        mid = (l + r) // 2
        if can_ship(mid): r = mid
        else: l = mid + 1
    return l

def split_array_largest_sum(nums, m):
    def can_split(mid):
        count, curr = 1, 0
        for num in nums:
            if curr + num > mid: count += 1; curr = 0
            curr += num
        return count <= m
    l, r = max(nums), sum(nums)
    while l < r:
        mid = (l + r) // 2
        if can_split(mid): r = mid
        else: l = mid + 1
    return l

def min_days_bouquets(bloomDay, m, k):
    if m * k > len(bloomDay): return -1
    def can_make(day):
        bouquets = flowers = 0
        for d in bloomDay:
            if d <= day:
                flowers += 1
                if flowers == k: bouquets += 1; flowers = 0
            else: flowers = 0
        return bouquets >= m
    l, r = min(bloomDay), max(bloomDay)
    while l < r:
        mid = (l + r) // 2
        if can_make(mid): r = mid
        else: l = mid + 1
    return l

print(koko_eating_bananas([3, 6, 7, 11], 8), ship_within_days([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 5), split_array_largest_sum([7, 2, 5, 10, 8], 2), min_days_bouquets([1, 10, 3, 10, 2], 3, 1))

# ------------Expected Output---------

# 4 15 18 3
```
