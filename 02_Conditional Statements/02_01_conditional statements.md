# Python Conditional Statements

> [← Course index](../README.md)

Conditional statements allow a program to **make decisions** and execute different blocks of code based on whether a condition evaluates to `True` or `False`.

### Table of Contents
1. [Indentation](#indentation)
2. [`if`, `if-else`, and `if-elif-else`](#if-elif-else)
3. [Short-Hand `if` & Ternary Operator](#short-hand)
4. [Logical Operators & Chained Comparisons](#logical-operators)
5. [Nested `if` & `pass` Statement](#nested-and-pass)
6. [Practice Examples](#extra-examples)

---

<a id="indentation"></a>
## Indentation

* Python relies on **indentation** (whitespace at the start of a line—standard is **4 spaces**) to define code blocks.
* Missing or inconsistent indentation raises an `IndentationError`.

---

<a id="if-elif-else"></a>
## 1. `if`, `if-else`, and `if-elif-else`

* **`if`:** Runs a block **only if** the condition is `True`.
* **`else`:** Runs when all preceding `if`/`elif` conditions are `False`.
* **`elif`:** Short for *"else if"*—checks additional conditions in order and stops at the **first `True` match**.

```python
if condition1:
    # runs if condition1 is True
elif condition2:
    # runs if condition1 is False and condition2 is True
else:
    # runs if all conditions above are False
```

```python
print("--- 1. if ---")
age = 18
if age >= 18:
    print("You are eligible to vote.")
```

**Output:**

```text
--- 1. if ---
You are eligible to vote.
```

```python
print("--- 2. if ... else ---")
age = 16
if age >= 18:
    print("You are eligible to vote.")
else:
    print("You are not eligible to vote.")
```

**Output:**

```text
--- 2. if ... else ---
You are not eligible to vote.
```

```python
print("--- 3. if ... elif ... else ---")
marks = 75
if marks >= 90:
    print("Grade: A")
elif marks >= 75:
    print("Grade: B")
elif marks >= 50:
    print("Grade: C")
else:
    print("Fail")
```

**Output:**

```text
--- 3. if ... elif ... else ---
Grade: B
```

---

<a id="short-hand"></a>
## 2. Short-Hand `if` & Ternary Operator

* **Short-hand `if`:** Place a single statement on the same line.
* **Ternary Expression (`value_if_true if condition else value_if_false`):** Great for assigning values cleanly on one line.

```python
print("--- Short-hand if ---")
a, b = 10, 5
if a > b: print("a is greater than b")

print("\n--- Short-hand if ... else (Ternary) ---")
a, b = 2, 330
print("A") if a > b else print("B")

# Practical ternary assignment:
status = "Adult" if a >= 18 else "Minor"
print("Status:", status)

# Chained ternary (3 conditions):
a, b = 330, 330
print("A") if a > b else print("=") if a == b else print("B")
```

**Output:**

```text
--- Short-hand if ---
a is greater than b

--- Short-hand if ... else (Ternary) ---
B
Status: Minor
=
```

---

<a id="logical-operators"></a>
## 3. Logical Operators & Chained Comparisons

* **`and`:** True only if **both** conditions are `True`.
* **`or`:** True if **at least one** condition is `True`.
* **`not`:** Reverses `True` to `False` and vice versa.
* **Chained Comparisons:** Python lets you write `18 <= age <= 65` directly instead of `age >= 18 and age <= 65`.
* **Truthy / Falsy Values:** Empty strings `""`, `0`, and `None` act as `False` in `if` conditions; non-empty values act as `True`.

```python
print("--- and, or, not ---")
a, b, c = 200, 33, 500

if a > b and c > a:
    print("and: Both conditions are True")

if a > b or a > c:
    print("or: At least one condition is True")

if not a > c:
    print("not: a is NOT greater than c")

# Chained comparison & Truthy check
age = 25
if 18 <= age <= 65:
    print("Working age (chained comparison)")

username = "Alice"
if username:    # True because string is non-empty
    print(f"Welcome, {username}!")
```

**Output:**

```text
--- and, or, not ---
and: Both conditions are True
or: At least one condition is True
not: a is NOT greater than c
Working age (chained comparison)
Welcome, Alice!
```

---

<a id="nested-and-pass"></a>
## 4. Nested `if` & `pass` Statement

* **Nested `if`:** Placing an `if` statement inside another `if` block for multi-step checks.
* **`pass` Statement:** A null placeholder used when an `if` block is syntactically required to have content, but you haven't written the code yet.

```python
print("--- Nested if ---")
x = 41
if x > 10:
    print("Above ten,")
    if x > 20:
        print("and also above 20!")
    else:
        print("but not above 20.")

print("\n--- pass ---")
a, b = 33, 200
if b > a:
    pass            # placeholder so Python doesn't raise an IndentationError
print("pass did nothing, program continues")
```

**Output:**

```text
--- Nested if ---
Above ten,
and also above 20!

--- pass ---
pass did nothing, program continues
```

---

<a id="extra-examples"></a>
## 5. Extra Examples

```python
print("--- Extra 1: Discount slabs (if-elif-else) ---")
for amount in (500, 3000, 7000, 20000):
    if amount > 10000:
        discount = amount * 0.20
    elif amount > 5000:
        discount = amount * 0.10
    elif amount > 1000:
        discount = amount * 0.05
    else:
        discount = 0
    print(f"amount={amount:<6} discount={discount:<7.1f} to pay={amount - discount:.1f}")
```

**Output:**

```text
--- Extra 1: Discount slabs (if-elif-else) ---
amount=500    discount=0.0     to pay=500.0
amount=3000   discount=150.0   to pay=2850.0
amount=7000   discount=700.0   to pay=6300.0
amount=20000  discount=4000.0  to pay=16000.0
```

```python
print("--- Extra 2: Even or Odd ---")
for num in (10, 11):
    print(num, "Even number" if num % 2 == 0 else "Odd number")

print("\n--- Extra 3: Largest of three numbers ---")
a, b, c = 30, 200, 50
if a >= b and a >= c:
    print("a is largest:", a)
elif b >= a and b >= c:
    print("b is largest:", b)
else:
    print("c is largest:", c)
```

**Output:**

```text
--- Extra 2: Even or Odd ---
10 Even number
11 Odd number

--- Extra 3: Largest of three numbers ---
b is largest: 200
```

```python
print("--- Extra 4: Nested if (divisibility by 2 and 3) ---")
for num in (6, 4, 9, 7):
    if num % 2 == 0:
        if num % 3 == 0:
            print(num, "divisible by both 2 & 3")
        else:
            print(num, "divisible by 2 but not 3")
    else:
        if num % 3 == 0:
            print(num, "divisible by 3 but not 2")
        else:
            print(num, "not divisible by 2 and 3")
```

**Output:**

```text
--- Extra 4: Nested if (divisibility by 2 and 3) ---
6 divisible by both 2 & 3
4 divisible by 2 but not 3
9 divisible by 3 but not 2
7 not divisible by 2 and 3
```
