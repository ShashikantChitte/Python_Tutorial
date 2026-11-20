# Python Modules

> [← Course index](../../README.md)

## What is a module?

- A module is simply a **Python file (`.py`)** that contains functions, classes, or variables.
- Modules help organise code and **reuse** it across multiple programs.
- Python includes many **built-in modules** (like `math`, `random`, `datetime`), and you can also create your own **custom modules**.

## Files Used in This Notebook

The example modules live in the `example_modules` folder next to this notebook:

```
Modules and Packages/
├── modules.ipynb
└── example_modules/
    ├── operations.py   ← Example 1 (functions)
    ├── animal.py       ← Example 2 (name clash)
    ├── bird.py         ← Example 2 (name clash)
    ├── a.py            ← Example 3 (classes)
    └── b.py            ← Example 3 (classes)
```

> 💡 **Self-Contained Setup:** The first code cell below automatically creates the `example_modules` folder and its 5 `.py` files if they don't already exist, and adds the folder to `sys.path` so every example runs smoothly anywhere!

```python
import os
import sys

os.makedirs("example_modules", exist_ok=True)

helper_files = {
    "operations.py": '"""Helper module for Example 1."""\ndef add(num1, num2):\n    print(num1 + num2)\n\ndef mul(num1, num2):\n    print(num1 * num2)\n',
    "animal.py": '"""Helper module for Example 2."""\ndef fly():\n    print("Animal Cant fly")\n\ndef color():\n    print("Animal is Black")\n',
    "bird.py": '"""Helper module for Example 2."""\ndef fly():\n    print("Bird Can Fly")\n\ndef color():\n    print("Bird is Green")\n',
    "a.py": '"""Helper module for Example 3."""\nclass Animal:\n    def display(self):\n        print("I like Cow")\n',
    "b.py": '"""Helper module for Example 3."""\nclass Bird:\n    def display(self):\n        print("I like Parrot")\n',
}

for fname, content in helper_files.items():
    fpath = os.path.join("example_modules", fname)
    if not os.path.exists(fpath):
        with open(fpath, "w", encoding="utf-8") as f:
            f.write(content)

if "example_modules" not in sys.path:
    sys.path.append("example_modules")

print("Ready! Helper modules in 'example_modules':", sorted(os.listdir("example_modules")))
```

**Output:**

```text
Ready! Helper modules in 'example_modules': ['a.py', 'animal.py', 'b.py', 'bird.py', 'greetings.py', 'operations.py']
```

## Import Styles Cheat Sheet

| Syntax | How to Call | Best For |
|--------|-------------|----------|
| `import mod` | `mod.func()` | Avoiding name clashes (recommended) |
| `import mod as m` | `m.func()` | Shortening long module names (e.g., `import numpy as np`) |
| `from mod import func` | `func()` | Using only 1–2 specific items |
| `from mod import func as f` | `f()` | Renaming an item to prevent name conflicts |
| `from mod import *` | `func()` | Quick testing only (can cause silent name clashes) |

## Example 1: Using Functions from a Module (`operations.py`)

```python
def add(num1, num2):
    print(num1 + num2)

def mul(num1, num2):
    print(num1 * num2)
```

```python
print("--- Example 1: using functions from a module (operations.py) ---")

print("Approach 1: import the whole module")
import operations
operations.add(10, 20)
operations.mul(10, 20)

print("\nApproach 2: import with an alias (as)")
import operations as op
op.add(10, 20)
op.mul(10, 20)

print("\nApproach 3: import specific functions")
from operations import add, mul
add(10, 20)
mul(10, 20)

print("\nApproach 4: import everything")
from operations import *
add(10, 20)
mul(10, 20)
```

**Output:**

```text
--- Example 1: using functions from a module (operations.py) ---
Approach 1: import the whole module
30
200

Approach 2: import with an alias (as)
30
200

Approach 3: import specific functions
30
200

Approach 4: import everything
30
200
```

## Example 2: Same Function Names in Two Modules (`animal.py` vs `bird.py`)

- **`animal.py`** defines `fly()` and `color()`.
- **`bird.py`** also defines `fly()` and `color()`.

> ⚠️ **Warning:** If two modules have functions with the same name, `from module import *` causes silent overwrites — the **last import wins**. Use `import animal` and `import bird` (or aliases) to keep them separate.

```python
print("--- Example 2: same function names in two modules ---")

print("Approach 1: import the modules separately (no clash)")
import animal
import bird

animal.fly()
animal.color()
bird.fly()
bird.color()

print("\nApproach 2: import everything from both (the LAST import wins)")
from animal import *
print("after 'from animal import *' ->", end=" ")
fly()
color()

from bird import *
print("after 'from bird import *'   ->", end=" ")
fly()
color()
print("fly() now belongs to module:", fly.__module__, "(animal.fly was silently replaced)")
```

**Output:**

```text
--- Example 2: same function names in two modules ---
Approach 1: import the modules separately (no clash)
Animal Cant fly
Animal is Black
Bird Can Fly
Bird is Green

Approach 2: import everything from both (the LAST import wins)
after 'from animal import *' -> Animal Cant fly
Animal is Black
after 'from bird import *'   -> Bird Can Fly
Bird is Green
fly() now belongs to module: bird (animal.fly was silently replaced)
```

## Example 3: Classes in Different Modules (`a.py` & `b.py`)

- **`a.py`** defines `class Animal`.
- **`b.py`** defines `class Bird`.

```python
print("--- Example 3: classes in different modules ---")

print("Approach 1: import the modules")
import a
import b
obj1 = a.Animal()
obj1.display()
obj2 = b.Bird()
obj2.display()

print("\nApproach 2: import the classes directly")
from a import Animal
from b import Bird
obj3 = Animal()
obj3.display()
obj4 = Bird()
obj4.display()
```

**Output:**

```text
--- Example 3: classes in different modules ---
Approach 1: import the modules
I like Cow
I like Parrot

Approach 2: import the classes directly
I like Cow
I like Parrot
```

## Bonus: Built-in Modules & `dir()`

- Python ships with a huge Standard Library (`math`, `random`, `datetime`, `os`, etc.).
- Use the built-in `dir(module_name)` function to inspect all functions and variables inside any module.

```python
print("--- Built-in module example: math ---")
import math

print("Square root of 25 :", math.sqrt(25))
print("Value of pi       :", math.pi)
print("Custom functions in 'operations':", [item for item in dir(operations) if not item.startswith("__")])
```

**Output:**

```text
--- Built-in module example: math ---
Square root of 25 : 5.0
Value of pi       : 3.141592653589793
Custom functions in 'operations': ['add', 'mul']
```

## Understanding `if __name__ == "__main__":`

- Every Python module has a special built-in variable `__name__`.
- When a file is **run directly**, `__name__` is `"__main__"`.
- When a file is **imported** into another file, `__name__` is the **module's filename** (e.g., `"operations"`).
- Putting test code inside `if __name__ == "__main__":` prevents it from running automatically when someone imports your module.

```python
print("Current notebook __name__ :", __name__)
print("Imported module __name__  :", operations.__name__)
```

**Output:**

```text
Current notebook __name__ : __main__
Imported module __name__  : operations
```

## Hands-On Practice Exercises

Try solving these in the code cell below before checking the solutions!

1. **Built-in `math` & `random`:** Import `math` with the alias `m` and print `m.factorial(5)`. Also import `randint` from `random` and print a random number between `1` and `10`.
2. **Create & Import Your Own Module:** Write a small module `example_modules/greetings.py` containing a function `welcome(name)` that prints `"Welcome, <name>!"`, then import and call it.
3. **Resolve Name Clashes:** Import `fly` from `animal` as `animal_fly` and `fly` from `bird` as `bird_fly`, then call both without conflict.

```python
# 1. Built-in math & random
import math as m
from random import randint

print("1. Factorial of 5:", m.factorial(5))
print("   Random (1-10) :", randint(1, 10))

# 2. Create & Import Your Own Module
with open("example_modules/greetings.py", "w", encoding="utf-8") as f:
    f.write('def welcome(name):\n    print(f"Welcome, {name}!")\n')

from greetings import welcome
welcome("Student")

# 3. Resolve Name Clashes using aliases
from animal import fly as animal_fly
from bird import fly as bird_fly

animal_fly()
bird_fly()
```

**Output:**

```text
1. Factorial of 5: 120
   Random (1-10) : 5
Welcome, Student!
Animal Cant fly
Bird Can Fly
```

## Key Takeaways

- A **module** is a `.py` file containing reusable functions, classes, and variables.
- Prefer `import module` or `import module as alias` over `from module import *` to prevent silent name overwrites.
- Use `dir(module)` to explore what a module provides.
- Use `if __name__ == "__main__":` to keep test code from running when a module is imported.
