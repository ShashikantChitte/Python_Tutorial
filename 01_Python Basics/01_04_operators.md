# Operators

> [← Course index](../README.md)

Operators are special symbols used to perform operations on variables and values.

| Category | Operators | Result |
| :--- | :--- | :--- |
| **Arithmetic** | `+`, `-`, `*`, `/`, `%`, `**` (power), `//` (floor division) | Number (`int` or `float`) |
| **Assignment** | `=`, `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=` | Updates variable value |
| **Comparison** | `==`, `!=`, `>`, `<`, `>=`, `<=` | `True` / `False` |
| **Logical** | `and`, `or`, `not` | `True` / `False` |

> 💡 **Division Tip:** Standard division (`/`) **always** returns a `float` (e.g., `4 / 2` gives `2.0`), whereas floor division (`//`) drops the decimal part.

### Truth Table for Logical Operators

| `a` | `b` | `a and b` | `a or b` | `not a` |
| :---: | :---: | :---: | :---: | :---: |
| `True` | `True` | `True` | `True` | `False` |
| `True` | `False` | `False` | `True` | `False` |
| `False` | `True` | `False` | `True` | `True` |
| `False` | `False` | `False` | `False` | `True` |

```python
print("--- Arithmetic Operators ---")
a, b = 5, 2
print("a + b  =", a + b)     # 7   (addition)
print("a - b  =", a - b)     # 3   (subtraction)
print("a * b  =", a * b)     # 10  (multiplication)
print("a / b  =", a / b)     # 2.5 (true division - always float)
print("a // b =", a // b)    # 2   (floor division - whole number)
print("a % b  =", a % b)     # 1   (modulus / remainder)
print("a ** b =", a ** b)    # 25  (exponentiation / power)
```

**Output:**

```text
--- Arithmetic Operators ---
a + b  = 7
a - b  = 3
a * b  = 10
a / b  = 2.5
a // b = 2
a % b  = 1
a ** b = 25
```

```python
print("--- Assignment Operators (Shortcuts) ---")
score = 10
score += 5    # same as: score = score + 5
print("After += 5:", score)

score *= 2    # same as: score = score * 2
print("After *= 2:", score)
```

**Output:**

```text
--- Assignment Operators (Shortcuts) ---
After += 5: 15
After *= 2: 30
```

```python
print("--- Comparison (Relational) Operators -> True/False ---")
a, b = 5, 10
print("a > b :", a > b)
print("a < b :", a < b)
print("a == b:", a == b)
print("a != b:", a != b)

b = 5
print("a <= b:", a <= b, "| a >= b:", a >= b)
```

**Output:**

```text
--- Comparison (Relational) Operators -> True/False ---
a > b : False
a < b : True
a == b: False
a != b: True
a <= b: True | a >= b: True
```

```python
print("--- Logical Operators ---")
p, q = True, False
print("p and q:", p and q)
print("p or q :", p or q)
print("not p  :", not p)
print("not q  :", not q)
print("(1 < 2) and (1 > 2):", (1 < 2) and (1 > 2))
print("(1 < 2) or  (1 > 2):", (1 < 2) or (1 > 2))
```

**Output:**

```text
--- Logical Operators ---
p and q: False
p or q : True
not p  : False
not q  : True
(1 < 2) and (1 > 2): False
(1 < 2) or  (1 > 2): True
```
