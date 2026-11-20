# Practice Code – Functions

> [← Course index](../README.md)

Practice for function arguments, lambda expressions, recursion, closures, decorators, generators and scope. Each problem is independent and runnable.

**20 problems** · 9 Easy · 10 Medium · 1 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Default and keyword arguments](#1-default-and-keyword-arguments) | Easy |
| 2 | [Accept any number of values with *args](#2-accept-any-number-of-values-with-args) | Easy |
| 3 | [Accept named options with **kwargs](#3-accept-named-options-with-kwargs) | Easy |
| 4 | [Sort by a custom key using a lambda](#4-sort-by-a-custom-key-using-a-lambda) | Easy |
| 5 | [Transform every item with map()](#5-transform-every-item-with-map) | Easy |
| 6 | [Select items with filter()](#6-select-items-with-filter) | Easy |
| 7 | [Combine all values with reduce()](#7-combine-all-values-with-reduce) | Medium |
| 8 | [Factorial using recursion](#8-factorial-using-recursion) | Easy |
| 9 | [Factorial using iteration](#9-factorial-using-iteration) | Easy |
| 10 | [Fibonacci with memoization](#10-fibonacci-with-memoization) | Medium |
| 11 | [Sum of digits using recursion](#11-sum-of-digits-using-recursion) | Medium |
| 12 | [Check a palindrome using recursion](#12-check-a-palindrome-using-recursion) | Medium |
| 13 | [Create a counter with a closure](#13-create-a-counter-with-a-closure) | Medium |
| 14 | [Write a decorator that logs function calls](#14-write-a-decorator-that-logs-function-calls) | Medium |
| 15 | [Write a decorator that takes arguments](#15-write-a-decorator-that-takes-arguments) | Hard |
| 16 | [Generate values lazily with yield](#16-generate-values-lazily-with-yield) | Medium |
| 17 | [Local, global and nonlocal scope](#17-local-global-and-nonlocal-scope) | Medium |
| 18 | [Type hints and docstrings](#18-type-hints-and-docstrings) | Easy |
| 19 | [Pass a function to another function](#19-pass-a-function-to-another-function) | Medium |
| 20 | [Pre-fill arguments with functools.partial](#20-pre-fill-arguments-with-functoolspartial) | Medium |

---

## 1. Default and keyword arguments

**Level:** Easy

**Problem:** Write a greeting function with a default greeting and call it in different ways.

**Explanation:** A parameter with a default value becomes optional. Keyword arguments let you pass values by name, in any order.

```python
def greet(name, greeting="Hello", punctuation="!"):
    return f"{greeting}, {name}{punctuation}"


print(greet("Ann"))
print(greet("Bob", "Good morning"))
print(greet("Cat", punctuation="?", greeting="Hi"))

# ------------Expected Output---------

# Hello, Ann!
# Good morning, Bob!
# Hi, Cat?
```

---

## 2. Accept any number of values with *args

**Level:** Easy

**Problem:** Write a function that adds up any number of numbers.

**Explanation:** `*args` collects all extra positional arguments into a tuple, so the function works with two numbers or twenty.

```python
def total(*numbers):
    return sum(numbers)


print(total(1, 2))
print(total(10, 20, 30, 40))
print(total())

# ------------Expected Output---------

# 3
# 100
# 0
```

---

## 3. Accept named options with **kwargs

**Level:** Easy

**Problem:** Build a profile description from any number of keyword arguments.

**Explanation:** `**kwargs` collects extra keyword arguments into a dictionary that you can loop over.

```python
def describe(**details):
    return ", ".join(f"{key}={value}" for key, value in details.items())


print(describe(name="Ann", role="QA", city="Pune"))
print(describe(browser="chrome"))

# ------------Expected Output---------

# name=Ann, role=QA, city=Pune
# browser=chrome
```

---

## 4. Sort by a custom key using a lambda

**Level:** Easy

**Problem:** Sort words by their length and then alphabetically.

**Explanation:** A lambda is a small anonymous function. Returning `(len(word), word)` as the key sorts by length first and uses alphabetical order to break ties.

```python
words = ["pear", "fig", "banana", "kiwi", "apple"]

print(sorted(words, key=lambda w: (len(w), w)))

# ------------Expected Output---------

# ['fig', 'kiwi', 'pear', 'apple', 'banana']
```

---

## 5. Transform every item with map()

**Level:** Easy

**Problem:** Square every number in a list.

**Explanation:** `map(function, iterable)` applies the function to every item lazily; wrapping the result in `list()` produces the values.

```python
numbers = [1, 2, 3, 4, 5]

squares = list(map(lambda n: n ** 2, numbers))
print(squares)

# ------------Expected Output---------

# [1, 4, 9, 16, 25]
```

---

## 6. Select items with filter()

**Level:** Easy

**Problem:** Keep only the even numbers from a list.

**Explanation:** `filter(function, iterable)` keeps the items for which the function returns `True`.

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8]

evens = list(filter(lambda n: n % 2 == 0, numbers))
print(evens)

# ------------Expected Output---------

# [2, 4, 6, 8]
```

---

## 7. Combine all values with reduce()

**Level:** Medium

**Problem:** Multiply all the numbers of a list together.

**Explanation:** `functools.reduce` repeatedly applies a two-argument function, carrying the result forward: `((1×2)×3)×4`.

```python
from functools import reduce

numbers = [1, 2, 3, 4]

product = reduce(lambda a, b: a * b, numbers)
print(product)

# ------------Expected Output---------

# 24
```

---

## 8. Factorial using recursion

**Level:** Easy

**Problem:** Compute `n!` by letting the function call itself.

**Explanation:** The base case (`n <= 1`) stops the recursion; every other call multiplies `n` by the factorial of `n - 1`. Negative input is rejected with `ValueError`.

```python
def fact_rec(n: int) -> int:
    if n < 0:
        raise ValueError("negative")
    return 1 if n <= 1 else n * fact_rec(n - 1)

print(fact_rec(5))
print(fact_rec(0))

# ------------Expected Output---------

# 120
# 1
```

---

## 9. Factorial using iteration

**Level:** Easy

**Problem:** Compute `n!` with a loop instead of recursion.

**Explanation:** Start the result at 1 and multiply it by every number from 2 to `n`. Iteration avoids recursion depth limits for large inputs.

```python
def fact_iter(n: int) -> int:
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

print(fact_iter(5))
print(fact_iter(10))

# ------------Expected Output---------

# 120
# 3628800
```

---

## 10. Fibonacci with memoization

**Level:** Medium

**Problem:** Compute `fib(50)` quickly.

**Explanation:** The naive recursive version repeats the same calculations exponentially often. `@lru_cache` remembers earlier results so each value is computed once.

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n: int) -> int:
    return n if n < 2 else fib(n - 1) + fib(n - 2)

print(fib(50))  # 12586269025

# ------------Expected Output---------

# 12586269025
```

---

## 11. Sum of digits using recursion

**Level:** Medium

**Problem:** Add the digits of a number recursively.

**Explanation:** The last digit is `n % 10` and the rest of the number is `n // 10`. The function adds the last digit to the sum of the rest and stops when `n` becomes 0.

```python
def sum_digits(n):
    if n == 0:
        return 0
    return n % 10 + sum_digits(n // 10)


print(sum_digits(2468))
print(sum_digits(100))

# ------------Expected Output---------

# 20
# 1
```

---

## 12. Check a palindrome using recursion

**Level:** Medium

**Problem:** Decide whether a word is a palindrome by comparing its ends recursively.

**Explanation:** A string of length 0 or 1 is a palindrome. Otherwise the first and last characters must match and the middle part must also be a palindrome.

```python
def is_palindrome(text):
    if len(text) <= 1:
        return True
    if text[0] != text[-1]:
        return False
    return is_palindrome(text[1:-1])


print(is_palindrome("level"))
print(is_palindrome("python"))

# ------------Expected Output---------

# True
# False
```

---

## 13. Create a counter with a closure

**Level:** Medium

**Problem:** Create independent counters that remember their own state.

**Explanation:** The inner function remembers `count` from the enclosing function even after the outer function has returned. `nonlocal` lets the inner function change it.

```python
def make_counter(start=0):
    count = start

    def counter():
        nonlocal count
        count += 1
        return count

    return counter


first = make_counter()
second = make_counter(100)
print(first(), first(), first())
print(second(), second())

# ------------Expected Output---------

# 1 2 3
# 101 102
```

---

## 14. Write a decorator that logs function calls

**Level:** Medium

**Problem:** Print every call of a function with its arguments and result without changing the function.

**Explanation:** A decorator takes a function and returns a wrapper. `functools.wraps` keeps the original name and docstring on the wrapper.

```python
import functools


def log_calls(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        print(f"Called {func.__name__}{args} -> {result}")
        return result
    return wrapper


@log_calls
def add(a, b):
    return a + b


add(2, 3)
add(10, 5)

# ------------Expected Output---------

# Called add(2, 3) -> 5
# Called add(10, 5) -> 15
```

---

## 15. Write a decorator that takes arguments

**Level:** Hard

**Problem:** Create `@repeat(3)` that runs a function three times.

**Explanation:** A decorator with arguments needs three levels: `repeat(times)` returns a decorator, which returns the wrapper. The extra outer function is what makes `@repeat(3)` possible.

```python
import functools


def repeat(times):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)
        return wrapper
    return decorator


@repeat(3)
def say(message):
    print(message)


say("Hello")

# ------------Expected Output---------

# Hello
# Hello
# Hello
```

---

## 16. Generate values lazily with yield

**Level:** Medium

**Problem:** Create a Fibonacci generator that produces values one at a time.

**Explanation:** A function containing `yield` returns a generator. It pauses at each `yield` and resumes where it left off, so it never builds the whole list in memory.

```python
def fibonacci(limit):
    a, b = 0, 1
    for _ in range(limit):
        yield a
        a, b = b, a + b


print(list(fibonacci(10)))

gen = fibonacci(3)
print(next(gen), next(gen), next(gen))

# ------------Expected Output---------

# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
# 0 1 1
```

---

## 17. Local, global and nonlocal scope

**Level:** Medium

**Problem:** Show how a function reads and changes variables from outer scopes.

**Explanation:** Assigning inside a function creates a new local variable. To change a module-level variable you must declare it `global`; for a variable in an enclosing function use `nonlocal`.

```python
counter = 0


def increment_global():
    global counter
    counter += 1


def shadow():
    counter = 100          # a new local variable
    return counter


increment_global()
increment_global()
print(counter)
print(shadow())
print(counter)

# ------------Expected Output---------

# 2
# 100
# 2
```

---

## 18. Type hints and docstrings

**Level:** Easy

**Problem:** Document a function so tools and readers know what it expects and returns.

**Explanation:** Annotations (`: float`, `-> float`) describe types without enforcing them, and the docstring becomes the function’s `__doc__`, which `help()` displays.

```python
def discount(price: float, percent: float = 10) -> float:
    """Return the price after applying a percentage discount."""
    return price * (1 - percent / 100)


print(discount(2000))
print(discount.__doc__)
print(discount.__annotations__)

# ------------Expected Output---------

# 1800.0
# Return the price after applying a percentage discount.
# {'price': <class 'float'>, 'percent': <class 'float'>, 'return': <class 'float'>}
```

---

## 19. Pass a function to another function

**Level:** Medium

**Problem:** Write `apply_twice(func, value)` that applies a function two times.

**Explanation:** Functions are first-class objects, so they can be passed as arguments and returned from other functions.

```python
def apply_twice(func, value):
    return func(func(value))


print(apply_twice(lambda n: n * 2, 5))
print(apply_twice(str.upper, "abc"))
print(apply_twice(lambda s: s + "!", "Hi"))

# ------------Expected Output---------

# 20
# ABC
# Hi!!
```

---

## 20. Pre-fill arguments with functools.partial

**Level:** Medium

**Problem:** Create `square` and `cube` functions from one `power` function.

**Explanation:** `partial` creates a new callable with some arguments already fixed, avoiding repetitive wrapper functions.

```python
from functools import partial


def power(base, exponent):
    return base ** exponent


square = partial(power, exponent=2)
cube = partial(power, exponent=3)

print(square(5), cube(5))

# ------------Expected Output---------

# 25 125
```
