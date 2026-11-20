# Chapter 01 - Introduction to Python

> [← Course index](../README.md)

### Table of Contents
1. [Introduction](#1-introduction)
2. [Environment Setup (VS Code + `venv`)](#2-environment-setup-vs-code--venv)
3. [Comments](#3-comments)
4. [Keywords](#4-keywords)

---

## 1. Introduction

Python is a **high-level, general-purpose** programming language created by Guido van Rossum (1991), designed for readability and rapid development.

* **Open-Source:** Free to use, modify, and distribute.
* **Platform-Independent:** Runs seamlessly on Windows, macOS, and Linux.
* **Interpreted:** Executes code line-by-line, making debugging straightforward.
* **Dynamically Typed:** No need to declare variable data types explicitly.

### Why Choose Python?
* **Beginner-Friendly:** Clean, intuitive syntax that reads like plain English.
* **Versatile:** Widely used in web development, data science, automation, and AI.
* **Strong Community:** Massive ecosystem with extensive third-party libraries and support.

---

## 2. Environment Setup (VS Code + `venv`)

1. **Install Python:** Visit [python.org](https://python.org), go to **Downloads**, and run the installer for your OS *(check **"Add Python to PATH"** on Windows)*.
2. **Install VS Code & Extensions:** Install **Visual Studio Code**, open Extensions (`Ctrl + Shift + X` / `Cmd + Shift + X`), and install the official **Python** and **Jupyter** extensions by Microsoft.
3. **Create & Activate a Virtual Environment (`venv`):** Open the VS Code terminal (`Ctrl + \``) in your project folder and run:
   * **Windows (PowerShell):**
     ```powershell
     python -m venv .venv
     .venv\Scripts\activate
     ```
     *(If PowerShell blocks activation, run `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` once first.)*
   * **macOS / Linux:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```
4. **Install `ipykernel`:** With `(.venv)` active in your terminal, install the Jupyter kernel package:
   ```bash
   pip install ipykernel
   ```
5. **Select Kernel & Run Cells:** Click **Select Kernel** in the top-right corner of this notebook, pick **Python Environments...**, and choose your `.venv` interpreter. Press `Shift + Enter` on any code cell to execute it.

---

## 3. Comments

Comments are **non-executable** notes in your code that the Python interpreter ignores at runtime.

**Why use them?**
* Explain complex logic to other developers (and your future self).
* Improve overall code readability.
* Temporarily disable lines of code during testing and debugging.

### Syntax
* **Single-line comment:** Starts with a hash symbol (`#`).
* **Multi-line comment:** Uses triple quotes (`'''` or `"""`). *(Note: Technically, Python treats unassigned triple-quoted strings as multi-line string literals/docstrings, which are ignored at runtime).*

> 💡 **VS Code Shortcut:** Select one or more lines and press `Ctrl + /` (Windows/Linux) or `Cmd + /` (macOS) to quickly comment/uncomment code.

```python
print("--- Comments ---")

# This is a single-line comment - Python ignores it.
print("Comments are ignored by Python")  # An inline comment after code

"""
This is a
multi-line comment (technically an unassigned string literal).
"""

'''
Single triple-quotes work the exact same way.
'''

# Temporarily disable code while testing:
# print("This line is disabled, so it never runs")

print("End of comments demo!")
```

**Output:**

```text
--- Comments ---
Comments are ignored by Python
End of comments demo!
```

---

## 4. Keywords

Keywords are **reserved words** in Python that have a predefined, special meaning.

* **Examples:** `if`, `else`, `while`, `for`, `True`, `False`, `None`, `def`, `return`, etc.
* **Strict Rule:** You **cannot** use keywords as variable, function, or class names.
* **Case-Sensitive:** All keywords are lowercase **except** `True`, `False`, and `None`.
* **Soft Keywords:** Modern Python (3.10+) also includes context-dependent soft keywords (`match`, `case`, `type`, `_`) that only act as keywords in specific patterns.

> 💡 See the full list for your Python version with `import keyword; print(keyword.kwlist)`.

```python
import keyword

print("--- Keywords ---")
print("Number of keywords in this Python version:", len(keyword.kwlist))
print("Keywords:", keyword.kwlist)

# Checking if words are keywords
print("\nIs 'if' a keyword?   ", keyword.iskeyword("if"))
print("Is 'True' a keyword? ", keyword.iskeyword("True"))
print("Is 'true' a keyword? ", keyword.iskeyword("true"))  # False (case-sensitive)
print("Is 'age' a keyword?  ", keyword.iskeyword("age"))

# Soft keywords check (Python 3.10+)
print("Soft keywords:", keyword.softkwlist)

# if = 10   # SyntaxError: cannot assign to a keyword
```

**Output:**

```text
--- Keywords ---
Number of keywords in this Python version: 35
Keywords: ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']

Is 'if' a keyword?    True
Is 'True' a keyword?  True
Is 'true' a keyword?  False
Is 'age' a keyword?   False
Soft keywords: ['_', 'case', 'match', 'type']
```
