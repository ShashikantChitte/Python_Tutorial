# Python Strings

> [← Course index](../README.md)

A string (`str`) is an **ordered, immutable sequence of characters** used to store and represent text.

### Table of Contents
1. [Creating Strings & Escape Characters](#creating-strings)
2. [String Operators & Membership](#string-operators)
3. [Indexing, Slicing & Reversing](#slicing-strings)
4. [Formatting Strings](#formatting-strings)
5. [Immutability & Length](#immutability)
6. [String Methods](#string-methods)

---

<a id="creating-strings"></a>
## 1. Creating Strings & Escape Characters

* Use **single quotes** (`'...'`), **double quotes** (`"..."`), **triple quotes** (`'''...'''` or `"""..."""`) for multi-line text, or the `str()` constructor.
* Use a backslash (`\`) to escape special characters inside quotes (e.g., `\'`, `\"`, `\n`, `\t`).
* Prefix with `r` (`r"C:\new\folder"`) to create a **raw string** where backslashes are treated literally.

> **Note:** Strings are always objects of the class `str`.

```python
print("--- Creating strings ---")
name = "john"            # double quotes
grade = 'B'              # single quotes
empty_name = str()       # empty string
full_name = str("David")
multiline = """Line 1
Line 2"""

print(name, grade, repr(empty_name), full_name)
print(type(full_name))   # <class 'str'>
print("It's Python!", r"C:\new\test.py")
```

**Output:**

```text
--- Creating strings ---
john B '' David
<class 'str'>
It's Python! C:\new\test.py
```

---

<a id="string-operators"></a>
## 2. String Operators & Membership

| Operator | Meaning | Example |
| :---: | :--- | :--- |
| `+` | Concatenates (joins) strings | `"Hi " + "All"` $\rightarrow$ `"Hi All"` |
| `*` | Repeats a string multiple times | `"Ha" * 3` $\rightarrow$ `"HaHaHa"` |
| `in` | Checks if a substring **exists** | `"come" in "welcome"` $\rightarrow$ `True` |
| `not in` | Checks if a substring **does not exist** | `"xyz" not in "welcome"` $\rightarrow$ `True` |

```python
print("--- String operators & membership ---")
s = "welcome"
print(s + " programming")
print(s * 3)

print('"come" in s     ->', "come" in s)
print('"lome" in s     ->', "lome" in s)
print('"come" not in s ->', "come" not in s)
print('"lome" not in s ->', "lome" not in s)
```

**Output:**

```text
--- String operators & membership ---
welcome programming
welcomewelcomewelcome
"come" in s     -> True
"lome" in s     -> False
"come" not in s -> False
"lome" not in s -> True
```

---

<a id="slicing-strings"></a>
## 3. Indexing, Slicing & Reversing

* Access a single character with `s[index]`, or extract a substring with `s[start:stop:step]`.
* Positive index starts from **`0`** (left $\rightarrow$ right).
* **Negative** index counts from **`-1`** at the end (right $\rightarrow$ left).
* `stop` is always **excluded**.

| index | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **char** | `w` | `e` | `l` | `c` | `o` | `m` | `e` |
| **negative** | `-7` | `-6` | `-5` | `-4` | `-3` | `-2` | `-1` |

```python
print("--- Slicing (example 1) ---")
mystr = "welcome"
print("mystr[0]     =", mystr[0])       # w (single character)
print("mystr[1:3]   =", mystr[1:3])     # el
print("mystr[:6]    =", mystr[:6])      # welcom
print("mystr[2:]    =", mystr[2:])      # lcome
print("mystr[1:-1]  =", mystr[1:-1])    # elcom
print("mystr[-5:-2] =", mystr[-5:-2])   # lco
```

**Output:**

```text
--- Slicing (example 1) ---
mystr[0]     = w
mystr[1:3]   = el
mystr[:6]    = welcom
mystr[2:]    = lcome
mystr[1:-1]  = elcom
mystr[-5:-2] = lco
```

```python
print("--- Slicing (example 2) ---")
s = "abcdefghijklmno"
print("s[-4:]     =", s[-4:])           # lmno
print("s[:-3]     =", s[:-3])           # abcdefghijkl
print("s[-5:-2]   =", s[-5:-2])         # klm
print("s[-8:-1:2] =", s[-8:-1:2])       # hjln
```

**Output:**

```text
--- Slicing (example 2) ---
s[-4:]     = lmno
s[:-3]     = abcdefghijkl
s[-5:-2]   = klm
s[-8:-1:2] = hjln
```

**Explanation (Example 2):**
* `s[-4:]` $\rightarrow$ from the 4th character from the end (`l`) till the end $\rightarrow$ `lmno`
* `s[:-3]` $\rightarrow$ from the start up to (excluding) the 3rd character from the end (`m`) $\rightarrow$ `abcdefghijkl`
* `s[-5:-2]` $\rightarrow$ from the 5th character from the end (`k`) up to (excluding) the 2nd from the end (`n`) $\rightarrow$ `klm`
* `s[-8:-1:2]` $\rightarrow$ from the 8th character from the end (`h`) up to (excluding) the last one, **step 2** $\rightarrow$ `hjln`

### Reverse a String
Use a **negative step** (`[::-1]`). This creates a reversed **copy**, leaving the original string unchanged.

```python
print("--- Reverse a string (negative step) ---")
s = "Python"
print(s[::-1], "| original unchanged:", s)
```

**Output:**

```text
--- Reverse a string (negative step) ---
nohtyP | original unchanged: Python
```

---

<a id="formatting-strings"></a>
## 4. Formatting Strings

* **f-strings** (Python 3.6+) are the recommended, cleanest way to embed variables and expressions directly inside strings.
* Older `.format()` still works when needed.

```python
print("--- f-strings ---")
age = 30
print(f"My name is John, I am {age}")
price = 59
print(f"The price is {price} dollars")
print(f"The price is {price:.2f} dollars")
print(f"The price is {20 * 59} dollars")
print("Old style: {}".format("format() still works"))
```

**Output:**

```text
--- f-strings ---
My name is John, I am 30
The price is 59 dollars
The price is 59.00 dollars
The price is 1180 dollars
Old style: format() still works
```

---

<a id="immutability"></a>
## 5. Strings are Immutable & String Length

* **Immutable object:** Cannot be modified in place after creation (`int`, `float`, `bool`, `tuple`, `str`). Trying to assign `s[0] = 'H'` raises a `TypeError`.
* **Mutable object:** Can be modified in place after creation (`list`, `dict`, `set`).
* **`len(s)`:** Returns the total number of characters (including spaces) in string `s`.

```python
print("--- Strings are immutable ---")
s1 = "hello"
print("Original String:", s1, "| Length:", len(s1))

try:
    s1[0] = "H"
except TypeError as err:
    print("TypeError:", err)

# Create a NEW string instead:
s2 = "H" + s1[1:]
print("Modified String (a NEW object):", s2)
print("s1 and s2 are different objects?", id(s1) != id(s2))
```

**Output:**

```text
--- Strings are immutable ---
Original String: hello | Length: 5
TypeError: 'str' object does not support item assignment
Modified String (a NEW object): Hello
s1 and s2 are different objects? True
```

```python
print("--- Lists are mutable (changed in place) ---")
lst = ["h", "e", "l", "l", "o"]
list_id_before = id(lst)
print("Original List:", lst)
lst[0] = "H"
print("Modified List:", lst)
print("Same object after change?", id(lst) == list_id_before)
```

**Output:**

```text
--- Lists are mutable (changed in place) ---
Original List: ['h', 'e', 'l', 'l', 'o']
Modified List: ['H', 'e', 'l', 'l', 'o']
Same object after change? True
```

---

<a id="string-methods"></a>
## 6. String Methods

> 💡 Because strings are immutable, **all string methods return a new string** and leave the original string untouched.

### (a) Case Conversion

```python
print("--- (a) Case conversion ---")
print("hello".capitalize())          # Hello
print("HeLLo".casefold())            # hello (aggressive lowercase for comparisons)
print("Hello".lower())               # hello
print("Hello".upper())               # HELLO
print("welcome to python".title())   # Welcome To Python
print("Welcome To Python".swapcase())# wELCOME tO pYTHON
```

**Output:**

```text
--- (a) Case conversion ---
Hello
hello
hello
HELLO
Welcome To Python
wELCOME tO pYTHON
```

### (b) Alignment & Formatting

```python
print("--- (b) Alignment & formatting ---")
print("Hi".center(6, "*"))
print("Hi".ljust(6, "-"))
print("Hi".rjust(6, "-"))
print("Hello, {}!".format("John"))
```

**Output:**

```text
--- (b) Alignment & formatting ---
**Hi**
Hi----
----Hi
Hello, John!
```

### (c) Search & Find
* `find(sub)` returns the lowest index of `sub`, or **`-1`** if not found.
* `index(sub)` works like `find()`, but raises a **`ValueError`** if not found.

```python
print("--- (c) Search & find ---")
s = "hello"
print('find("l")  ->', s.find("l"))     # 2
print('find("x")  ->', s.find("x"))     # -1 (not found)
print('index("e") ->', s.index("e"))    # 1

try:
    s.index("x")
except ValueError as err:
    print('index("x") -> ValueError:', err)
```

**Output:**

```text
--- (c) Search & find ---
find("l")  -> 2
find("x")  -> -1
index("e") -> 1
index("x") -> ValueError: substring not found
```

### (d) Count & Replace

```python
print("--- (d) Count & replace ---")
print('"banana".count("a") ->', "banana".count("a"))
print("Hello World".replace("World", "There"))
print("one one one".replace("one", "two", 2))   # optional count limit
```

**Output:**

```text
--- (d) Count & replace ---
"banana".count("a") -> 3
Hello There
two two one
```

### (e) Validation (Boolean Checks)

```python
print("--- (e) Validation (Boolean checks) ---")
print('"abc123".isalnum() ->', "abc123".isalnum())
print('"abc!".isalnum()   ->', "abc!".isalnum())
print('"Hello".isalpha()  ->', "Hello".isalpha())
print('"hello".islower()  ->', "hello".islower())
print('"HELLO".isupper()  ->', "HELLO".isupper())
print('"   ".isspace()    ->', "   ".isspace())
```

**Output:**

```text
--- (e) Validation (Boolean checks) ---
"abc123".isalnum() -> True
"abc!".isalnum()   -> False
"Hello".isalpha()  -> True
"hello".islower()  -> True
"HELLO".isupper()  -> True
"   ".isspace()    -> True
```

### (f) `isdecimal()`, `isdigit()`, `isnumeric()`

**`isdecimal()`**
- `True` if all characters are decimal characters (`0–9` only).
- Strictly the digits used to form base-10 numbers.
- Superscripts, fractions, and Roman numerals are **not** decimals.

```python
print("123".isdecimal())     # True   (all decimal digits)
print("①②③".isdecimal())     # False  (circled numbers are not decimal)
print("10.5".isdecimal())    # False  (the dot is not a decimal digit)
```

**Output:**

```text
True
False
False
```

**`isdigit()`**
- `True` if all characters are digits: decimal digits `0–9` plus superscripts, subscripts, and digits from other numeral systems.

```python
print("123".isdigit())     # True
print("²³".isdigit())      # True   (superscript 2 and 3)
print("⑤".isdigit())       # True   (circled digit)
print("10.5".isdigit())    # False  (dot not allowed)
```

**Output:**

```text
True
True
True
False
```

**`isnumeric()`**
- `True` if all characters are numeric – the **broadest** check: digits, decimals, vulgar fractions, Roman numerals, etc.
- Accepts everything `isdecimal()` and `isdigit()` accept, plus more.

```python
print("123".isnumeric())    # True
print("²³".isnumeric())     # True
print("⑤".isnumeric())      # True
print("⅔".isnumeric())      # True   (fraction two-thirds)
print("Ⅷ".isnumeric())      # True   (Roman numeral 8)
print("10.5".isnumeric())   # False
```

**Output:**

```text
True
True
True
True
True
False
```

| Method | Accepts only `0–9` | Superscripts / circled (`²`, `⑤`) | Fractions & Roman (`⅔`, `Ⅷ`) |
| :--- | :---: | :---: | :---: |
| `isdecimal()` | ✅ | ❌ | ❌ |
| `isdigit()` | ✅ | ✅ | ❌ |
| `isnumeric()` | ✅ | ✅ | ✅ |

- Use `isdecimal()` for base-10 digits only.
- Use `isdigit()` for digits + superscripts.
- Use `isnumeric()` for the broadest check.

### (g) Splitting & Joining

```python
print("--- (g) Splitting & joining ---")
s = "one,two,three"
print(s.split(","))              # ['one', 'two', 'three']
print("a b c".split())            # ['a', 'b', 'c'] (splits on whitespace by default)
print("-".join(["a", "b", "c"]))  # a-b-c
```

**Output:**

```text
--- (g) Splitting & joining ---
['one', 'two', 'three']
['a', 'b', 'c']
a-b-c
```

### (h) Start & End Checks

```python
print("--- (h) Start & end checks ---")
print('"hello".startswith("he")    ->', "hello".startswith("he"))
print('"hello.py".endswith(".py")  ->', "hello.py".endswith(".py"))
```

**Output:**

```text
--- (h) Start & end checks ---
"hello".startswith("he")    -> True
"hello.py".endswith(".py")  -> True
```

### (i) Trimming Whitespace

```python
print("--- (i) Trimming whitespace ---")
print(repr(" hello ".strip()))
print(repr(" hello".lstrip()))
print(repr("hello ".rstrip()))
print(repr("   hello   ".rstrip()), "<- rstrip keeps the leading spaces")
```

**Output:**

```text
--- (i) Trimming whitespace ---
'hello'
'hello'
'hello'
'   hello' <- rstrip keeps the leading spaces
```

## String Methods Summary

| Purpose | Methods |
| :--- | :--- |
| Case conversion | `capitalize()`, `casefold()`, `lower()`, `upper()`, `title()`, `swapcase()` |
| Alignment & formatting | `center()`, `ljust()`, `rjust()`, `format()` |
| Search & find | `find()`, `index()` |
| Counting & replacing | `count()`, `replace()` |
| Validation | `isalnum()`, `isalpha()`, `isdecimal()`, `isdigit()`, `isnumeric()`, `islower()`, `isupper()`, `isspace()` |
| Splitting & joining | `split()`, `join()` |
| Start/End checks | `startswith()`, `endswith()` |
| Trimming spaces | `strip()`, `lstrip()`, `rstrip()` |
