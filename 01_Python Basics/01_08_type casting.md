# Type Casting

> [← Course index](../README.md)

Type casting (type conversion) is the process of converting a value from one data type to another using built-in constructor functions:

| Function | Converts To | Key Behavior |
| :--- | :--- | :--- |
| `int()` | Integer | Converts numeric strings (`"10"`) or truncates decimals (`3.9` $\rightarrow$ `3`) |
| `float()` | Floating-point | Adds `.0` to integers (`5` $\rightarrow$ `5.0`) or parses decimal strings (`"3.14"`) |
| `str()` | String | Converts any data type into text (`100` $\rightarrow$ `"100"`) |
| `bool()` | Boolean | `0`, `0.0`, `""`, and `None` become `False`; almost everything else becomes `True` |

> ⚠️ **Note:** Converting non-numeric text like `int("abc")` or a float string directly like `int("3.14")` raises a `ValueError`.

```python
print("--- Type Casting ---")
# str -> int
x = "10"
y = int(x)
print(repr(x), "->", repr(y), type(y))

# str -> float, int -> str, float -> int
print(float("3.14"), "|", str(100) + "!", "|", int(3.9), "(int() truncates decimals)")

# Converting to bool (Truthy vs. Falsy)
print("bool(0):", bool(0), "| bool(1):", bool(1))
print("bool(''):", bool(""), "| bool('Python'):", bool("Python"))

# int("abc")   # ValueError: invalid literal for int() with base 10: 'abc'
```

**Output:**

```text
--- Type Casting ---
'10' -> 10 <class 'int'>
3.14 | 100! | 3 (int() truncates decimals)
bool(0): False | bool(1): True
bool(''): False | bool('Python'): True
```
