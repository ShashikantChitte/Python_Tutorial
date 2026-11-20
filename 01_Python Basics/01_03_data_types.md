# Data Types

> [← Course index](../README.md)

Every value in Python has a data type that defines what kind of data it holds and what operations can be performed on it.

## Basic (Scalar) Data Types

| Type | Meaning | Examples |
| :--- | :--- | :--- |
| `int` | Whole numbers (positive, negative, or zero) | `10`, `-5`, `0` |
| `float` | Decimal (floating-point) numbers | `3.14`, `-0.5`, `10.0` |
| `str` | Text / sequence of characters | `"Hello"`, `'Python'`, `'A'` |
| `bool` | Logical values | `True`, `False` |
| `NoneType` | Represents the absence of a value | `None` |

> 💡 **Notes:**
> * Python has **no separate character (`char`) type**—a single letter like `'A'` is simply a `str` of length 1.
> * Use `type(value)` to check a variable's exact data type, or `isinstance(value, int)` to verify it.

```python
print("--- Basic Data Types ---")
x = 100          # int
y = 10.5         # float
s = "Python"     # str
c = 'A'          # str (single character is still a string)
flag = True      # bool
empty = None     # NoneType

for value in (x, y, s, c, flag, empty):
    print(f"{value!r:10} -> {type(value)}")

# Verifying type with isinstance()
print("\nIs x an integer?", isinstance(x, int))
```

**Output:**

```text
--- Basic Data Types ---
100        -> <class 'int'>
10.5       -> <class 'float'>
'Python'   -> <class 'str'>
'A'        -> <class 'str'>
True       -> <class 'bool'>
None       -> <class 'NoneType'>

Is x an integer? True
```
