# Chapter 02 - Variables in Python

> [← Course index](../README.md)

### Table of Contents
1. [Variables](#variables)
2. [Variable Naming Conventions](#variable-naming-conventions)
3. [Dynamic Typing](#dynamic-typing)
4. [More Ways to Assign Values](#more-ways-to-assign-values)
5. [Deleting Variables](#deleting-variables)

---

<a id="variables"></a>
## 1. Variables

Variables act as named containers for storing data values in memory.

* **No Explicit Declaration:** A variable is created the moment you first assign a value to it using `=`.
* **Case-Sensitive:** `name`, `Name`, and `NAME` are treated as three completely different variables.

```python
print("--- Variables (no declaration needed) ---")
x = 10          # integer
name = "John"   # string
print(x, name)
print(type(x), type(name))
```

**Output:**

```text
--- Variables (no declaration needed) ---
10 John
<class 'int'> <class 'str'>
```

```python
print("--- Variable names are case-sensitive ---")
name = "john"
Name = "JOHN"
print("name =", name, "| Name =", Name, "| same variable?", name == Name)
```

**Output:**

```text
--- Variable names are case-sensitive ---
name = john | Name = JOHN | same variable? False
```

---

<a id="variable-naming-conventions"></a>
## 2. Variable Naming Conventions

Variable names can be short (`x`, `y`) or descriptive (`age`, `car_name`).

### Naming Rules (Mandatory)
* Must start with a **letter (`a-z`, `A-Z`) or underscore (`_`)**.
* **Cannot** start with a number.
* Can only contain letters, numbers, and underscores (`A-z`, `0-9`, and `_`).
* Are **case-sensitive**.
* **Cannot** be a Python keyword (e.g., `if`, `class`, `for`).

**Valid examples:**
```python
myvar = "John"
my_var = "John"
_my_var = "John"
myVar = "John"
MYVAR = "John"
myvar2 = "John"
```

**Invalid examples:**
```python
2myvar = "John"   # starts with number
my-var = "John"   # contains hyphen
my var = "John"   # contains space
```

> 💡 **Best Practice (PEP 8 Style):** Use `snake_case` (lowercase words separated by underscores) for regular variables like `user_age`, and `UPPER_CASE` for constants like `PI = 3.14`.

```python
print("--- Valid variable names ---")
myvar = "John"
my_var = "John"
_my_var = "John"
myVar = "John"
MYVAR = "John"
myvar2 = "John"
print(myvar, my_var, _my_var, myVar, MYVAR, myvar2)
```

**Output:**

```text
--- Valid variable names ---
John John John John John John
```

```python
import keyword

print("--- Checking valid vs. invalid variable names ---")
# 2myvar = "John"   # SyntaxError: starts with a number
# my-var = "John"   # SyntaxError: contains a hyphen
# my var = "John"   # SyntaxError: contains a space

candidates = ["myvar", "my_var", "_my_var", "myVar", "MYVAR", "myvar2",
              "2myvar", "my-var", "my var", "class"]

for candidate in candidates:
    is_valid = candidate.isidentifier() and not keyword.iskeyword(candidate)
    print(f"{candidate!r:10} valid variable name? {is_valid}")
```

**Output:**

```text
--- Checking valid vs. invalid variable names ---
'myvar'    valid variable name? True
'my_var'   valid variable name? True
'_my_var'  valid variable name? True
'myVar'    valid variable name? True
'MYVAR'    valid variable name? True
'myvar2'   valid variable name? True
'2myvar'   valid variable name? False
'my-var'   valid variable name? False
'my var'   valid variable name? False
'class'    valid variable name? False
```

---

<a id="dynamic-typing"></a>
## 3. Dynamic Typing

* Python is a **dynamically typed** language.
* You do not need to declare a variable's data type—it is determined automatically **at runtime**.
* A variable can even change its data type after it has been set.

```python
print("--- Dynamic typing: the type is decided at runtime ---")
x = 10
print(x, type(x))        # int

x = "Hello"
print(x, type(x))        # now a str
```

**Output:**

```text
--- Dynamic typing: the type is decided at runtime ---
10 <class 'int'>
Hello <class 'str'>
```

---

<a id="more-ways-to-assign-values"></a>
## 4. More Ways to Assign Values

Python provides clean shortcuts for working with multiple variables:
* **Multiple Assignment:** Assign different values to multiple variables on a single line.
* **Chained Assignment:** Assign the exact same value to multiple variables at once.
* **Swapping Variables:** Exchange values between two variables directly without needing a temporary variable.

```python
print("--- Assigning multiple values at once ---")
a, b, c = 10, 20, "welcome"
print(a, b, c)
```

**Output:**

```text
--- Assigning multiple values at once ---
10 20 welcome
```

```python
print("--- Assigning the same value to multiple variables ---")
a = b = c = 100
print(a, b, c)
```

**Output:**

```text
--- Assigning the same value to multiple variables ---
100 100 100
```

```python
print("--- Swapping two variables ---")
a, b = 10, 20
print("before:", a, b)

a, b = b, a
print("after: ", a, b)
```

**Output:**

```text
--- Swapping two variables ---
before: 10 20
after:  20 10
```

---

<a id="deleting-variables"></a>
## 5. Deleting Variables

* Use the `del` keyword to delete a variable reference from memory.
* Trying to access a deleted variable raises a `NameError` unless it is reassigned.

```python
print("--- Deleting variables ---")
a = 100
b = 200

del a
print("b still works ->", b)
print("Is 'a' still defined?", "a" in globals())
# print(a)   # NameError: name 'a' is not defined
```

**Output:**

```text
--- Deleting variables ---
b still works -> 200
Is 'a' still defined? False
```
