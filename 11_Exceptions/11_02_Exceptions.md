# Python Exceptions

> [← Course index](../README.md)

## Difference Between Error and Exception

| Feature | Error (e.g., `SyntaxError`) | Exception (e.g., `ZeroDivisionError`) |
|---------|-----------------------------|---------------------------------------|
| **When it happens** | Before the program starts (compile/parse time) | While the program is running (runtime) |
| **Can it be caught?** | No (in the same file) — must be fixed by the programmer | Yes, using `try / except` blocks |
| **Common examples** | `SyntaxError`, `IndentationError` | `ZeroDivisionError`, `ValueError`, `TypeError`, `NameError`, `FileNotFoundError` |

```python
print("--- Error: SyntaxError stops the program before it even starts ---")
broken_code = 'print("Hello world"'                 # missing closing parenthesis
try:
    compile(broken_code, "<demo>", "exec")         # compile without running
except SyntaxError as err:
    print("SyntaxError:", err.msg)

print("\n--- Exception: raised while the program is running ---")
n = 10
try:
    res = n / 0
except ZeroDivisionError as err:
    print("ZeroDivisionError:", err)
```

**Output:**

```text
--- Error: SyntaxError stops the program before it even starts ---
SyntaxError: '(' was never closed

--- Exception: raised while the program is running ---
ZeroDivisionError: division by zero
```

## Common Built-in Exceptions

| Exception | When it occurs |
|-----------|----------------|
| `ZeroDivisionError` | Dividing a number by `0` |
| `ValueError` | Right data type, invalid value (e.g., `int("abc")`) |
| `TypeError` | Operation on incompatible types (e.g., `"a" + 5`) |
| `NameError` | Using a variable that has not been defined |
| `IndexError` / `KeyError` | Accessing an invalid list index or missing dictionary key |
| `FileNotFoundError` | Trying to open a file that does not exist |

## Exception Handling (`try`, `except`, `else`, `finally`)

```python
try:
    # risky code that might raise an exception
except SomeException as err:
    # runs ONLY if SomeException occurs
else:
    # runs ONLY if NO exception occurred
finally:
    # ALWAYS runs (cleanup: closing files, connections, etc.)
```

> 💡 **Best Practice:** Avoid a bare `except:` (which also catches `KeyboardInterrupt`). Always catch specific exceptions like `except ValueError:` or `except Exception:`.

### 1. `try-except` & Catching Multiple Exceptions

```python
print("--- 1a. Basic try-except ---")
try:
    print(undefined_name)                          # not defined -> NameError
except NameError as err:
    print("Caught NameError:", err)
print("...and the program keeps running!")

print("\n--- 1b. Handling multiple exceptions (separate & tuple syntax) ---")
for sample in ("0", "abc", 10):
    try:
        num = int(sample)
        print("100 /", num, "=", 100 / num)
    except ZeroDivisionError:
        print(f"Input {sample!r}: You can't divide by zero!")
    except (ValueError, TypeError) as err:         # catching multiple types in one block
        print(f"Input {sample!r}: Invalid input ({err})")
```

**Output:**

```text
--- 1a. Basic try-except ---
Caught NameError: name 'undefined_name' is not defined
...and the program keeps running!

--- 1b. Handling multiple exceptions (separate & tuple syntax) ---
Input '0': You can't divide by zero!
Input 'abc': Invalid input (invalid literal for int() with base 10: 'abc')
100 / 10 = 10.0
```

### 2. `try-except-else` and `try-except-finally`

```python
print("--- 2a. try-except-else (else runs ONLY when nothing went wrong) ---")
for risky in (lambda: print("Hello"), lambda: 100 / 0):
    try:
        risky()
    except ZeroDivisionError:
        print("Something went wrong")
    else:
        print("Nothing went wrong (else block ran)")

print("\n--- 2b. try-except-finally (finally ALWAYS runs) ---")
for risky in (lambda: print("Welcome"), lambda: undefined_name):
    try:
        risky()
    except NameError:
        print("Caught NameError")
    finally:
        print("Cleanup complete (finally block ALWAYS runs)")
```

**Output:**

```text
--- 2a. try-except-else (else runs ONLY when nothing went wrong) ---
Hello
Nothing went wrong (else block ran)
Something went wrong

--- 2b. try-except-finally (finally ALWAYS runs) ---
Welcome
Cleanup complete (finally block ALWAYS runs)
Caught NameError
Cleanup complete (finally block ALWAYS runs)
```

### 3. Putting All Four Blocks Together (`try-except-else-finally`)

| Input | Output |
|-------|--------|
| `"0"` | `You can't divide by zero!` + `Execution complete.` |
| `"abc"` | `Enter a valid number!` + `Execution complete.` |
| `"4"` | `Result is 25.0` + `Execution complete.` |

```python
print("--- 3. try-except-else-finally together ---")
def divide_100_by(text):
    try:
        n = int(text)
        res = 100 / n
    except ZeroDivisionError:
        print("You can't divide by zero!")
    except ValueError:
        print("Enter a valid number!")
    else:
        print("Result is", res)
    finally:
        print("Execution complete.")

for sample in ("0", "abc", "4"):
    print(f"input: {sample}")
    divide_100_by(sample)
```

**Output:**

```text
--- 3. try-except-else-finally together ---
input: 0
You can't divide by zero!
Execution complete.
input: abc
Enter a valid number!
Execution complete.
input: 4
Result is 25.0
Execution complete.
```

### 4. Nested `try` Blocks

```python
import os
import tempfile

print("--- 4. Nested try blocks ---")
with tempfile.TemporaryDirectory() as folder:
    target = os.path.join(folder, "myfile.txt")
    try:
        file = open(target, "w")
        try:
            file.write("Welcome")
            print("Written successfully")
        except Exception as err:
            print("Write error:", err)
        finally:
            file.close()
            print("File closed safely")
    except OSError as err:
        print("Open error:", err)
```

**Output:**

```text
--- 4. Nested try blocks ---
Written successfully
File closed safely
```

## Raising Exceptions (`raise`) & Custom Exception Classes

- Use `raise` to trigger an exception manually when a business rule is violated.
- Create your own custom exception class by inheriting from `Exception`.

```python
print("--- Raising built-in exceptions ---")
def set_age(age):                                  # named set_age so it doesn't hide built-in set()
    if not isinstance(age, int):
        raise TypeError("Only integers are allowed")
    if age < 0:
        raise ValueError("Age cannot be negative")
    print(f"Age set to {age}")

set_age(20)
for bad_age in (-5, "twenty"):
    try:
        set_age(bad_age)
    except (ValueError, TypeError) as err:
        print("Caught:", type(err).__name__, "->", err)

print("\n--- User-defined custom exception class ---")
class NegativeAgeError(Exception):
    """Raised when an age below zero is given."""

def set_age_strict(age):
    if age < 0:
        raise NegativeAgeError(f"Age cannot be negative (got {age}).")
    return age

try:
    set_age_strict(-10)
except NegativeAgeError as e:
    print("Caught custom exception:", e)
print("Is subclass of Exception?", issubclass(NegativeAgeError, Exception))
```

**Output:**

```text
--- Raising built-in exceptions ---
Age set to 20
Caught: ValueError -> Age cannot be negative
Caught: TypeError -> Only integers are allowed

--- User-defined custom exception class ---
Caught custom exception: Age cannot be negative (got -10).
Is subclass of Exception? True
```

## Hands-On Practice Exercises

Try solving these in the code cell below before checking the solutions!

1. **Safe List Lookup:** Write a function `get_item(items, index)` that returns `items[index]`, or prints `"Index out of range!"` if an `IndexError` occurs.
2. **Custom `InsufficientBalanceError`:** Create a custom exception class `InsufficientBalanceError(Exception)`. Write `withdraw(balance, amount)` that raises `InsufficientBalanceError` if `amount > balance`, otherwise returns `balance - amount`.

```python
# Your practice code here
```

### Solutions

```python
# 1. Safe List Lookup
def get_item(items, index):
    try:
        return items[index]
    except IndexError:
        print("Index out of range!")
        return None

print("1. Valid lookup  :", get_item(["a", "b", "c"], 1))
print("   Invalid lookup:", get_item(["a", "b", "c"], 10))

# 2. Custom InsufficientBalanceError
class InsufficientBalanceError(Exception):
    pass

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientBalanceError(f"Cannot withdraw {amount} from {balance}")
    return balance - amount

try:
    print("2. Remaining balance:", withdraw(500, 200))
    withdraw(500, 800)
except InsufficientBalanceError as err:
    print("   Caught:", err)
```

**Output:**

```text
1. Valid lookup  : b
Index out of range!
   Invalid lookup: None
2. Remaining balance: 300
   Caught: Cannot withdraw 800 from 500
```

## Key Takeaways

1. **Errors** (like `SyntaxError`) happen before execution and must be fixed in code; **Exceptions** happen at runtime and can be handled.
2. Use `try` for risky code and `except SpecificError:` to handle failures gracefully.
3. `else` runs only when `try` succeeds; `finally` always runs for cleanup.
4. Use `raise` and custom subclasses of `Exception` to enforce clear rules in your programs.
