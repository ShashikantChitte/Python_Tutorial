# Strings

> [← Practice Codes index](README.md)

String manipulation, validation, parsing and string-matching algorithms – from reversing a string up to KMP and text justification.

**See also:** Dynamic-programming string problems (edit distance, longest common subsequence, longest palindromic subsequence) are in [Recursion and Dynamic Programming](10_06_Recursion_and_Dynamic_Programming.md).

**23 problems** · 10 Easy · 7 Medium · 6 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Reverse a string without slicing or reversed()](#1-reverse-a-string-without-slicing-or-reversed) | Easy |
| 2 | [Count the frequency of each character](#2-count-the-frequency-of-each-character) | Easy |
| 3 | [Reverse the words in a sentence](#3-reverse-the-words-in-a-sentence) | Easy |
| 4 | [Remove all vowels or whitespace from a string](#4-remove-all-vowels-or-whitespace-from-a-string) | Easy |
| 5 | [Count vowels, consonants, digits, and special characters](#5-count-vowels-consonants-digits-and-special-characters) | Easy |
| 6 | [Remove duplicate characters while preserving order](#6-remove-duplicate-characters-while-preserving-order) | Easy |
| 7 | [Check for a pangram and find the missing pangram characters](#7-check-for-a-pangram-and-find-the-missing-pangram-characters) | Easy |
| 8 | [Case conversions (toggle, camelCase, snake_case) and URLify](#8-case-conversions-toggle-camelcase-snake_case-and-urlify) | Easy |
| 9 | [Reverse only the vowels in a string](#9-reverse-only-the-vowels-in-a-string) | Easy |
| 10 | [Check string rotation and subsequence](#10-check-string-rotation-and-subsequence) | Easy |
| 11 | [Check if a string is a palindrome (ignore case and punctuation)](#11-check-if-a-string-is-a-palindrome-ignore-case-and-punctuation) | Medium |
| 12 | [Check if two strings are anagrams](#12-check-if-two-strings-are-anagrams) | Medium |
| 13 | [Find the first non-repeating character](#13-find-the-first-non-repeating-character) | Medium |
| 14 | [Find duplicate characters in a string](#14-find-duplicate-characters-in-a-string) | Medium |
| 15 | [Compress a string (aaabb -> a3b2)](#15-compress-a-string-aaabb---a3b2) | Medium |
| 16 | [Find the longest common prefix of a list of strings](#16-find-the-longest-common-prefix-of-a-list-of-strings) | Medium |
| 17 | [Convert camelCase to snake_case and back](#17-convert-camelcase-to-snake_case-and-back) | Medium |
| 18 | [Find the longest substring without repeating characters](#18-find-the-longest-substring-without-repeating-characters) | Hard |
| 19 | [Check whether parentheses/brackets are balanced](#19-check-whether-parenthesesbrackets-are-balanced) | Hard |
| 20 | [Check if a string is a valid number, IP address, or email](#20-check-if-a-string-is-a-valid-number-ip-address-or-email) | Hard |
| 21 | [Minimum window substring and smallest window containing all distinct characters](#21-minimum-window-substring-and-smallest-window-containing-all-distinct-characters) | Hard |
| 22 | [Wildcard matching, regex matching and KMP pattern search](#22-wildcard-matching-regex-matching-and-kmp-pattern-search) | Hard |
| 23 | [Reorganize a string, restore IP addresses and justify text](#23-reorganize-a-string-restore-ip-addresses-and-justify-text) | Hard |

---

## Easy

### 1. Reverse a string without slicing or reversed()

```python
def reverse_string(s: str) -> str:
    chars = list(s)
    i, j = 0, len(chars) - 1
    while i < j:
        chars[i], chars[j] = chars[j], chars[i]
        i += 1
        j -= 1
    return "".join(chars)

print(reverse_string("hello"))  # olleh

# ------------Expected Output---------

# olleh
```

### 2. Count the frequency of each character

```python
from collections import Counter

def char_freq(s: str) -> dict:
    return dict(Counter(s))

print(char_freq("banana"))  # {'b': 1, 'a': 3, 'n': 2}

# ------------Expected Output---------

# {'b': 1, 'a': 3, 'n': 2}
```

### 3. Reverse the words in a sentence

```python
def reverse_words(sentence: str) -> str:
    return " ".join(reversed(sentence.split()))

print(reverse_words("the sky is blue"))  # blue is sky the

# ------------Expected Output---------

# blue is sky the
```

### 4. Remove all vowels or whitespace from a string

```python
import re

def remove_vowels(s: str) -> str:
    return re.sub(r"[aeiouAEIOU]", "", s)

def remove_whitespace(s: str) -> str:
    return "".join(s.split())

print(remove_vowels("Automation"), remove_whitespace(" a b  c "))  # tmtn abc

# ------------Expected Output---------

# tmtn abc
```

### 5. Count vowels, consonants, digits, and special characters

```python
def classify_chars(s: str) -> dict:
    res = {"vowels": 0, "consonants": 0, "digits": 0, "special": 0}
    for ch in s:
        if ch.isalpha():
            res["vowels" if ch.lower() in "aeiou" else "consonants"] += 1
        elif ch.isdigit():
            res["digits"] += 1
        elif not ch.isspace():
            res["special"] += 1
    return res

print(classify_chars("Hello World 123 !@#"))

# ------------Expected Output---------

# {'vowels': 3, 'consonants': 7, 'digits': 3, 'special': 3}
```

### 6. Remove duplicate characters while preserving order

```python
def remove_duplicate_chars(s: str) -> str:
    return "".join(dict.fromkeys(s))

print("Dedup Chars    :", remove_duplicate_chars("programming"))

# ------------Expected Output---------

# Dedup Chars    : progamin
```

### 7. Check for a pangram and find the missing pangram characters

```python
import string

def is_pangram(s: str) -> bool:
    return set(string.ascii_lowercase).issubset(set(s.lower()))


def missing_pangram_chars(s: str) -> str:
    return "".join(sorted(set(string.ascii_lowercase) - set(s.lower())))



print("Is Pangram     :", is_pangram(
    "The quick brown fox jumps over the lazy dog"))
print("Missing Chars  :", missing_pangram_chars("hello world"))

# ------------Expected Output---------

# Is Pangram     : True
# Missing Chars  : abcfgijkmnpqstuvxyz
```

### 8. Case conversions (toggle, camelCase, snake_case) and URLify

```python
def toggle_case(s: str) -> str:
    return s.swapcase()


def to_camel_case(s: str) -> str:
    words = s.split()
    return words[0].lower() + "".join(w.capitalize() for w in words[1:]) if words else ""


def to_snake_case(s: str) -> str:
    return "_".join(s.lower().split())


def urlify(s: str) -> str:
    return s.strip().replace(" ", "%20")


print("Toggle Case    :", toggle_case("HeLLo WoRLd"))
print("Camel Case     :", to_camel_case("hello world from python"))
print("Snake Case     :", to_snake_case("Hello World From Python"))
print("URLify         :", urlify("Mr John Smith   "))

# ------------Expected Output---------

# Toggle Case    : hEllO wOrlD
# Camel Case     : helloWorldFromPython
# Snake Case     : hello_world_from_python
# URLify         : Mr%20John%20Smith
```

### 9. Reverse only the vowels in a string

```python
def reverse_vowels(s: str) -> str:
    vowels = [ch for ch in s if ch.lower() in "aeiou"]
    return "".join(vowels.pop() if ch.lower() in "aeiou" else ch for ch in s)


print("Reverse Vowels :", reverse_vowels("hello"))

# -----------Expected Output--------

# Reverse Vowels : holle
```

### 10. Check string rotation and subsequence

```python
def is_rotation(s1: str, s2: str) -> bool:
    return len(s1) == len(s2) and s2 in (s1 + s1)

def is_subsequence(s: str, t: str) -> bool:
    it = iter(t)
    return all(ch in it for ch in s)


print("Is Rotation    :", is_rotation("waterbottle", "erbottlewat"))
print("Is Subsequence :", is_subsequence("abc", "ahbgdc"))

# -----------Expected Output--------

# Is Rotation    : True
# Is Subsequence : True
```

---

## Medium

### 11. Check if a string is a palindrome (ignore case and punctuation)

**Two-pointer approach**

```python
import re

def is_palindrome(s: str) -> bool:
    cleaned = re.sub(r"[^a-z0-9]", "", s.lower())
    i, j = 0, len(cleaned) - 1
    while i < j:
        if cleaned[i] != cleaned[j]:
            return False
        i, j = i + 1, j - 1
    return True

print(is_palindrome("A man, a plan, a canal: Panama"))  # True

# ------------Expected Output---------

# True
```

**Shorter version using slicing**

```python
import re

def is_palindrome(s: str) -> bool:
    cleaned = re.sub(r"[^a-z0-9]", "", s.lower())
    return cleaned == cleaned[::-1]

print("Is Palindrome  :", is_palindrome("A man, a plan, a canal: Panama"))

# ------------Expected Output---------

# Is Palindrome  : True
```

### 12. Check if two strings are anagrams

```python
from collections import Counter

def is_anagram(a: str, b: str) -> bool:
    norm = lambda s: Counter(s.replace(" ", "").lower())
    return norm(a) == norm(b)

print(is_anagram("Listen", "Silent"))  # True

# ------------Expected Output---------

# True
```

### 13. Find the first non-repeating character

```python
from collections import Counter

def first_unique_char(s: str):
    counts = Counter(s)
    for ch in s:
        if counts[ch] == 1:
            return ch
    return None

print(first_unique_char("swiss"))  # w

# ------------Expected Output---------

# w
```

### 14. Find duplicate characters in a string

```python
from collections import Counter

def duplicate_chars(s: str) -> list:
    return [ch for ch, n in Counter(s).items() if n > 1]

print(duplicate_chars("programming"))  # ['r', 'g', 'm']

# ------------Expected Output---------

# ['r', 'g', 'm']
```

### 15. Compress a string (aaabb -> a3b2)

```python
def compress(s: str) -> str:
    if not s:
        return ""
    out, count = [], 1
    for prev, cur in zip(s, s[1:]):
        if cur == prev:
            count += 1
        else:
            out.append(f"{prev}{count}")
            count = 1
    out.append(f"{s[-1]}{count}")
    return "".join(out)

print(compress("aaabb"))  # a3b2

# ------------Expected Output---------

# a3b2
```

### 16. Find the longest common prefix of a list of strings

```python
def longest_common_prefix(words: list[str]) -> str:
    if not words:
        return ""
    prefix = words[0]
    for w in words[1:]:
        while not w.startswith(prefix):
            prefix = prefix[:-1]
            if not prefix:
                return ""
    return prefix

print(longest_common_prefix(["flower", "flow", "flight"]))  # fl

# ------------Expected Output---------

# fl
```

### 17. Convert camelCase to snake_case and back

```python
import re

def camel_to_snake(s: str) -> str:
    return re.sub(r"(?<!^)(?=[A-Z])", "_", s).lower()

def snake_to_camel(s: str) -> str:
    first, *rest = s.split("_")
    return first + "".join(w.capitalize() for w in rest)

print(camel_to_snake("userLoginCount"))  # user_login_count
print(snake_to_camel("user_login_count"))  # userLoginCount

# ------------Expected Output---------

# user_login_count
# userLoginCount
```

---

## Hard

### 18. Find the longest substring without repeating characters

```python
def longest_unique_substring(s: str) -> str:
    last_seen, start, best = {}, 0, (0, 0)
    for i, ch in enumerate(s):
        if ch in last_seen and last_seen[ch] >= start:
            start = last_seen[ch] + 1
        last_seen[ch] = i
        if i - start + 1 > best[1] - best[0]:
            best = (start, i + 1)
    return s[best[0]:best[1]]

print(longest_unique_substring("abcabcbb"))  # abc

# ------------Expected Output---------

# abc
```

**Variant – return only the length**

```python
def longest_unique_substring(s: str) -> int:
    seen, start, max_len = {}, 0, 0
    for i, ch in enumerate(s):
        if ch in seen and seen[ch] >= start:
            start = seen[ch] + 1
        seen[ch] = i
        max_len = max(max_len, i - start + 1)
    return max_len

print("Longest Unique Sub:", longest_unique_substring("abcabcbb"))

# ------------Expected Output---------

# Longest Unique Sub: 3
```

### 19. Check whether parentheses/brackets are balanced

```python
def is_balanced(s: str) -> bool:
    pairs = {")": "(", "]": "[", "}": "{"}
    stack = []
    for ch in s:
        if ch in pairs.values():
            stack.append(ch)
        elif ch in pairs:
            if not stack or stack.pop() != pairs[ch]:
                return False
    return not stack

print(is_balanced("{[()]}"), is_balanced("{[(])}"))  # True False

# ------------Expected Output---------

# True False
```

### 20. Check if a string is a valid number, IP address, or email

```python
import ipaddress, re

def is_number(s: str) -> bool:
    try:
        float(s)
        return True
    except ValueError:
        return False

def is_ip(s: str) -> bool:
    try:
        ipaddress.ip_address(s)
        return True
    except ValueError:
        return False

EMAIL_RE = re.compile(r"^[\w.+-]+@[A-Za-z0-9-]+(\.[A-Za-z0-9-]+)+$")
def is_email(s: str) -> bool:
    return bool(EMAIL_RE.match(s))

print(is_number("-3.14"), is_ip("192.168.1.1"), is_email("a.b@test.com"))

# ------------Expected Output---------

# True True True
```

### 21. Minimum window substring and smallest window containing all distinct characters

```python
from collections import Counter

def min_window(s: str, t: str) -> str:
    need, missing = Counter(t), len(t)
    left = start = end = 0
    for right, ch in enumerate(s, 1):
        if need[ch] > 0: missing -= 1
        need[ch] -= 1
        if missing == 0:
            while left < right and need[s[left]] < 0:
                need[s[left]] += 1; left += 1
            if end == 0 or right - left <= end - start:
                start, end = left, right
    return s[start:end]

def smallest_window_all_chars(s: str) -> int:
    need = len(set(s))
    freq, l, formed, min_len = {}, 0, 0, float('inf')
    for r, ch in enumerate(s):
        freq[ch] = freq.get(ch, 0) + 1
        if freq[ch] == 1: formed += 1
        while formed == need:
            min_len = min(min_len, r - l + 1)
            freq[s[l]] -= 1
            if freq[s[l]] == 0: formed -= 1
            l += 1
    return min_len

print(min_window("ADOBECODEBANC", "ABC"), smallest_window_all_chars("aabcbcdbca"))

# ------------Expected Output---------

# BANC 4
```

### 22. Wildcard matching, regex matching and KMP pattern search

```python
def is_match_wildcard(s: str, p: str) -> bool:
    dp = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]
    dp[0][0] = True
    for j in range(1, len(p) + 1):
        if p[j - 1] == "*": dp[0][j] = dp[0][j - 1]
    for i in range(1, len(s) + 1):
        for j in range(1, len(p) + 1):
            if p[j - 1] in ("?", s[i - 1]): dp[i][j] = dp[i - 1][j - 1]
            elif p[j - 1] == "*": dp[i][j] = dp[i - 1][j] or dp[i][j - 1]
    return dp[-1][-1]

def regex_match(s: str, p: str) -> bool:
    dp = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]
    dp[0][0] = True
    for j in range(2, len(p) + 1):
        if p[j - 1] == "*" and dp[0][j - 2]: dp[0][j] = True
    for i in range(1, len(s) + 1):
        for j in range(1, len(p) + 1):
            if p[j - 1] in (".", s[i - 1]): dp[i][j] = dp[i - 1][j - 1]
            elif p[j - 1] == "*": dp[i][j] = dp[i][j - 2] or (dp[i - 1][j] and p[j - 2] in (".", s[i - 1]))
    return dp[-1][-1]

def kmp_search(text: str, pattern: str) -> int:
    lps = [0] * len(pattern)
    length, i = 0, 1
    while i < len(pattern):
        if pattern[i] == pattern[length]:
            length += 1; lps[i] = length; i += 1
        elif length != 0:
            length = lps[length - 1]
        else:
            i += 1
    i = j = 0
    while i < len(text):
        if text[i] == pattern[j]: i += 1; j += 1
        if j == len(pattern): return i - j
        elif i < len(text) and text[i] != pattern[j]:
            if j != 0: j = lps[j - 1]
            else: i += 1
    return -1

print(is_match_wildcard("adceb", "*a*b"), regex_match("aab", "c*a*b"), kmp_search("abxabcabcaby", "abcaby"))

# ------------Expected Output---------

# True True 6
```

### 23. Reorganize a string, restore IP addresses and justify text

```python
import heapq
from collections import Counter
from math import factorial

def reorganize_string(s: str) -> str:
    heap = [(-freq, ch) for ch, freq in Counter(s).items()]
    heapq.heapify(heap)
    prev, result = (0, ""), []
    while heap:
        freq, ch = heapq.heappop(heap)
        result.append(ch)
        if prev[0] < 0: heapq.heappush(heap, prev)
        prev = (freq + 1, ch)
    return "".join(result) if len(result) == len(s) else ""

def restore_ip_addresses(s: str):
    result = []
    def backtrack(start, parts):
        if start == len(s) and len(parts) == 4: result.append(".".join(parts)); return
        if len(parts) >= 4: return
        for length in range(1, 4):
            if start + length <= len(s):
                part = s[start:start + length]
                if (part.startswith("0") and len(part) > 1) or int(part) > 255: continue
                backtrack(start + length, parts + [part])
    backtrack(0, [])
    return result

def full_justify(words, maxWidth):
    res, cur, num_of_letters = [], [], 0
    for w in words:
        if num_of_letters + len(w) + len(cur) > maxWidth:
            for i in range(maxWidth - num_of_letters):
                cur[i % (len(cur) - 1 or 1)] += ' '
            res.append(''.join(cur))
            cur, num_of_letters = [], 0
        cur.append(w); num_of_letters += len(w)
    res.append(' '.join(cur).ljust(maxWidth))
    return res

print(reorganize_string("aab"), restore_ip_addresses("25525511135"), full_justify(["This", "is", "an", "example", "of", "text", "justification."], 16))

# ------------Expected Output---------

# aba ['255.255.11.135', '255.255.111.35'] ['This    is    an', 'example  of text', 'justification.  ']
```
