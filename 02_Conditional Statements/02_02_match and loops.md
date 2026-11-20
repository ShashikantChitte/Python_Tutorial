# Match, `range()` Function & Loops in Python

> [← Course index](../README.md)

### Table of Contents
1. [`match` Statement (Python 3.10+)](#match-statement)
2. [`range()` Function](#range-function)
3. [Looping Statements (`while` & `for`)](#looping-statements)
4. [Loop Control (`break`, `continue`, `loop-else`)](#jumping-statements)

---

<a id="match-statement"></a>
## 1. Python `match` Statement

> ⚠️ Requires **Python 3.10+**.

* Compares a value against multiple `case` patterns and runs the **first matching block** (similar to `switch` in other languages).
* **No `break` needed:** Unlike C/Java, Python automatically exits the `match` block after a match—there is no fall-through.
* Use `|` to combine multiple patterns in one `case`, and `case _:` as the default fallback.

```python
match expression:
    case value1:
        # code block
    case value2 | value3:
        # matches either value2 or value3
    case _:
        # default block (if no match)
```

```python
print("--- match statement ---")
for day in (1, 3, 7, 10):
    match day:
        case 1: print(day, "-> Sunday")
        case 2: print(day, "-> Monday")
        case 3: print(day, "-> Tuesday")
        case 4: print(day, "-> Wednesday")
        case 5: print(day, "-> Thursday")
        case 6: print(day, "-> Friday")
        case 7: print(day, "-> Saturday")
        case _: print(day, "-> Invalid week day")        # default
```

**Output:**

```text
--- match statement ---
1 -> Sunday
3 -> Tuesday
7 -> Saturday
10 -> Invalid week day
```

```python
print("--- match: combine values with | ---")
for day in (1, 4, 10):
    match day:
        case 2 | 3 | 4 | 5 | 6: print(day, "-> Weekday")
        case 1 | 7:             print(day, "-> Weekend")
        case _:                 print(day, "-> Invalid week day")
```

**Output:**

```text
--- match: combine values with | ---
1 -> Weekend
4 -> Weekday
10 -> Invalid week day
```

---

<a id="range-function"></a>
## 2. `range()` Function

* Generates a memory-efficient **sequence of integers**.
* **Syntax:** `range(start, stop, step)`

| Parameter | Meaning | Default |
| :--- | :--- | :--- |
| `start` | Beginning number (inclusive) | `0` |
| `stop` | End number (**exclusive** — stops just before this value) | *Required* |
| `step` | Increment/decrement per step (cannot be `0`) | `1` |

```python
print("--- range(start, stop, step) ---")
print("range(10)          :", list(range(10)))
print("range(5, 10)       :", list(range(5, 10)))
print("range(0, 10, 2)    :", list(range(0, 10, 2)))        # even numbers
print("range(1, 10, 2)    :", list(range(1, 10, 2)))        # odd numbers
print("range(10, 1, -1)   :", list(range(10, 1, -1)))       # reverse
print("range(-10, -5)     :", list(range(-10, -5)))
print("range(-10, -5, 2)  :", list(range(-10, -5, 2)))
```

**Output:**

```text
--- range(start, stop, step) ---
range(10)          : [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
range(5, 10)       : [5, 6, 7, 8, 9]
range(0, 10, 2)    : [0, 2, 4, 6, 8]
range(1, 10, 2)    : [1, 3, 5, 7, 9]
range(10, 1, -1)   : [10, 9, 8, 7, 6, 5, 4, 3, 2]
range(-10, -5)     : [-10, -9, -8, -7, -6]
range(-10, -5, 2)  : [-10, -8, -6]
```

---

<a id="looping-statements"></a>
## 3. Looping Statements

### 1. `while` Loop
Repeats a block **as long as** a condition remains `True` (ideal when you don't know the exact number of iterations in advance).
> ⚠️ Always update the loop variable inside a `while` loop to avoid an **infinite loop**.

### 2. `for` Loop
Iterates over a sequence (such as `range()`, a string, or a list) a fixed number of times.
> 💡 In Python, the loop variable remains accessible **after** the loop ends (`for i in range(1, 6): pass` $\rightarrow$ `i` is `5`).

```python
print("--- while loop: 1..10 ---")
i = 1
while i <= 10:
    print(i, end=" ")
    i += 1
print()

print("\n--- while loop: 10..1 (descending) ---")
i = 10
while i >= 1:
    print(i, end=" ")
    i -= 1
print()
```

**Output:**

```text
--- while loop: 1..10 ---
1 2 3 4 5 6 7 8 9 10 

--- while loop: 10..1 (descending) ---
10 9 8 7 6 5 4 3 2 1 
```

```python
print("--- while loop: even numbers (two ways) ---")
i = 2
while i <= 10:
    print(i, end=" ")
    i += 2
print()

i = 1
while i <= 10:
    if i % 2 == 0:
        print(i, end=" ")
    i += 1
print()
```

**Output:**

```text
--- while loop: even numbers (two ways) ---
2 4 6 8 10 
2 4 6 8 10 
```

```python
print("--- for loop: 1..10 & 10..1 ---")
for i in range(1, 11):
    print(i, end=" ")
print()

for i in range(10, 0, -1):
    print(i, end=" ")
print()

print("\n--- for loop: iterating directly over text ---")
for ch in "Python":
    print(ch, end="-")
print()
```

**Output:**

```text
--- for loop: 1..10 & 10..1 ---
1 2 3 4 5 6 7 8 9 10 
10 9 8 7 6 5 4 3 2 1 

--- for loop: iterating directly over text ---
P-y-t-h-o-n-
```

```python
print("--- for loop: even numbers & post-loop variable ---")
for i in range(2, 11, 2):
    print(i, end=" ")
print()

for i in range(1, 6):
    pass
print("i after the loop =", i)          # 5
```

**Output:**

```text
--- for loop: even numbers & post-loop variable ---
2 4 6 8 10 
i after the loop = 5
```

---

<a id="jumping-statements"></a>
## 4. Loop Control (`break`, `continue`, and `else`)

* **`break`:** Exits the loop **immediately**.
* **`continue`:** **Skips** the remaining code in the current iteration and jumps to the next iteration.
* **`else` on a loop:** Runs **only if** the loop finished naturally without hitting a `break` statement.

```python
print("--- break: stop the loop at 5 ---")
for i in range(1, 11):
    if i == 5:
        break
    print(i, end=" ")
print()

print("\n--- continue: skip 3, 5, and 7 ---")
for i in range(1, 11):
    if i in (3, 5, 7):
        continue
    print(i, end=" ")
print()

print("\n--- for...else: runs when no break occurs ---")
for i in range(1, 4):
    print(i, end=" ")
else:
    print("-> Loop finished without break!")
```

**Output:**

```text
--- break: stop the loop at 5 ---
1 2 3 4 

--- continue: skip 3, 5, and 7 ---
1 2 4 6 8 9 10 

--- for...else: runs when no break occurs ---
1 2 3 -> Loop finished without break!
```
