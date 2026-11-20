# Practice Code – Strings

> [← Course index](../README.md)

String practice from beginner to interview level: slicing, searching, counting, cleaning and validating text. Each problem is independent and runnable.

**21 problems** · 12 Easy · 9 Medium · 0 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Reverse a string without slicing or reversed()](#1-reverse-a-string-without-slicing-or-reversed) | Easy |
| 2 | [Count the frequency of each character](#2-count-the-frequency-of-each-character) | Easy |
| 3 | [Reverse the words in a sentence](#3-reverse-the-words-in-a-sentence) | Easy |
| 4 | [Remove all vowels from a string](#4-remove-all-vowels-from-a-string) | Easy |
| 5 | [Remove all whitespace from a string](#5-remove-all-whitespace-from-a-string) | Easy |
| 6 | [Count vowels, consonants, digits and special characters](#6-count-vowels-consonants-digits-and-special-characters) | Easy |
| 7 | [Remove duplicate characters while preserving order](#7-remove-duplicate-characters-while-preserving-order) | Easy |
| 8 | [Check whether a sentence is a pangram](#8-check-whether-a-sentence-is-a-pangram) | Easy |
| 9 | [Find the missing letters of a pangram](#9-find-the-missing-letters-of-a-pangram) | Easy |
| 10 | [Reverse only the vowels in a string](#10-reverse-only-the-vowels-in-a-string) | Medium |
| 11 | [Check if a string is a palindrome (ignore case and punctuation)](#11-check-if-a-string-is-a-palindrome-ignore-case-and-punctuation) | Medium |
| 12 | [Check whether two strings are anagrams](#12-check-whether-two-strings-are-anagrams) | Medium |
| 13 | [Find the first non-repeating character](#13-find-the-first-non-repeating-character) | Medium |
| 14 | [Find the duplicate characters in a string](#14-find-the-duplicate-characters-in-a-string) | Medium |
| 15 | [Compress a string (aaabb → a3b2)](#15-compress-a-string-aaabb--a3b2) | Medium |
| 16 | [Find the longest common prefix of a list of words](#16-find-the-longest-common-prefix-of-a-list-of-words) | Medium |
| 17 | [Convert camelCase to snake_case](#17-convert-camelcase-to-snake_case) | Medium |
| 18 | [Convert snake_case to camelCase](#18-convert-snake_case-to-camelcase) | Medium |
| 19 | [Count the words and find the longest word in a sentence](#19-count-the-words-and-find-the-longest-word-in-a-sentence) | Easy |
| 20 | [Mask a credit card number](#20-mask-a-credit-card-number) | Easy |
| 21 | [Search, count and replace inside a string](#21-search-count-and-replace-inside-a-string) | Easy |

---

## 1. Reverse a string without slicing or reversed()

**Level:** Easy

**Problem:** Reverse `"hello"` without using `[::-1]` or `reversed()`.

**Explanation:** Turn the string into a list, then swap characters from the two ends moving towards the middle (two-pointer technique), and join the list back.

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

---

## 2. Count the frequency of each character

**Level:** Easy

**Problem:** Return a dictionary of how many times each character appears.

**Explanation:** `collections.Counter` counts the items of any iterable; wrapping it in `dict()` gives a normal dictionary.

```python
from collections import Counter

def char_freq(s: str) -> dict:
    return dict(Counter(s))

print(char_freq("banana"))  # {'b': 1, 'a': 3, 'n': 2}

# ------------Expected Output---------

# {'b': 1, 'a': 3, 'n': 2}
```

---

## 3. Reverse the words in a sentence

**Level:** Easy

**Problem:** Turn `"the sky is blue"` into `"blue is sky the"`.

**Explanation:** `split()` breaks the sentence into words, `reversed()` flips their order and `" ".join()` stitches them back together.

```python
def reverse_words(sentence: str) -> str:
    return " ".join(reversed(sentence.split()))

print(reverse_words("the sky is blue"))  # blue is sky the

# ------------Expected Output---------

# blue is sky the
```

---

## 4. Remove all vowels from a string

**Level:** Easy

**Problem:** Remove every vowel (upper or lower case) from a word.

**Explanation:** `re.sub` with the character class `[aeiouAEIOU]` replaces every vowel with an empty string.

```python
import re

def remove_vowels(s: str) -> str:
    return re.sub(r"[aeiouAEIOU]", "", s)

print(remove_vowels("Automation"))

# ------------Expected Output---------

# tmtn
```

---

## 5. Remove all whitespace from a string

**Level:** Easy

**Problem:** Remove spaces, tabs and newlines from a string.

**Explanation:** `split()` with no argument splits on any run of whitespace, and joining the pieces with an empty string leaves no whitespace behind.

```python
def remove_whitespace(s: str) -> str:
    return "".join(s.split())

print(remove_whitespace(" a b  c "))

# ------------Expected Output---------

# abc
```

---

## 6. Count vowels, consonants, digits and special characters

**Level:** Easy

**Problem:** Classify every character of a string and count each category.

**Explanation:** Use `isalpha()` and `isdigit()` to classify each character. Letters are vowels or consonants depending on whether they appear in `"aeiou"`; anything else that is not a space is a special character.

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

---

## 7. Remove duplicate characters while preserving order

**Level:** Easy

**Problem:** Turn `"programming"` into `"progamin"` – keep only the first occurrence of each character.

**Explanation:** `dict.fromkeys(s)` creates a dictionary whose keys are the unique characters in their original order, so joining the keys gives the answer.

```python
def remove_duplicate_chars(s: str) -> str:
    return "".join(dict.fromkeys(s))

print(remove_duplicate_chars("programming"))

# ------------Expected Output---------

# progamin
```

---

## 8. Check whether a sentence is a pangram

**Level:** Easy

**Problem:** A pangram contains every letter of the alphabet at least once. Check a sentence.

**Explanation:** Compare the set of letters in the sentence with `string.ascii_lowercase`; if the alphabet is a subset of the sentence’s letters, it is a pangram.

```python
import string

def is_pangram(s: str) -> bool:
    return set(string.ascii_lowercase).issubset(set(s.lower()))

print(is_pangram("The quick brown fox jumps over the lazy dog"))
print(is_pangram("hello world"))

# ------------Expected Output---------

# True
# False
```

---

## 9. Find the missing letters of a pangram

**Level:** Easy

**Problem:** List the alphabet letters that are missing from a sentence.

**Explanation:** Subtract the set of letters in the sentence from the set of all lowercase letters, then sort the result for readable output.

```python
import string

def missing_pangram_chars(s: str) -> str:
    return "".join(sorted(set(string.ascii_lowercase) - set(s.lower())))

print(missing_pangram_chars("hello world"))

# ------------Expected Output---------

# abcfgijkmnpqstuvxyz
```

---

## 10. Reverse only the vowels in a string

**Level:** Medium

**Problem:** Reverse the order of the vowels while keeping every other character in place (`"hello"` → `"holle"`).

**Explanation:** Collect the vowels into a list, then rebuild the string; each time a vowel position is reached, take the last remaining vowel with `pop()`.

```python
def reverse_vowels(s: str) -> str:
    vowels = [ch for ch in s if ch.lower() in "aeiou"]
    return "".join(vowels.pop() if ch.lower() in "aeiou" else ch for ch in s)

print(reverse_vowels("hello"))

# ------------Expected Output---------

# holle
```

---

## 11. Check if a string is a palindrome (ignore case and punctuation)

**Level:** Medium

**Problem:** Decide whether a phrase such as `"A man, a plan, a canal: Panama"` reads the same both ways.

**Explanation:** Lower-case the text and strip everything except letters and digits with a regex, then compare characters from both ends with two pointers.

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

---

## 12. Check whether two strings are anagrams

**Level:** Medium

**Problem:** Decide whether `"Listen"` and `"Silent"` use exactly the same letters.

**Explanation:** Normalise each string (remove spaces, lower-case) and compare their `Counter` objects – equal letter counts mean an anagram.

```python
from collections import Counter

def is_anagram(a: str, b: str) -> bool:
    norm = lambda s: Counter(s.replace(" ", "").lower())
    return norm(a) == norm(b)

print(is_anagram("Listen", "Silent"))  # True

# ------------Expected Output---------

# True
```

---

## 13. Find the first non-repeating character

**Level:** Medium

**Problem:** Return the first character of `"swiss"` that appears only once.

**Explanation:** Count all characters first, then scan the string again and return the first one whose count is 1.

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

---

## 14. Find the duplicate characters in a string

**Level:** Medium

**Problem:** Return the characters of `"programming"` that appear more than once.

**Explanation:** Count characters with `Counter`, then keep those whose count is greater than 1 using a list comprehension.

```python
from collections import Counter

def duplicate_chars(s: str) -> list:
    return [ch for ch, n in Counter(s).items() if n > 1]

print(duplicate_chars("programming"))  # ['r', 'g', 'm']

# ------------Expected Output---------

# ['r', 'g', 'm']
```

---

## 15. Compress a string (aaabb → a3b2)

**Level:** Medium

**Problem:** Run-length encode a string.

**Explanation:** Walk through pairs of neighbouring characters with `zip(s, s[1:])`, count each run and append `character + count` when the character changes. The final run is appended after the loop.

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

---

## 16. Find the longest common prefix of a list of words

**Level:** Medium

**Problem:** Find the longest prefix shared by `["flower", "flow", "flight"]`.

**Explanation:** Start with the first word as the prefix and shorten it until each following word starts with it.

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

---

## 17. Convert camelCase to snake_case

**Level:** Medium

**Problem:** Turn `userLoginCount` into `user_login_count`.

**Explanation:** A regex with a lookahead inserts an underscore before every capital letter (except at the start), then the result is lower-cased.

```python
import re

def camel_to_snake(s: str) -> str:
    return re.sub(r"(?<!^)(?=[A-Z])", "_", s).lower()

print(camel_to_snake("userLoginCount"))

# ------------Expected Output---------

# user_login_count
```

---

## 18. Convert snake_case to camelCase

**Level:** Medium

**Problem:** Turn `user_login_count` into `userLoginCount`.

**Explanation:** Split on underscores, keep the first word as is and capitalise the first letter of each remaining word.

```python
def snake_to_camel(s: str) -> str:
    first, *rest = s.split("_")
    return first + "".join(w.capitalize() for w in rest)

print(snake_to_camel("user_login_count"))

# ------------Expected Output---------

# userLoginCount
```

---

## 19. Count the words and find the longest word in a sentence

**Level:** Easy

**Problem:** Report how many words a sentence has and which word is the longest.

**Explanation:** `split()` gives the words, `len()` counts them, and `max(words, key=len)` returns the longest one (the first one when there is a tie).

```python
sentence = "Python makes automation testing simple and powerful"

words = sentence.split()
print("Word count   :", len(words))
print("Longest word :", max(words, key=len))

# ------------Expected Output---------

# Word count   : 7
# Longest word : automation
```

---

## 20. Mask a credit card number

**Level:** Easy

**Problem:** Show only the last four digits of a card number and hide the rest with `*`.

**Explanation:** Slice off the last four characters and multiply `"*"` by the number of hidden characters. Negative slicing (`[-4:]`) makes the code independent of the number length.

```python
def mask_card(number: str) -> str:
    digits = number.replace(" ", "")
    return "*" * (len(digits) - 4) + digits[-4:]

print(mask_card("4111 1111 1111 1234"))

# ------------Expected Output---------

# ************1234
```

---

## 21. Search, count and replace inside a string

**Level:** Easy

**Problem:** Find where a word occurs, count its occurrences and replace it.

**Explanation:** `find()` returns the first index (or -1), `count()` counts non-overlapping matches and `replace()` returns a new string because strings are immutable.

```python
text = "test one, test two, test three"

print(text.find("test"))
print(text.find("four"))
print(text.count("test"))
print(text.replace("test", "check", 2))

# ------------Expected Output---------

# 0
# -1
# 3
# check one, check two, test three
```
