# Practice Code – Exceptions

> [← Course index](../README.md)

Handling errors with `try / except / else / finally`, raising and defining exceptions, chaining, retries and cleanup. Each problem is independent and runnable.

**21 problems** · 7 Easy · 11 Medium · 3 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Handle division by zero](#1-handle-division-by-zero) | Easy |
| 2 | [Convert user input safely with ValueError](#2-convert-user-input-safely-with-valueerror) | Easy |
| 3 | [Handle different errors with separate except blocks](#3-handle-different-errors-with-separate-except-blocks) | Easy |
| 4 | [Catch several exception types in one clause](#4-catch-several-exception-types-in-one-clause) | Easy |
| 5 | [Catch a parent exception class](#5-catch-a-parent-exception-class) | Medium |
| 6 | [Understand try / except / else / finally](#6-understand-try--except--else--finally) | Medium |
| 7 | [Always release a resource with finally](#7-always-release-a-resource-with-finally) | Medium |
| 8 | [Raise an exception to reject invalid data](#8-raise-an-exception-to-reject-invalid-data) | Easy |
| 9 | [Define a custom exception](#9-define-a-custom-exception) | Medium |
| 10 | [Chain exceptions with raise ... from](#10-chain-exceptions-with-raise--from) | Hard |
| 11 | [Inspect the details of an exception](#11-inspect-the-details-of-an-exception) | Medium |
| 12 | [Retry an operation using a loop](#12-retry-an-operation-using-a-loop) | Medium |
| 13 | [Write a retry decorator](#13-write-a-retry-decorator) | Hard |
| 14 | [Fall back to a default when a file is missing](#14-fall-back-to-a-default-when-a-file-is-missing) | Easy |
| 15 | [Keep asking until the input is valid](#15-keep-asking-until-the-input-is-valid) | Medium |
| 16 | [Raise IndexError when popping from an empty stack](#16-raise-indexerror-when-popping-from-an-empty-stack) | Medium |
| 17 | [Reject negative input in a recursive function](#17-reject-negative-input-in-a-recursive-function) | Medium |
| 18 | [Check assumptions with assert](#18-check-assumptions-with-assert) | Easy |
| 19 | [Ignore an expected exception with contextlib.suppress](#19-ignore-an-expected-exception-with-contextlibsuppress) | Medium |
| 20 | [Process a batch and collect the errors](#20-process-a-batch-and-collect-the-errors) | Hard |
| 21 | [EAFP versus LBYL](#21-eafp-versus-lbyl) | Medium |

---

## 1. Handle division by zero

**Level:** Easy

**Problem:** Divide two numbers and report a friendly message when the divisor is zero.

**Explanation:** Code that may fail goes into `try`. When `ZeroDivisionError` occurs, execution jumps to the matching `except` block instead of crashing.

```python
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "Cannot divide by zero"


print(divide(10, 2))
print(divide(10, 0))

# ------------Expected Output---------

# 5.0
# Cannot divide by zero
```

---

## 2. Convert user input safely with ValueError

**Level:** Easy

**Problem:** Convert a list of strings to integers and skip the ones that are not numbers.

**Explanation:** `int()` raises `ValueError` for text that is not a number. Catching it inside the loop lets the remaining values still be processed.

```python
raw_values = ["10", "abc", "25", "3.5", "40"]
numbers = []

for raw in raw_values:
    try:
        numbers.append(int(raw))
    except ValueError:
        print(f"Skipping invalid value: {raw!r}")

print(numbers)

# ------------Expected Output---------

# Skipping invalid value: 'abc'
# Skipping invalid value: '3.5'
# [10, 25, 40]
```

---

## 3. Handle different errors with separate except blocks

**Level:** Easy

**Problem:** Respond differently to a wrong type and an invalid value.

**Explanation:** Several `except` clauses can follow one `try`. Python runs the first one that matches the raised exception type.

```python
def to_percent(value):
    try:
        return f"{int(value) / 100:.0%}"
    except ValueError:
        return "not a number"
    except TypeError:
        return "wrong type"


print(to_percent("45"))
print(to_percent("abc"))
print(to_percent(None))

# ------------Expected Output---------

# 45%
# not a number
# wrong type
```

---

## 4. Catch several exception types in one clause

**Level:** Easy

**Problem:** Treat `KeyError` and `IndexError` the same way.

**Explanation:** Putting a tuple of exception types in one `except` clause handles all of them with the same code. `as error` gives access to the exception object.

```python
data = {"items": [10, 20, 30]}

for key, index in [("items", 1), ("items", 9), ("missing", 0)]:
    try:
        print(data[key][index])
    except (KeyError, IndexError) as error:
        print("Lookup failed:", type(error).__name__)

# ------------Expected Output---------

# 20
# Lookup failed: IndexError
# Lookup failed: KeyError
```

---

## 5. Catch a parent exception class

**Level:** Medium

**Problem:** Handle both `KeyError` and `IndexError` by catching their common parent.

**Explanation:** Exceptions form a hierarchy. `LookupError` is the parent of `KeyError` and `IndexError`, so one `except LookupError` handles both.

```python
print(issubclass(KeyError, LookupError), issubclass(IndexError, LookupError))

for container, key in [({"a": 1}, "b"), ([1, 2, 3], 7)]:
    try:
        container[key]
    except LookupError as error:
        print("Caught", type(error).__name__)

# ------------Expected Output---------

# True True
# Caught KeyError
# Caught IndexError
```

---

## 6. Understand try / except / else / finally

**Level:** Medium

**Problem:** See exactly when each block runs.

**Explanation:** `else` runs only when no exception happened, and `finally` always runs – whether or not there was an error – which makes it ideal for cleanup.

```python
def run(value):
    try:
        result = 10 / value
    except ZeroDivisionError:
        print("except: division by zero")
    else:
        print("else: result is", result)
    finally:
        print("finally: always runs")


run(2)
print("---")
run(0)

# ------------Expected Output---------

# else: result is 5.0
# finally: always runs
# ---
# except: division by zero
# finally: always runs
```

---

## 7. Always release a resource with finally

**Level:** Medium

**Problem:** Make sure a resource is closed even when an error occurs in the middle.

**Explanation:** The `finally` block runs even if the `try` block raises or returns, so the cleanup is guaranteed.

```python
def process(fail):
    resource = "database connection"
    print("opened", resource)
    try:
        if fail:
            raise RuntimeError("something went wrong")
        return "processed"
    finally:
        print("closed", resource)


print(process(False))

try:
    process(True)
except RuntimeError as error:
    print("Error:", error)

# ------------Expected Output---------

# opened database connection
# closed database connection
# processed
# opened database connection
# closed database connection
# Error: something went wrong
```

---

## 8. Raise an exception to reject invalid data

**Level:** Easy

**Problem:** Raise `ValueError` when an age is outside the valid range.

**Explanation:** `raise` creates and throws an exception with a message. The caller can then decide how to handle it.

```python
def set_age(age):
    if not 0 <= age <= 120:
        raise ValueError(f"Invalid age: {age}")
    return age


print(set_age(30))

try:
    set_age(150)
except ValueError as error:
    print("Error:", error)

# ------------Expected Output---------

# 30
# Error: Invalid age: 150
```

---

## 9. Define a custom exception

**Level:** Medium

**Problem:** Create a `ValidationError` that also records which field failed.

**Explanation:** A custom exception is a class that inherits from `Exception`. Extra attributes carry details that the handler can use.

```python
class ValidationError(Exception):
    def __init__(self, field, message):
        super().__init__(f"{field}: {message}")
        self.field = field


def validate_user(user):
    if "@" not in user.get("email", ""):
        raise ValidationError("email", "must contain @")
    return True


try:
    validate_user({"email": "ann.example.com"})
except ValidationError as error:
    print(error)
    print("Failed field:", error.field)

# ------------Expected Output---------

# email: must contain @
# Failed field: email
```

---

## 10. Chain exceptions with raise ... from

**Level:** Hard

**Problem:** Wrap a low-level `KeyError` in a more meaningful error without losing the original cause.

**Explanation:** `raise NewError(...) from original` links the two exceptions. The original stays available as `__cause__`, which is invaluable when debugging.

```python
class ConfigError(Exception):
    pass


def read_setting(config, key):
    try:
        return config[key]
    except KeyError as error:
        raise ConfigError(f"Missing setting: {key}") from error


try:
    read_setting({"env": "qa"}, "timeout")
except ConfigError as error:
    print(error)
    print("Caused by:", repr(error.__cause__))

# ------------Expected Output---------

# Missing setting: timeout
# Caused by: KeyError('timeout')
```

---

## 11. Inspect the details of an exception

**Level:** Medium

**Problem:** Print the type, message and arguments of a caught exception.

**Explanation:** The exception object has a type, its `args` tuple and a string form. `repr(error)` shows the type and arguments together.

```python
try:
    int("forty-two")
except ValueError as error:
    print(type(error).__name__)
    print(error.args)
    print(str(error))
    print(repr(error))

# ------------Expected Output---------

# ValueError
# ("invalid literal for int() with base 10: 'forty-two'",)
# invalid literal for int() with base 10: 'forty-two'
# ValueError("invalid literal for int() with base 10: 'forty-two'")
```

---

## 12. Retry an operation using a loop

**Level:** Medium

**Problem:** Try an unreliable operation up to three times before giving up.

**Explanation:** A `for` loop with `try / except` retries after a failure and `break`s on success. The `else` clause of the loop runs only when every attempt failed.

```python
attempts = {"count": 0}


def unreliable():
    attempts["count"] += 1
    if attempts["count"] < 3:
        raise ConnectionError("temporary failure")
    return "connected"


for attempt in range(1, 4):
    try:
        print(f"Attempt {attempt}:", unreliable())
        break
    except ConnectionError as error:
        print(f"Attempt {attempt} failed:", error)
else:
    print("All attempts failed")

# ------------Expected Output---------

# Attempt 1 failed: temporary failure
# Attempt 2 failed: temporary failure
# Attempt 3: connected
```

---

## 13. Write a retry decorator

**Level:** Hard

**Problem:** Re-run a flaky function automatically a few times before the error is raised.

**Explanation:** The decorator wraps the function in a loop that catches the listed exceptions, waits briefly and tries again. If every attempt fails, the last exception is raised.

```python
import functools, time

def retry(times=3, delay=0.1, exceptions=(AssertionError,)):
    def decorator(fn):
        @functools.wraps(fn)
        def wrapper(*args, **kwargs):
            last = None
            for attempt in range(1, times + 1):
                try: return fn(*args, **kwargs)
                except exceptions as e:
                    last = e; time.sleep(delay)
            raise last
        return wrapper
    return decorator

attempts = {"count": 0}


@retry(times=3, delay=0, exceptions=(ConnectionError,))
def flaky():
    attempts["count"] += 1
    if attempts["count"] < 3:
        raise ConnectionError("temporary failure")
    return "success"


print(flaky())
print("Attempts used:", attempts["count"])

# ------------Expected Output---------

# success
# Attempts used: 3
```

---

## 14. Fall back to a default when a file is missing

**Level:** Easy

**Problem:** Read a configuration file and use defaults if it does not exist.

**Explanation:** Opening a missing file raises `FileNotFoundError`. The handler returns default settings so the program can continue.

```python
import json


def load_config(path):
    try:
        with open(path, encoding="utf-8") as f:
            return json.load(f)
    except FileNotFoundError:
        return {"env": "qa", "timeout": 10}


print(load_config("no_such_config.json"))

# ------------Expected Output---------

# {'env': 'qa', 'timeout': 10}
```

---

## 15. Keep asking until the input is valid

**Level:** Medium

**Problem:** Repeat a prompt until the user enters a positive whole number (simulated with a list of answers).

**Explanation:** A `while True` loop retries after each `ValueError` and leaves with `break` once the value is valid. The answers are simulated with an iterator so the example runs without typing.

```python
answers = iter(["abc", "-4", "0", "7"])

while True:
    raw = next(answers)
    try:
        number = int(raw)
        if number <= 0:
            raise ValueError("must be positive")
        break
    except ValueError as error:
        print(f"Invalid input {raw!r}: {error}")

print("Accepted:", number)

# ------------Expected Output---------

# Invalid input 'abc': invalid literal for int() with base 10: 'abc'
# Invalid input '-4': must be positive
# Invalid input '0': must be positive
# Accepted: 7
```

---

## 16. Raise IndexError when popping from an empty stack

**Level:** Medium

**Problem:** Show how a data structure signals misuse with an exception.

**Explanation:** The `Stack` class raises `IndexError` when `pop()` is called on an empty stack. Callers can catch it or check `is_empty()` first.

```python
class Stack:
    def __init__(self):
        self._data = []

    def push(self, x): self._data.append(x)

    def pop(self):
        if self.is_empty():
            raise IndexError("pop from empty stack")
        return self._data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("peek from empty stack")
        return self._data[-1]

    def is_empty(self): return not self._data
    def __len__(self): return len(self._data)

stack = Stack()
stack.push("a")
print(stack.pop())

try:
    stack.pop()
except IndexError as error:
    print("Error:", error)

# ------------Expected Output---------

# a
# Error: pop from empty stack
```

---

## 17. Reject negative input in a recursive function

**Level:** Medium

**Problem:** Make a factorial function refuse negative numbers.

**Explanation:** The function raises `ValueError` before it starts recursing, so invalid input is reported immediately with a clear exception.

```python
def fact_rec(n: int) -> int:
    if n < 0:
        raise ValueError("negative")
    return 1 if n <= 1 else n * fact_rec(n - 1)

print(fact_rec(5))

try:
    fact_rec(-3)
except ValueError as error:
    print("Error:", error)

# ------------Expected Output---------

# 120
# Error: negative
```

---

## 18. Check assumptions with assert

**Level:** Easy

**Problem:** Use `assert` to verify a condition and see what happens when it fails.

**Explanation:** `assert condition, message` raises `AssertionError` with the message when the condition is false. It is mainly used for internal checks and in tests.

```python
def average(numbers):
    assert len(numbers) > 0, "numbers must not be empty"
    return sum(numbers) / len(numbers)


print(average([4, 8, 6]))

try:
    average([])
except AssertionError as error:
    print("AssertionError:", error)

# ------------Expected Output---------

# 6.0
# AssertionError: numbers must not be empty
```

---

## 19. Ignore an expected exception with contextlib.suppress

**Level:** Medium

**Problem:** Delete a file that may or may not exist without writing a try/except block.

**Explanation:** `contextlib.suppress` silently ignores the listed exception types inside the `with` block – handy for "delete if present" style cleanup.

```python
import os
from contextlib import suppress

with suppress(FileNotFoundError):
    os.remove("file_that_does_not_exist.txt")

print("Cleanup finished without errors")

# ------------Expected Output---------

# Cleanup finished without errors
```

---

## 20. Process a batch and collect the errors

**Level:** Hard

**Problem:** Process every record, remember which ones failed and report them at the end.

**Explanation:** Catching the exception inside the loop lets processing continue with the next record. Failed items and their reasons are stored for a summary.

```python
records = ["10", "20", "x", "40", None, "60"]
total = 0
errors = []

for position, record in enumerate(records):
    try:
        total += int(record)
    except (ValueError, TypeError) as error:
        errors.append((position, type(error).__name__))

print("Total:", total)
print("Errors:", errors)

# ------------Expected Output---------

# Total: 130
# Errors: [(2, 'ValueError'), (4, 'TypeError')]
```

---

## 21. EAFP versus LBYL

**Level:** Medium

**Problem:** Compare "look before you leap" with "easier to ask forgiveness than permission".

**Explanation:** LBYL checks conditions first. EAFP simply tries the operation and handles the failure; it is the more Pythonic style and avoids race conditions in many cases.

```python
settings = {"theme": "dark"}

# LBYL - check first
if "language" in settings:
    language = settings["language"]
else:
    language = "en"

# EAFP - try, then handle the failure
try:
    language2 = settings["language"]
except KeyError:
    language2 = "en"

print(language, language2)
print(settings.get("language", "en"))   # the shortest option for dictionaries

# ------------Expected Output---------

# en en
# en
```
