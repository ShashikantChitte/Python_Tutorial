# Formatting Output

> [← Course index](../README.md)

Python offers multiple ways to format text and variables inside `print()`, along with escape characters for layout control.

| Method | Syntax Example | Notes |
| :--- | :--- | :--- |
| **Comma-separated** | `print(name, age)` | Adds a space between items automatically |
| **`%` Operator** | `"Name: %s" % name` | Older C-style formatting (`%s` str, `%d` int, `%f`/`%g` float) |
| **`.format()` Method** | `"Name: {}".format(name)` | Supports positional `{0}` and empty `{}` placeholders |
| **`f-strings` (Recommended)** | `f"Name: {name}"` | Modern, fastest, and easiest to read (Python 3.6+) |

### Common Escape Characters & `print()` Options
* `\n` — Moves output to a **new line**.
* `\t` — Inserts a **tab space**.
* `sep` & `end` — Control what separates multiple arguments (default `' '`) and what prints at the end (default `'\n'`).

```python
print("--- Formatting Output ---")
name, age, sal = "John", 30, 50000.50

# 1. Basic comma separation
print(name, age, sal)

# 2. % formatting (legacy)
print("Name:%s  Age:%d  Salary:%g" % (name, age, sal))

# 3. .format() method
print("Name:{} Age:{} Salary:{}".format(name, age, sal))
print("Salary:{2} Age:{1} Name:{0}".format(name, age, sal))

# 4. f-strings (modern & recommended)
print(f"Name:{name} Age:{age} Salary:{sal}")
print(f"Formatted Salary: {sal:.2f}")   # .2f forces 2 decimal places

# 5. Escape characters & print() parameters
print("Welcome to\nPython")              # \n -> new line
print("Welcome\tto Python")            # \t -> tab space
print("2026", "11", "18", sep="-")       # custom separator
print("Hello", end=" ")                  # stay on the same line
print("World!")
```

**Output:**

```text
--- Formatting Output ---
John 30 50000.5
Name:John  Age:30  Salary:50000.5
Name:John Age:30 Salary:50000.5
Salary:50000.5 Age:30 Name:John
Name:John Age:30 Salary:50000.5
Formatted Salary: 50000.50
Welcome to
Python
Welcome	to Python
2026-11-18
Hello World!
```
