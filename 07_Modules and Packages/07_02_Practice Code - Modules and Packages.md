# Practice Code – Modules and Packages

> [← Course index](../README.md)

Practice with the standard library, import styles, creating your own modules and packages, the `__name__` guard and dynamic imports. Problems that need extra files create them in a temporary folder, so nothing is left behind.

**18 problems** · 7 Easy · 8 Medium · 3 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Use the math module](#1-use-the-math-module) | Easy |
| 2 | [Import specific names and use aliases](#2-import-specific-names-and-use-aliases) | Easy |
| 3 | [Generate reproducible random numbers](#3-generate-reproducible-random-numbers) | Easy |
| 4 | [Work with dates using datetime](#4-work-with-dates-using-datetime) | Easy |
| 5 | [Count items with collections.Counter](#5-count-items-with-collectionscounter) | Easy |
| 6 | [Use a deque as a fast queue](#6-use-a-deque-as-a-fast-queue) | Medium |
| 7 | [Generate combinations with itertools](#7-generate-combinations-with-itertools) | Medium |
| 8 | [Generate permutations with itertools](#8-generate-permutations-with-itertools) | Medium |
| 9 | [Build all test-data combinations with itertools.product](#9-build-all-test-data-combinations-with-itertoolsproduct) | Medium |
| 10 | [Work with file paths using pathlib](#10-work-with-file-paths-using-pathlib) | Easy |
| 11 | [Read environment variables and split file names with os](#11-read-environment-variables-and-split-file-names-with-os) | Easy |
| 12 | [Create your own module and import it](#12-create-your-own-module-and-import-it) | Medium |
| 13 | [Use the if __name__ == "__main__" guard](#13-use-the-if-__name__--__main__-guard) | Medium |
| 14 | [Create and import a package](#14-create-and-import-a-package) | Hard |
| 15 | [Import a module by name using importlib](#15-import-a-module-by-name-using-importlib) | Hard |
| 16 | [Reload a module after it changes](#16-reload-a-module-after-it-changes) | Hard |
| 17 | [Inspect which modules are loaded](#17-inspect-which-modules-are-loaded) | Medium |
| 18 | [Fall back gracefully when a module is missing](#18-fall-back-gracefully-when-a-module-is-missing) | Medium |

---

## 1. Use the math module

**Level:** Easy

**Problem:** Use `math` for square roots, constants and rounding.

**Explanation:** `import math` loads the module, and its functions and constants are accessed with the `math.` prefix.

```python
import math

print(math.sqrt(144))
print(math.pi)
print(math.ceil(4.2), math.floor(4.8))
print(math.factorial(5))

# ------------Expected Output---------

# 12.0
# 3.141592653589793
# 5 4
# 120
```

---

## 2. Import specific names and use aliases

**Level:** Easy

**Problem:** Import only the functions you need and rename a module to a shorter alias.

**Explanation:** `from module import name` brings a name into your namespace directly, and `as` gives it (or a whole module) a different name.

```python
from math import sqrt, pow as power
import statistics as stats

print(sqrt(81))
print(power(2, 10))
print(stats.mean([10, 20, 30, 40]))
print(stats.median([5, 1, 9, 3]))

# ------------Expected Output---------

# 9.0
# 1024.0
# 25
# 4.0
```

---

## 3. Generate reproducible random numbers

**Level:** Easy

**Problem:** Get random numbers that are the same every time the program runs.

**Explanation:** Calling `random.seed()` with a fixed value makes the generator deterministic, which is very useful for repeatable tests.

```python
import random

random.seed(42)
print(random.randint(1, 100))
print(random.randint(1, 100))

random.seed(42)
print(random.randint(1, 100))   # same as the first value

# ------------Expected Output---------

# 82
# 15
# 82
```

---

## 4. Work with dates using datetime

**Level:** Easy

**Problem:** Add days to a date, format it and count the days until another date.

**Explanation:** `timedelta` represents a duration that can be added to a `date`; subtracting two dates gives a `timedelta` whose `.days` is the difference. `strftime` formats the date.

```python
from datetime import date, timedelta

start = date(2026, 1, 31)

print(start + timedelta(days=30))
print(start.strftime("%d %B %Y, %A"))
print((date(2026, 12, 25) - start).days)

# ------------Expected Output---------

# 2026-03-02
# 31 January 2026, Saturday
# 328
```

---

## 5. Count items with collections.Counter

**Level:** Easy

**Problem:** Count how often each letter appears and list the most common ones.

**Explanation:** `Counter` is a dictionary subclass for counting. `most_common(n)` returns the `n` most frequent items with their counts.

```python
from collections import Counter

counts = Counter("mississippi")

print(counts)
print(counts.most_common(2))
print(counts["s"])

# ------------Expected Output---------

# Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
# [('i', 4), ('s', 4)]
# 4
```

---

## 6. Use a deque as a fast queue

**Level:** Medium

**Problem:** Add and remove items from both ends efficiently.

**Explanation:** `collections.deque` supports O(1) `append`/`popleft` on both ends, unlike a list where removing from the front is slow.

```python
from collections import deque

queue = deque(["task1", "task2"])
queue.append("task3")
queue.appendleft("urgent")

print(queue)
print(queue.popleft())
print(list(queue))

# ------------Expected Output---------

# deque(['urgent', 'task1', 'task2', 'task3'])
# urgent
# ['task1', 'task2', 'task3']
```

---

## 7. Generate combinations with itertools

**Level:** Medium

**Problem:** List every way to pick 2 items from a list where order does not matter.

**Explanation:** `itertools.combinations(items, r)` yields tuples of `r` items without repeats and without caring about order.

```python
from itertools import combinations

team = ["Ann", "Bob", "Cat", "Dan"]

for pair in combinations(team, 2):
    print(pair)

# ------------Expected Output---------

# ('Ann', 'Bob')
# ('Ann', 'Cat')
# ('Ann', 'Dan')
# ('Bob', 'Cat')
# ('Bob', 'Dan')
# ('Cat', 'Dan')
```

---

## 8. Generate permutations with itertools

**Level:** Medium

**Problem:** List every possible ordering of three items.

**Explanation:** `itertools.permutations` yields all orderings; for 3 items there are 3! = 6 of them.

```python
from itertools import permutations

for order in permutations(["A", "B", "C"]):
    print("".join(order))

# ------------Expected Output---------

# ABC
# ACB
# BAC
# BCA
# CAB
# CBA
```

---

## 9. Build all test-data combinations with itertools.product

**Level:** Medium

**Problem:** Create every combination of browser and operating system for a test matrix.

**Explanation:** `itertools.product` returns the Cartesian product of its inputs – ideal for data-driven test matrices.

```python
from itertools import product

browsers = ["chrome", "firefox"]
systems = ["windows", "linux", "mac"]

for browser, system in product(browsers, systems):
    print(f"{browser} on {system}")

# ------------Expected Output---------

# chrome on windows
# chrome on linux
# chrome on mac
# firefox on windows
# firefox on linux
# firefox on mac
```

---

## 10. Work with file paths using pathlib

**Level:** Easy

**Problem:** Build a path and read its name, stem, suffix and parent folder.

**Explanation:** `Path` objects join with `/` and expose useful properties, so no manual string slicing is needed.

```python
from pathlib import Path

report = Path("reports") / "2026" / "summary.final.txt"

print(report.name)
print(report.stem)
print(report.suffix)
print(report.parent.name)

# ------------Expected Output---------

# summary.final.txt
# summary.final
# .txt
# 2026
```

---

## 11. Read environment variables and split file names with os

**Level:** Easy

**Problem:** Read an environment variable with a default and split a file name from its extension.

**Explanation:** `os.getenv(name, default)` returns the default when the variable is not set. `os.path.splitext` separates the extension, and `os.path.basename` returns the last path component.

```python
import os

print(os.getenv("SOME_UNSET_VARIABLE", "default-value"))
print(os.path.splitext("report.final.pdf"))
print(os.path.basename("reports/2026/summary.txt"))

# ------------Expected Output---------

# default-value
# ('report.final', '.pdf')
# summary.txt
```

---

## 12. Create your own module and import it

**Level:** Medium

**Problem:** Write a small module to disk and import it like any other module.

**Explanation:** A module is just a `.py` file. Python finds it through `sys.path`, so adding its folder there makes `import calculator` work. Here the file is created in a temporary folder.

```python
import pathlib
import shutil
import sys
import tempfile

folder = tempfile.mkdtemp()
pathlib.Path(folder, "calculator.py").write_text(
    "PI = 3.14\n\n"
    "def add(a, b):\n"
    "    return a + b\n"
)

sys.path.insert(0, folder)
import calculator

print(calculator.add(2, 3))
print(calculator.PI)
print(calculator.__name__)

sys.path.remove(folder)
shutil.rmtree(folder)

# ------------Expected Output---------

# 5
# 3.14
# calculator
```

---

## 13. Use the if __name__ == "__main__" guard

**Level:** Medium

**Problem:** Make a file behave differently when it is imported and when it is run directly.

**Explanation:** When a file is imported, `__name__` is the module name; when it is run as a script it is `"__main__"`. Code under the guard therefore runs only when the file is executed directly.

```python
import pathlib
import runpy
import shutil
import sys
import tempfile

folder = tempfile.mkdtemp()
script = pathlib.Path(folder, "greeter.py")
script.write_text(
    'print("module loaded")\n\n'
    "def main():\n"
    '    print("running as a script")\n\n'
    'if __name__ == "__main__":\n'
    "    main()\n"
)

sys.path.insert(0, folder)
print("--- imported ---")
import greeter

print("--- run directly ---")
runpy.run_path(str(script), run_name="__main__")

sys.path.remove(folder)
shutil.rmtree(folder)

# ------------Expected Output---------

# --- imported ---
# module loaded
# --- run directly ---
# module loaded
# running as a script
```

---

## 14. Create and import a package

**Level:** Hard

**Problem:** Create a package folder with an `__init__.py` and import from it.

**Explanation:** A package is a folder containing `__init__.py`. The `__init__.py` can re-export names from its modules, so users can write `from shop import total` instead of knowing the inner file layout.

```python
import pathlib
import shutil
import sys
import tempfile

folder = tempfile.mkdtemp()
package = pathlib.Path(folder, "shop")
package.mkdir()
(package / "__init__.py").write_text("from .prices import total\n")
(package / "prices.py").write_text(
    "def total(prices, tax=0.18):\n"
    "    return round(sum(prices) * (1 + tax), 2)\n"
)

sys.path.insert(0, folder)
from shop import total
import shop.prices

print(total([100, 200, 300]))
print(shop.prices.total([100], tax=0))

sys.path.remove(folder)
shutil.rmtree(folder)

# ------------Expected Output---------

# 708.0
# 100
```

---

## 15. Import a module by name using importlib

**Level:** Hard

**Problem:** Import modules whose names are only known at run time.

**Explanation:** `importlib.import_module(name)` imports a module from a string, which is how plugin systems and test frameworks load code dynamically.

```python
import importlib

for name in ["math", "json", "collections"]:
    module = importlib.import_module(name)
    print(module.__name__)

json_module = importlib.import_module("json")
print(json_module.dumps({"a": 1}))

# ------------Expected Output---------

# math
# json
# collections
# {"a": 1}
```

---

## 16. Reload a module after it changes

**Level:** Hard

**Problem:** Pick up changes made to a module file without restarting Python.

**Explanation:** Python caches imported modules in `sys.modules`. `importlib.reload()` runs the file again so the module object reflects the latest code.

```python
import importlib
import pathlib
import shutil
import sys
import tempfile

folder = tempfile.mkdtemp()
file = pathlib.Path(folder, "settings.py")
file.write_text('VERSION = "1.0"\n')

sys.path.insert(0, folder)
import settings
print(settings.VERSION)

file.write_text('VERSION = "2.0.1-updated"\n')
importlib.invalidate_caches()
importlib.reload(settings)
print(settings.VERSION)

sys.path.remove(folder)
shutil.rmtree(folder)

# ------------Expected Output---------

# 1.0
# 2.0.1-updated
```

---

## 17. Inspect which modules are loaded

**Level:** Medium

**Problem:** Check whether a module has been imported and what Python searches for modules.

**Explanation:** `sys.modules` is a dictionary of every imported module and `sys.path` is the list of folders Python searches when you import something.

```python
import sys

print("colorsys" in sys.modules)
import colorsys
print("colorsys" in sys.modules)

print(type(sys.path).__name__, len(sys.path) > 0)
print(sys.modules["colorsys"] is colorsys)

# ------------Expected Output---------

# False
# True
# list True
# True
```

---

## 18. Fall back gracefully when a module is missing

**Level:** Medium

**Problem:** Try to import an optional library and use an alternative when it is not installed.

**Explanation:** A failed import raises `ModuleNotFoundError`. Catching it lets you provide a fallback or a clear message instead of crashing.

```python
try:
    import not_a_real_module_xyz as fast_lib
except ModuleNotFoundError as error:
    print("Missing module:", error.name)
    import json as fast_lib

print("Using:", fast_lib.__name__)

# ------------Expected Output---------

# Missing module: not_a_real_module_xyz
# Using: json
```
