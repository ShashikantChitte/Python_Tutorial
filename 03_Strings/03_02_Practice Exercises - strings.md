# Practice Code – Strings

> [← Course index](../README.md)

Try each task yourself first, then compare with the solution below it.

Tasks that need input use `input()`, so type your own answer when the cell asks for it. The final cell verifies all fixed-value solutions automatically.

### Quick Navigation
1. [Create Strings in Different Ways](#task-1)
2. [Immutable Strings Demo](#task-2)
3. [Concatenation and Repetition](#task-3)
4. [String Slicing Practice](#task-4)
5. [Check Substring using `in` and `not in`](#task-5)
6. [String Formatting with f-strings](#task-6)
7. [Case Conversion Methods](#task-7)
8. [Counting and Replacing](#task-8)
9. [Validation Check](#task-9)
10. [Split and Strip](#task-10)
11. [Bonus: Reverse & Palindrome Check](#task-11)
12. [Check All Solutions](#self-checks)

---

<a id="task-1"></a>
## 1. Create Strings in Different Ways

**Task:** Write a program to create strings using single quotes, double quotes, and the `str()` constructor. Print them and check their type using `type()`.

```python
print("--- 1. Create strings in different ways ---")
single = 'Python'            # single quotes
double = "Python"            # double quotes
constructor = str("Python")  # str() constructor

for text in (single, double, constructor):
    print(text, type(text))
```

**Output:**

```text
--- 1. Create strings in different ways ---
Python <class 'str'>
Python <class 'str'>
Python <class 'str'>
```

---

<a id="task-2"></a>
## 2. Immutable Strings Demo

**Task:** Create a string `"hello"`, print its memory address using `id()`. Now modify it (e.g., `s = s + " world"`) and print again. Show that the address changes.

```python
print("--- 2. Immutable strings demo ---")
s = "hello"
address_before = id(s)
print("address of 'hello'      :", address_before)

s = s + " world"
address_after = id(s)
print("address of 'hello world':", address_after)
print("Different object (strings are immutable)?", address_before != address_after)
```

**Output:**

```text
--- 2. Immutable strings demo ---
address of 'hello'      : 139877762664896
address of 'hello world': 139877757665776
Different object (strings are immutable)? True
```

---

<a id="task-3"></a>
## 3. Concatenation and Repetition

**Task:** Take two strings `"Python"` and `"Rocks"`.
- Concatenate them using `+`.
- Repeat the string `"Python"` 3 times using `*`.

```python
print("--- 3. Concatenation and repetition ---")
first, second = "Python", "Rocks"
joined = first + " " + second
repeated = first * 3

print(joined)
print(repeated)
```

**Output:**

```text
--- 3. Concatenation and repetition ---
Python Rocks
PythonPythonPython
```

---

<a id="task-4"></a>
## 4. String Slicing Practice

**Task:** Take a string `"welcome"`. Print:
- Characters from index `1` to `4` (`s[1:4]`)
- First 5 characters
- Last 3 characters using negative indexing
- Middle substring `"lco"` using slicing

```python
print("--- 4. String slicing practice ---")
s = "welcome"
from_1_to_4 = s[1:4]
first_five = s[:5]
last_three = s[-3:]
middle = s[2:5]

print("index 1 to 4  :", from_1_to_4)
print("first 5 chars :", first_five)
print("last 3 chars  :", last_three)
print("middle 'lco'  :", middle, "| same with negative indexes:", s[-5:-2])
```

**Output:**

```text
--- 4. String slicing practice ---
index 1 to 4  : elc
first 5 chars : welco
last 3 chars  : ome
middle 'lco'  : lco | same with negative indexes: lco
```

---

<a id="task-5"></a>
## 5. Check Substring using `in` and `not in`

**Task:** Write a program that asks the user to input a word. Check if `"python"` is present in that word (case-insensitive). Print `"Found"` or `"Not Found"`.

```python
print("--- 5. Substring check with in / not in ---")
word = input("Enter a word: ")
found = "python" in word.lower()
print("Found" if found else "Not Found")
print("Not in 'java' ->", "python" not in "java")
```

**Output:**

```text
--- 5. Substring check with in / not in ---
Enter a word: pythonic
Found
Not in 'java' -> True
```

---

<a id="task-6"></a>
## 6. String Formatting with f-strings

**Task:** Take user input for name and age. Use an f-string to print:
`"My name is <name>, and I am <age> years old."`

```python
print("--- 6. f-string formatting ---")
person = input("Enter your name: ")
age = input("Enter your age: ")
message = f"My name is {person}, and I am {age} years old."
print(message)
```

**Output:**

```text
--- 6. f-string formatting ---
Enter your name: Asha
Enter your age: 25
My name is Asha, and I am 25 years old.
```

---

<a id="task-7"></a>
## 7. Case Conversion Methods

**Task:** Write a program that takes `"welcome to python"` and prints:
- Capitalized string
- Title case string
- Uppercase string
- Swapcase string

```python
print("--- 7. Case conversion methods ---")
text = "welcome to python"
capitalized = text.capitalize()
title_case = text.title()
upper_case = text.upper()
swapped = text.swapcase()

print("capitalize:", capitalized)
print("title     :", title_case)
print("upper     :", upper_case)
print("swapcase  :", swapped)
```

**Output:**

```text
--- 7. Case conversion methods ---
capitalize: Welcome to python
title     : Welcome To Python
upper     : WELCOME TO PYTHON
swapcase  : WELCOME TO PYTHON
```

---

<a id="task-8"></a>
## 8. Counting and Replacing

**Task:** Given the string `"banana"`, count how many times `"a"` appears. Then replace `"banana"` with `"orange"` in the string `"I like banana"`.

```python
print("--- 8. Counting and replacing ---")
a_count = "banana".count("a")
replaced = "I like banana".replace("banana", "orange")

print("'a' appears in banana:", a_count, "times")
print(replaced)
```

**Output:**

```text
--- 8. Counting and replacing ---
'a' appears in banana: 3 times
I like orange
```

---

<a id="task-9"></a>
## 9. Validation Check

**Task:** Ask the user to input a string. Check if:
- It contains only alphabets (`isalpha()`)
- It contains only digits (`isdigit()`)
- It is alphanumeric (`isalnum()`)

Print results accordingly.

```python
print("--- 9. Validation check ---")
value = input("Enter a string: ")
only_alpha = value.isalpha()
only_digits = value.isdigit()
alnum = value.isalnum()

print("Only alphabets:", only_alpha)
print("Only digits   :", only_digits)
print("Alphanumeric  :", alnum)
```

**Output:**

```text
--- 9. Validation check ---
Enter a string: abc123
Only alphabets: False
Only digits   : False
Alphanumeric  : True
```

---

<a id="task-10"></a>
## 10. Split and Strip

**Task:**
- Given the string `" apple,banana,grape "`, strip spaces.
- Split the string by commas `,`.
- Print the list of fruits.

```python
print("--- 10. Split and strip ---")
raw = " apple,banana,grape "
stripped = raw.strip()
fruits = stripped.split(",")

print("after strip:", repr(stripped))
print("fruits     :", fruits)
for fruit in fruits:
    print(" -", fruit)
```

**Output:**

```text
--- 10. Split and strip ---
after strip: 'apple,banana,grape'
fruits     : ['apple', 'banana', 'grape']
 - apple
 - banana
 - grape
```

---

<a id="task-11"></a>
## 11. Bonus: Reverse & Palindrome Check

**Task:** Reverse the string `"madam"` using slicing (`[::-1]`) and check if it reads the same backward as forward (case-insensitive).

```python
print("--- 11. Reverse & palindrome check ---")
sample = "Madam"
reversed_sample = sample[::-1]
is_palindrome = sample.lower() == reversed_sample.lower()

print("Original  :", sample)
print("Reversed  :", reversed_sample)
print("Palindrome:", is_palindrome)
```

**Output:**

```text
--- 11. Reverse & palindrome check ---
Original  : Madam
Reversed  : madaM
Palindrome: True
```

---

<a id="self-checks"></a>
## 12. Check All Solutions

These `assert` statements verify the fixed-value solutions above. If something is wrong, Python stops with an `AssertionError`.

```python
assert type(single) is type(double) is type(constructor) is str
assert address_before != address_after
assert joined == "Python Rocks" and repeated == "PythonPythonPython"
assert (from_1_to_4, first_five, last_three, middle) == ("elc", "welco", "ome", "lco")
assert a_count == 3 and replaced == "I like orange"
assert (capitalized, title_case, upper_case, swapped) == (
    "Welcome to python", "Welcome To Python", "WELCOME TO PYTHON", "WELCOME TO PYTHON")
assert fruits == ["apple", "banana", "grape"]
assert is_palindrome is True

print("All self-checks passed.")
```

**Output:**

```text
All self-checks passed.
```

## Notes on the Solutions

- **#2** – The raw `id()` number is different on every run, so what matters is the check `address_before != address_after`.
- **#4** – "Index 1 to 4" is read the Python way: start index `1`, stop index `4` (excluded) $\rightarrow$ `s[1:4]` = `elc`. "Middle substring `lco`" is `s[2:5]` (or `s[-5:-2]`).
