# Python Functions

> [← Course index](../README.md)

## What is a function?

- A reusable block of code that runs **only when it is called**.
- You can pass data (**parameters**) into a function and **return** output back to the caller.
- Defined using the `def` keyword.

```python
def function_name(parameters):
    """Optional docstring explaining what the function does."""
    # function body
    return value
```

- **Parameter:** variable listed inside the parentheses in the function definition.
- **Argument:** actual value passed into the function when calling it.

## Function Patterns (Parameters & Return Values)

| Pattern | Parameters? | `return`? | Example Use Case |
|---------|-------------|-----------|------------------|
| **1. No params, no return** | No | No | Printing a welcome banner |
| **2. With params, no return** | Yes | No | Printing a personalized greeting |
| **3. With params & return** | Yes | Yes | Calculating a sum or area |
| **4. Returning `None`** | Optional | Empty / omitted | Functions that perform an action without returning data |

```python
print("--- 1. No parameters, no return value ---")
def say_hello():
    print("Hello")

say_hello()

print("\n--- 2. Parameters, no return value ---")
def greet(name):
    print("Hello", name)

greet("Smith")
greet("David")

print("\n--- 3. Parameters and return value ---")
def cal(a, b):
    return a + b

print(cal(10, 20))
total = cal(100, 200)
print("stored result:", total)

print("\n--- 4. Function returning None ---")
def returns_nothing():
    return

def has_no_return():
    i = 100

print(returns_nothing(), has_no_return())
```

**Output:**

```text
--- 1. No parameters, no return value ---
Hello

--- 2. Parameters, no return value ---
Hello Smith
Hello David

--- 3. Parameters and return value ---
30
stored result: 300

--- 4. Function returning None ---
None None
```

## Types of Function Arguments

1. **Positional (required) arguments** – matched by position order.
2. **Keyword arguments** – matched by `parameter=value` name (order doesn't matter).
3. **Default arguments** – uses a fallback value if the caller omits that argument.
4. **Arbitrary positional arguments (`*args`)** – collects extra positional arguments into a **tuple**.
5. **Arbitrary keyword arguments (`**kwargs`)** – collects extra keyword arguments into a **dictionary**.

### Examples: Positional, Keyword, Default & Mixing Rules

> **Rule:** Positional arguments must always come **before** keyword arguments. Writing `show_three(10, b=20, 30)` raises a `SyntaxError`.

```python
print("--- Positional & Keyword arguments ---")
def greetings(name, greetmsg):
    print(greetmsg + " " + name)

greetings("John", "Hello")                    # positional
greetings(greetmsg="Hi", name="Alice")        # keyword (order doesn't matter)

print("\n--- Default arguments ---")
def show_default(i, j=10):
    print(i, j)

show_default(100, 200)
show_default(100)                             # uses default j=10

print("\n--- Mixing positional & keyword arguments ---")
def show_three(a, b, c):
    print(a, b, c)

show_three(10, 20, 30)
show_three(10, 20, c=30)                      # valid: positional first, keyword after
# show_three(10, b=20, 30)                    # SyntaxError: positional argument follows keyword argument
```

**Output:**

```text
--- Positional & Keyword arguments ---
Hello John
Hi Alice

--- Default arguments ---
100 200
100 10

--- Mixing positional & keyword arguments ---
10 20 30
10 20 30
```

### Examples: Arbitrary Arguments (`*args` and `**kwargs`)

```python
print("--- *args (received as a tuple) ---")
def sum_function(*numbers):
    print("   numbers is a", type(numbers).__name__, numbers)
    total = 0
    for i in numbers:
        total += i
    return total

print("Sum 1:", sum_function(1, 2))
print("Sum 2:", sum_function(1, 2, 3))
print("Sum 3:", sum_function())

print("\n--- **kwargs (received as a dictionary) ---")
def user_profile(**details):
    print("   details is a", type(details).__name__, details)
    for key, val in details.items():
        print(f"   {key}: {val}")

user_profile(name="John", role="Developer", city="Pune")
```

**Output:**

```text
--- *args (received as a tuple) ---
   numbers is a tuple (1, 2)
Sum 1: 3
   numbers is a tuple (1, 2, 3)
Sum 2: 6
   numbers is a tuple ()
Sum 3: 0

--- **kwargs (received as a dictionary) ---
   details is a dict {'name': 'John', 'role': 'Developer', 'city': 'Pune'}
   name: John
   role: Developer
   city: Pune
```

### Returning Multiple Values

```python
print("--- Returning multiple values (returned as a tuple) ---")
def largest(a, b):
    if a > b:
        return a, b
    else:
        return b, a

res = largest(10, 20)
print(res, type(res))
big, small = largest(3, 8)                    # unpack the returned tuple
print("big =", big, "| small =", small)
```

**Output:**

```text
--- Returning multiple values (returned as a tuple) ---
(20, 10) <class 'tuple'>
big = 8 | small = 3
```

## Scope of Variables (`local` vs `global`)

- **Local variable:** declared inside a function; exists only while the function runs.
- **Global variable:** declared outside functions; readable anywhere, but requires the `global` keyword to modify inside a function.

```python
print("--- Scope 1: reading global & local ---")
x = 20                                        # global

def scope_one():
    y = 10                                    # local
    print("inside: x =", x, "| y =", y)

scope_one()
print("outside: x =", x)
try:
    print(y)
except NameError as err:
    print("outside: y -> NameError:", err)

print("\n--- Scope 2: same name creates a separate local variable ---")
x = 100
def scope_two():
    x = 200                                   # new local variable (does NOT change global x)
    print("inside :", x)

scope_two()
print("outside:", x)

print("\n--- Scope 3 & 4: modifying or creating a global variable using 'global' ---")
def scope_three():
    global x, z
    x = 200                                   # updates existing global x
    z = 500                                   # creates a new global z
    print("inside : x =", x, "| z =", z)

scope_three()
print("outside: x =", x, "| z =", z)
```

**Output:**

```text
--- Scope 1: reading global & local ---
inside: x = 20 | y = 10
outside: x = 20
outside: y -> NameError: name 'y' is not defined

--- Scope 2: same name creates a separate local variable ---
inside : 200
outside: 100

--- Scope 3 & 4: modifying or creating a global variable using 'global' ---
inside : x = 200 | z = 500
outside: x = 200 | z = 500
```

## Lambda Functions (Anonymous One-Line Functions)

- Small, single-expression functions defined without `def` using the `lambda` keyword.
- Syntax: `lambda arguments : expression`

```python
print("--- Lambda functions ---")
square = lambda n: n * n
add = lambda a, b: a + b

print("Square of 5 :", square(5))
print("Add 10 + 20 :", add(10, 20))
```

**Output:**

```text
--- Lambda functions ---
Square of 5 : 25
Add 10 + 20 : 30
```

## Hands-On Practice Exercises

Try solving these in the code cell below before checking the solutions!

1. **Even or Odd Checker:** Write a function `is_even(num)` that returns `True` if `num` is even and `False` otherwise.
2. **Default & Keyword Arguments:** Write a function `order_coffee(size="Medium", sugar=1)` that prints `"Ordering <size> coffee with <sugar> sugar(s)"`. Call it with defaults and with keyword arguments.
3. **`*args` Average:** Write a function `average(*marks)` that returns the average of any number of marks passed to it (return `0` if none are passed).
4. **Multiple Return Values:** Write a function `min_max(numbers)` that takes a list and returns both the smallest and largest numbers.

```python
# Your practice code here
```

### Solutions

```python
# 1. Even or Odd Checker
def is_even(num):
    return num % 2 == 0

print("1. is_even(4):", is_even(4), "| is_even(7):", is_even(7))

# 2. Default & Keyword Arguments
def order_coffee(size="Medium", sugar=1):
    print(f"2. Ordering {size} coffee with {sugar} sugar(s)")

order_coffee()
order_coffee(sugar=2, size="Large")

# 3. *args Average
def average(*marks):
    return sum(marks) / len(marks) if marks else 0

print("3. Average:", average(80, 90, 100))

# 4. Multiple Return Values
def min_max(numbers):
    return min(numbers), max(numbers)

low, high = min_max([12, 5, 29, 8])
print(f"4. Min: {low}, Max: {high}")
```

**Output:**

```text
1. is_even(4): True | is_even(7): False
2. Ordering Medium coffee with 1 sugar(s)
2. Ordering Large coffee with 2 sugar(s)
3. Average: 90.0
4. Min: 5, Max: 29
```

## Key Takeaways

- Use `def` to define reusable functions (and `lambda` for quick one-liners).
- **Parameters** are placeholders in the definition; **arguments** are the actual values passed.
- Positional arguments must always come **before** keyword arguments.
- `*args` packs extra positional arguments into a **tuple**; `**kwargs` packs extra keyword arguments into a **dictionary**.
- Prefer passing parameters and returning values over modifying `global` variables.
