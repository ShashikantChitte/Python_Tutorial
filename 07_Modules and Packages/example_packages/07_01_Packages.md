# Python Packages

> [← Course index](../../README.md)

## What is a package?

- A package is a **collection of modules** (Python files) grouped inside a folder.
- **Module** = one `.py` file, while **Package** = a *folder* containing multiple `.py` files (and optionally sub-packages).
- Packages help organise related modules together in medium and large projects.
- A package folder usually includes an **`__init__.py`** file (even an empty one) to mark it as a package and optionally expose selected functions/classes at the package level.

## Files Used in This Notebook

All three examples live inside the `example_packages` folder next to this notebook:

```
Modules and Packages/
├── packages.ipynb
└── example_packages/
    ├── example1_single_package/
    │   ├── client.py
    │   └── pack1/              (__init__.py, module1.py, module2.py)
    ├── example2_nested_packages/
    │   ├── client.py
    │   └── pack1/              (__init__.py, module1.py)
    │       └── pack2/          (__init__.py, module2.py)
    └── example3_classes_across_packages/
        ├── pack1/              (__init__.py, emp.py → Employee class)
        ├── pack2/              (__init__.py, stu.py → Student class)
        └── pack3/              (client.py)
```

> 💡 **Self-Contained Setup:** Each example reuses names like `pack1`, `module1`, and `module2`. The setup cell below automatically creates all three package folders and their `.py` files if they don't already exist, and defines `run_example()` to run each `client.py` in its **own Python process** so Python's module cache (`sys.modules`) never mixes them up!

```python
import os
import subprocess
import sys

# Auto-create example_packages folder structure so this notebook runs anywhere
package_files = {
    # Example 1: Single Package
    "example1_single_package/pack1/__init__.py": "",
    "example1_single_package/pack1/module1.py": 'def display():\n    print("This is display function from module1")\n',
    "example1_single_package/pack1/module2.py": 'def show():\n    print("This is show function from module2")\n',
    "example1_single_package/client.py": (
        'import os, sys\n'
        'here = os.path.dirname(os.path.abspath(__file__))\n'
        'sys.path.append(os.path.join(here, "pack1"))\n\n'
        '# Approach 1: import whole modules\n'
        'import module1, module2\n'
        'module1.display()\n'
        'module2.show()\n\n'
        '# Approach 2: import everything\n'
        'from module1 import *\n'
        'from module2 import *\n'
        'display()\n'
        'show()\n\n'
        '# Bonus: dotted package import (from project root)\n'
        'sys.path.append(here)\n'
        'from pack1.module1 import display as dotted_display\n'
        'dotted_display()\n'
    ),
    # Example 2: Nested Packages
    "example2_nested_packages/pack1/__init__.py": "",
    "example2_nested_packages/pack1/module1.py": 'def display():\n    print("This is display function from module1")\n',
    "example2_nested_packages/pack1/pack2/__init__.py": "",
    "example2_nested_packages/pack1/pack2/module2.py": 'def show():\n    print("This is show function from module2")\n',
    "example2_nested_packages/client.py": (
        'import os, sys\n'
        'here = os.path.dirname(os.path.abspath(__file__))\n\n'
        'sys.path.append(os.path.join(here, "pack1"))\n'
        'from module1 import *\n\n'
        'sys.path.append(os.path.join(here, "pack1", "pack2"))\n'
        'from module2 import *\n\n'
        'display()\n'
        'show()\n\n'
        '# Bonus: dotted sub-package import\n'
        'sys.path.append(here)\n'
        'from pack1.pack2.module2 import show as dotted_show\n'
        'dotted_show()\n'
    ),
    # Example 3: Classes Across Packages
    "example3_classes_across_packages/pack1/__init__.py": "",
    "example3_classes_across_packages/pack1/emp.py": (
        'class Employee:\n'
        '    def __init__(self, eid, ename, sal):\n'
        '        self.eid = eid\n'
        '        self.ename = ename\n'
        '        self.sal = sal\n\n'
        '    def displayemp(self):\n'
        '        print(f"empid:{self.eid} empname:{self.ename} empsal:{self.sal}")\n'
    ),
    "example3_classes_across_packages/pack2/__init__.py": "",
    "example3_classes_across_packages/pack2/stu.py": (
        'class Student:\n'
        '    def __init__(self, sid, sname, sgrad):\n'
        '        self.sid = sid\n'
        '        self.sname = sname\n'
        '        self.sgrad = sgrad\n\n'
        '    def displaystu(self):\n'
        '        print(f"stuid:{self.sid} stuname:{self.sname} stugrade:{self.sgrad}")\n'
    ),
    "example3_classes_across_packages/pack3/client.py": (
        'import os, sys\n'
        'root = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))\n\n'
        'sys.path.append(os.path.join(root, "pack1"))\n'
        'from emp import Employee\n'
        'e = Employee(101, "Scott", 40000)\n'
        'e.displayemp()\n\n'
        'sys.path.append(os.path.join(root, "pack2"))\n'
        'from stu import Student\n'
        's = Student(141, "David", "A")\n'
        's.displaystu()\n\n'
        '# Bonus: dotted package import\n'
        'sys.path.append(root)\n'
        'from pack1.emp import Employee as PkgEmployee\n'
        'PkgEmployee(102, "John", 55000).displayemp()\n'
    )
}

for rel_path, content in package_files.items():
    full_path = os.path.join("example_packages", rel_path)
    os.makedirs(os.path.dirname(full_path), exist_ok=True)
    if not os.path.exists(full_path):
        with open(full_path, "w", encoding="utf-8") as f:
            f.write(content)


def run_example(relative_path):
    """Run one example client in its own Python process and show what it prints."""
    path = os.path.join("example_packages", relative_path)
    result = subprocess.run([sys.executable, path], capture_output=True, text=True)
    print(result.stdout, end="")
    if result.returncode != 0:
        print(result.stderr)

print("Ready! Package examples created in 'example_packages/'.")
```

**Output:**

```text
Ready! Package examples created in 'example_packages/'.
```

## Example 1: Importing Modules from a Single Package

```
pack1/
├── __init__.py
├── module1.py   → display()
├── module2.py   → show()
└── client.py
```

**`module1.py`**
```python
def display():
    print("This is display function from module1")
```

**`module2.py`**
```python
def show():
    print("This is show function from module2")
```

**`client.py`**
```python
import sys
sys.path.append("C:/Automation/PWPython/pack1")

# Approach 1: import whole modules
import module1, module2
module1.display()
module2.show()

# Approach 2: import everything
from module1 import *
from module2 import *
display()
show()
```

**Key points**
- `sys.path.append(path)` tells Python **where to find** the package.
- You can import whole modules or specific functions.

> 💡 The files in `example_packages` compute paths dynamically using `os.path.dirname(__file__)` instead of a hard-coded `C:/...` path so they work on any OS.

```python
print("--- Running Example 1: Single Package ---")
run_example("example1_single_package/client.py")
```

**Output:**

```text
--- Running Example 1: Single Package ---
This is display function from module1
This is show function from module2
This is display function from module1
This is show function from module2
This is display function from module1
```

## Example 2: Importing from Nested Packages (Sub-Packages)

```
pack1/
├── __init__.py
├── module1.py       → display()
└── pack2/
    ├── __init__.py
    └── module2.py   → show()
```

**`client.py`**
```python
import sys

sys.path.append("C:/Automation/PWPython/pack1")         # import from pack1
from module1 import *

sys.path.append("C:/Automation/PWPython/pack1/pack2")   # import from pack2 inside pack1
from module2 import *

display()
show()
```

**Key points**
- You can nest packages inside other packages (**sub-packages**).
- Each sub-package acts like a sub-folder with its own modules.

```python
print("--- Running Example 2: Nested Packages ---")
run_example("example2_nested_packages/client.py")
```

**Output:**

```text
--- Running Example 2: Nested Packages ---
This is display function from module1
This is show function from module2
This is show function from module2
```

## Example 3: Importing Classes from Different Packages

```
pack1/
└── emp.py   → Employee class
pack2/
└── stu.py   → Student class
pack3/
└── client.py
```

**`emp.py`**
```python
class Employee:
    def __init__(self, eid, ename, sal):
        self.eid = eid
        self.ename = ename
        self.sal = sal

    def displayemp(self):
        print(f"empid:{self.eid} empname:{self.ename} empsal:{self.sal}")
```

**`stu.py`**
```python
class Student:
    def __init__(self, sid, sname, sgrad):
        self.sid = sid
        self.sname = sname
        self.sgrad = sgrad

    def displaystu(self):
        print(f"stuid:{self.sid} stuname:{self.sname} stugrade:{self.sgrad}")
```

**`client.py`**
```python
import sys

sys.path.append("C:/Automation/PWPython/pack1")
from emp import Employee
e = Employee(101, 'Scott', 40000)
e.displayemp()

sys.path.append("C:/Automation/PWPython/pack2")
from stu import Student
s = Student(141, 'David', 'A')
s.displaystu()
```

**Key points**
- Classes can be imported from different packages into one client program.
- Keeps large projects modular and clean.

```python
print("--- Running Example 3: Classes Across Packages ---")
run_example("example3_classes_across_packages/pack3/client.py")
```

**Output:**

```text
--- Running Example 3: Classes Across Packages ---
empid:101 empname:Scott empsal:40000
stuid:141 stuname:David stugrade:A
empid:102 empname:John empsal:55000
```

## Bonus: Dotted Package Imports & `__init__.py` (Recommended)

Instead of adding every sub-folder to `sys.path`, add the **project root** once and use dot notation:

```python
from pack1.module1 import display
from pack1.pack2.module2 import show
from pack1.emp import Employee
```

### What does `__init__.py` actually do?
1. Marks a folder as a Python package.
2. Runs automatically the first time the package is imported.
3. Can re-export key functions/classes so users can import directly from the package name (e.g., `from pack1 import display`).

## Hands-On Practice Exercises

Try solving these in the code cell below before checking the solutions!

1. **Build Your Own Package (`math_pkg`):** Create a folder `example_packages/my_practice/math_pkg` with `__init__.py`, `basic.py` (with `add(a, b)`), and `advanced.py` (with `power(a, b)`).
2. **Dotted Import:** Add `example_packages/my_practice` to `sys.path` and import `add` and `power` using dotted package syntax (`from math_pkg.basic import add`).
3. **Package-Level Export:** Put `from .basic import add` inside `math_pkg/__init__.py` so you can import `add` directly via `from math_pkg import add`.

```python
import os, sys

base = os.path.join("example_packages", "my_practice")
pkg_dir = os.path.join(base, "math_pkg")
os.makedirs(pkg_dir, exist_ok=True)

# 1. Create package files + 3. Expose 'add' in __init__.py
with open(os.path.join(pkg_dir, "__init__.py"), "w", encoding="utf-8") as f:
    f.write("from .basic import add\n")

with open(os.path.join(pkg_dir, "basic.py"), "w", encoding="utf-8") as f:
    f.write("def add(a, b):\n    return a + b\n")

with open(os.path.join(pkg_dir, "advanced.py"), "w", encoding="utf-8") as f:
    f.write("def power(a, b):\n    return a ** b\n")

# 2. Dotted import
if base not in sys.path:
    sys.path.append(base)

from math_pkg.basic import add
from math_pkg.advanced import power
from math_pkg import add as pkg_add   # works because of __init__.py!

print("add(5, 3)     :", add(5, 3))
print("power(2, 4)   :", power(2, 4))
print("pkg_add(10, 2):", pkg_add(10, 2))
```

**Output:**

```text
add(5, 3)     : 8
power(2, 4)   : 16
pkg_add(10, 2): 12
```

## Summary

| Concept | Description |
|---------|-------------|
| **Module** | A single `.py` file containing Python code |
| **Package** | A folder containing multiple modules and an `__init__.py` file |
| **Sub-package** | A package nested inside another package (`pack1.pack2`) |
| **Dotted Import** | `from package.subpackage.module import func` (recommended in real projects) |
| **`sys.path.append()`** | Tells Python where to search for custom package folders |
