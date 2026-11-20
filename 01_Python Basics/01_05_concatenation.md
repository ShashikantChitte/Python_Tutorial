# Concatenation

> [← Course index](../README.md)

In Python, the `+` operator behaves differently depending on the data types involved:

* **Numbers (`int`, `float`, `bool`):** Performs mathematical **addition** (`True` acts as `1`, `False` acts as `0`).
* **Strings (`str`):** Performs **concatenation** (joins strings end-to-end without adding a space).
* **Mixed Types (`int` + `str`):** Raises a `TypeError`. Convert the number using `str()` or use an `f-string` instead.
* **Bonus (`*` with strings):** Repeats a string multiple times (e.g., `"Hi! " * 3`).

```python
print("--- Addition vs. Concatenation (+) ---")
# Numeric addition
print(10 + 10)                  # 20
print(10.5 + 12.0)              # 22.5
print(10 + 10.5)                # 20.5
print(True + 5)                 # 6  (True = 1)
print(False + 5)                # 5  (False = 0)
print(True + True)              # 2

# String concatenation & repetition
print("Hello" + " " + "Python") # Hello Python
print("Ha" * 3)                 # HaHaHa

# Mixing int and str
try:
    print(10 + "welcome")       # TypeError: unsupported operand type(s)
except TypeError as err:
    print("TypeError:", err)

# Safe ways to combine numbers and text:
print(str(10) + " welcome")
print(f"{10} welcome")
```

**Output:**

```text
--- Addition vs. Concatenation (+) ---
20
22.5
20.5
6
5
2
Hello Python
HaHaHa
TypeError: unsupported operand type(s) for +: 'int' and 'str'
10 welcome
10 welcome
```
