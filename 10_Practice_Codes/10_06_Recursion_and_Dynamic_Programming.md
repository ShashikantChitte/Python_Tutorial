# Recursion and Dynamic Programming

> [← Practice Codes index](README.md)

Recursion, memoization and dynamic programming on strings, sequences and matrices.

**See also:** Wildcard / regex matching (a DP on strings) is in [Strings](10_01_Strings.md).

**5 problems** · 1 Easy · 1 Medium · 3 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Factorial using recursion and iteration](#1-factorial-using-recursion-and-iteration) | Easy |
| 2 | [Fibonacci with memoization](#2-fibonacci-with-memoization) | Medium |
| 3 | [Edit distance, longest common subsequence and interleaving strings](#3-edit-distance-longest-common-subsequence-and-interleaving-strings) | Hard |
| 4 | [Longest palindromic subsequence, palindrome partitioning and longest valid parentheses](#4-longest-palindromic-subsequence-palindrome-partitioning-and-longest-valid-parentheses) | Hard |
| 5 | [Rod cutting, minimum jumps, coin change (count ways) and largest square of 1s](#5-rod-cutting-minimum-jumps-coin-change-count-ways-and-largest-square-of-1s) | Hard |

---

## Easy

### 1. Factorial using recursion and iteration

```python
def fact_rec(n: int) -> int:
    if n < 0:
        raise ValueError("negative")
    return 1 if n <= 1 else n * fact_rec(n - 1)

def fact_iter(n: int) -> int:
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

print(fact_rec(5), fact_iter(5))  # 120 120

# ------------Expected Output---------

# 120 120
```

---

## Medium

### 2. Fibonacci with memoization

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n: int) -> int:
    return n if n < 2 else fib(n - 1) + fib(n - 2)

print(fib(50))  # 12586269025

# ------------Expected Output---------

# 12586269025
```

---

## Hard

### 3. Edit distance, longest common subsequence and interleaving strings

```python
def edit_distance(s1: str, s2: str) -> int:
    n, m = len(s1), len(s2)
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        for j in range(m + 1):
            if i == 0: dp[i][j] = j
            elif j == 0: dp[i][j] = i
            elif s1[i - 1] == s2[j - 1]: dp[i][j] = dp[i - 1][j - 1]
            else: dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[n][m]

def longest_common_subsequence(s1: str, s2: str) -> str:
    n, m = len(s1), len(s2)
    dp = [[""] * (m + 1) for _ in range(n + 1)]
    for i in range(n):
        for j in range(m):
            if s1[i] == s2[j]: dp[i + 1][j + 1] = dp[i][j] + s1[i]
            else: dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j], key=len)
    return dp[n][m]

def is_interleave(s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3): return False
    dp = [[False] * (len(s2) + 1) for _ in range(len(s1) + 1)]
    dp[0][0] = True
    for i in range(len(s1) + 1):
        for j in range(len(s2) + 1):
            if i > 0: dp[i][j] |= dp[i - 1][j] and s1[i - 1] == s3[i + j - 1]
            if j > 0: dp[i][j] |= dp[i][j - 1] and s2[j - 1] == s3[i + j - 1]
    return dp[-1][-1]

print(edit_distance("horse", "ros"), longest_common_subsequence("abcde", "ace"), is_interleave("aabcc", "dbbca", "aadbbcbcac"))

# ------------Expected Output---------

# 3 ace True
```

### 4. Longest palindromic subsequence, palindrome partitioning and longest valid parentheses

```python
def longest_palindromic_subsequence(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n): dp[i][i] = 1
    for cl in range(2, n + 1):
        for i in range(n - cl + 1):
            j = i + cl - 1
            if s[i] == s[j] and cl == 2: dp[i][j] = 2
            elif s[i] == s[j]: dp[i][j] = dp[i + 1][j - 1] + 2
            else: dp[i][j] = max(dp[i][j - 1], dp[i + 1][j])
    return dp[0][n - 1]

def palindromic_partitions(s: str):
    result = []
    def backtrack(start, path):
        if start == len(s): result.append(path[:]); return
        for end in range(start + 1, len(s) + 1):
            sub = s[start:end]
            if sub == sub[::-1]: backtrack(end, path + [sub])
    backtrack(0, [])
    return result

def longest_valid_parentheses(s: str) -> int:
    stack, max_len = [-1], 0
    for i, ch in enumerate(s):
        if ch == '(': stack.append(i)
        else:
            stack.pop()
            if not stack: stack.append(i)
            else: max_len = max(max_len, i - stack[-1])
    return max_len

print(longest_palindromic_subsequence("bbbab"), palindromic_partitions("aab"), longest_valid_parentheses(")()())"))

# ------------Expected Output---------

# 4 [['a', 'a', 'b'], ['aa', 'b']] 4
```

### 5. Rod cutting, minimum jumps, coin change (count ways) and largest square of 1s

```python
def rod_cutting(prices, n):
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        for j in range(i): dp[i] = max(dp[i], prices[j] + dp[i - j - 1])
    return dp[n]

def min_jumps(arr):
    jumps = cur_end = cur_farthest = 0
    for i in range(len(arr) - 1):
        cur_farthest = max(cur_farthest, i + arr[i])
        if i == cur_end: jumps += 1; cur_end = cur_farthest
    return jumps

def coin_change_count(coins, amount):
    dp = [0] * (amount + 1)
    dp[0] = 1
    for coin in coins:
        for i in range(coin, amount + 1): dp[i] += dp[i - coin]
    return dp[amount]

def largest_square(matrix):
    rows, cols = len(matrix), len(matrix[0])
    dp = [[0] * cols for _ in range(rows)]
    max_side = 0
    for i in range(rows):
        for j in range(cols):
            if matrix[i][j] == 1:
                dp[i][j] = 1 if (i == 0 or j == 0) else min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1
                max_side = max(max_side, dp[i][j])
    return max_side

print(rod_cutting([1, 5, 8, 9, 10, 17, 17, 20], 8), min_jumps([2, 3, 1, 1, 4]), coin_change_count([1, 2, 5], 5), largest_square([[0, 1, 1, 0], [1, 1, 1, 1], [0, 1, 1, 1], [0, 1, 1, 0]]))

# ------------Expected Output---------

# 22 2 4 2
```
