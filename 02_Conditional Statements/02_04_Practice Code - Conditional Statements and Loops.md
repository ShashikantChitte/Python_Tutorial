# Practice Code – Conditional Statements & Loops

> [← Course index](../README.md)

Practice for `if / elif / else`, `match`, `for` and `while` loops, `break` / `continue` and nested loops. Each problem is independent and runnable.

**18 problems** · 10 Easy · 8 Medium · 0 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Check whether a number is even or odd](#1-check-whether-a-number-is-even-or-odd) | Easy |
| 2 | [Find the largest of three numbers](#2-find-the-largest-of-three-numbers) | Easy |
| 3 | [Leap year checker](#3-leap-year-checker) | Easy |
| 4 | [Grade calculator with an elif ladder](#4-grade-calculator-with-an-elif-ladder) | Easy |
| 5 | [Simple calculator using match-case](#5-simple-calculator-using-match-case) | Medium |
| 6 | [Classify a triangle from its three sides](#6-classify-a-triangle-from-its-three-sides) | Medium |
| 7 | [FizzBuzz](#7-fizzbuzz) | Easy |
| 8 | [Sum of the digits of a number (while loop)](#8-sum-of-the-digits-of-a-number-while-loop) | Easy |
| 9 | [Check whether a number is a palindrome](#9-check-whether-a-number-is-a-palindrome) | Easy |
| 10 | [Print a multiplication table](#10-print-a-multiplication-table) | Easy |
| 11 | [Print a right-angled triangle pattern with nested loops](#11-print-a-right-angled-triangle-pattern-with-nested-loops) | Easy |
| 12 | [Generate the Fibonacci series](#12-generate-the-fibonacci-series) | Easy |
| 13 | [Find all Armstrong numbers between 100 and 999](#13-find-all-armstrong-numbers-between-100-and-999) | Medium |
| 14 | [Number guessing game with break, continue and for-else](#14-number-guessing-game-with-break-continue-and-for-else) | Medium |
| 15 | [Check whether a number is prime](#15-check-whether-a-number-is-prime) | Medium |
| 16 | [Generate all primes up to n (Sieve of Eratosthenes)](#16-generate-all-primes-up-to-n-sieve-of-eratosthenes) | Medium |
| 17 | [GCD and LCM of two numbers](#17-gcd-and-lcm-of-two-numbers) | Medium |
| 18 | [Convert a number to an Excel column name](#18-convert-a-number-to-an-excel-column-name) | Medium |

---

## 1. Check whether a number is even or odd

**Level:** Easy

**Problem:** For each number in a list, print whether it is even or odd.

**Explanation:** A number is even when `number % 2 == 0`. A conditional expression (`x if condition else y`) keeps the check on one line.

```python
for number in [7, 12, 0, -3]:
    result = "even" if number % 2 == 0 else "odd"
    print(number, "is", result)

# ------------Expected Output---------

# 7 is odd
# 12 is even
# 0 is even
# -3 is odd
```

---

## 2. Find the largest of three numbers

**Level:** Easy

**Problem:** Find the largest of `a`, `b` and `c` using only comparisons.

**Explanation:** Test whether `a` beats both others; if not, only `b` and `c` remain, so one more comparison decides. This shows how an `if / elif / else` chain narrows down the options.

```python
a, b, c = 25, 78, 42

if a >= b and a >= c:
    largest = a
elif b >= c:
    largest = b
else:
    largest = c

print("Largest:", largest)

# ------------Expected Output---------

# Largest: 78
```

---

## 3. Leap year checker

**Level:** Easy

**Problem:** Decide whether a year is a leap year.

**Explanation:** A year is a leap year if it is divisible by 4, except century years, which must also be divisible by 400. The expression combines `and` / `or` to encode that rule.

```python
def is_leap_year(year):
    return year % 4 == 0 and (year % 100 != 0 or year % 400 == 0)

for y in [1900, 2000, 2024, 2026]:
    print(y, is_leap_year(y))

# ------------Expected Output---------

# 1900 False
# 2000 True
# 2024 True
# 2026 False
```

---

## 4. Grade calculator with an elif ladder

**Level:** Easy

**Problem:** Convert marks into a grade: 90+ A, 75+ B, 60+ C, 40+ D, otherwise F.

**Explanation:** Conditions are checked top to bottom and the first true one wins, so the ranges only need a lower bound.

```python
def grade(marks):
    if marks >= 90:
        return "A"
    elif marks >= 75:
        return "B"
    elif marks >= 60:
        return "C"
    elif marks >= 40:
        return "D"
    else:
        return "F"

for m in [95, 82, 67, 45, 20]:
    print(m, "->", grade(m))

# ------------Expected Output---------

# 95 -> A
# 82 -> B
# 67 -> C
# 45 -> D
# 20 -> F
```

---

## 5. Simple calculator using match-case

**Level:** Medium

**Problem:** Build a calculator that handles `+ - * /` and reports unknown operators and division by zero.

**Explanation:** `match` compares the operator against each `case` pattern; `case _` is the default. The division branch guards against a zero divisor.

```python
def calculate(a, op, b):
    match op:
        case "+":
            return a + b
        case "-":
            return a - b
        case "*":
            return a * b
        case "/":
            return a / b if b != 0 else "Cannot divide by zero"
        case _:
            return "Unknown operator"

for expr in [(8, "+", 2), (8, "/", 0), (8, "%", 3)]:
    print(expr, "=", calculate(*expr))

# ------------Expected Output---------

# (8, '+', 2) = 10
# (8, '/', 0) = Cannot divide by zero
# (8, '%', 3) = Unknown operator
```

---

## 6. Classify a triangle from its three sides

**Level:** Medium

**Problem:** Return whether three side lengths form an equilateral, isosceles or scalene triangle, or no triangle at all.

**Explanation:** First check the triangle inequality (each side must be shorter than the sum of the other two). Then compare equal sides to classify the triangle.

```python
def triangle_type(a, b, c):
    if a + b <= c or a + c <= b or b + c <= a:
        return "Not a triangle"
    if a == b == c:
        return "Equilateral"
    if a == b or b == c or a == c:
        return "Isosceles"
    return "Scalene"

for sides in [(3, 3, 3), (5, 5, 8), (3, 4, 5), (1, 2, 5)]:
    print(sides, "->", triangle_type(*sides))

# ------------Expected Output---------

# (3, 3, 3) -> Equilateral
# (5, 5, 8) -> Isosceles
# (3, 4, 5) -> Scalene
# (1, 2, 5) -> Not a triangle
```

---

## 7. FizzBuzz

**Level:** Easy

**Problem:** For numbers 1 to n print "Fizz" for multiples of 3, "Buzz" for multiples of 5 and "FizzBuzz" for both. Extend it with custom rules.

**Explanation:** The generator joins the words of every rule that divides `i`; if no rule matches, it falls back to the number itself. Passing different `rules` gives variations such as a "Bazz" for 7.

```python
def fizzbuzz(n: int, rules=((3, "Fizz"), (5, "Buzz"))):
    for i in range(1, n + 1):
        out = "".join(word for div, word in rules if i % div == 0)
        yield out or str(i)

print(list(fizzbuzz(15)))
print(list(fizzbuzz(21, ((3, "Fizz"), (5, "Buzz"), (7, "Bazz")))))  # variation

# ------------Expected Output---------

# ['1', '2', 'Fizz', '4', 'Buzz', 'Fizz', '7', '8', 'Fizz', 'Buzz', '11', 'Fizz', '13', '14', 'FizzBuzz']
# ['1', '2', 'Fizz', '4', 'Buzz', 'Fizz', 'Bazz', '8', 'Fizz', 'Buzz', '11', 'Fizz', '13', 'Bazz', 'FizzBuzz', '16', '17', 'Fizz', '19', 'Buzz', 'FizzBazz']
```

---

## 8. Sum of the digits of a number (while loop)

**Level:** Easy

**Problem:** Add up the digits of `98765` without converting it to a string.

**Explanation:** Repeatedly take the last digit with `% 10`, add it to the total, and drop it with `// 10` until the number becomes 0.

```python
number = 98765
total = 0

while number > 0:
    total += number % 10
    number //= 10

print("Sum of digits:", total)

# ------------Expected Output---------

# Sum of digits: 35
```

---

## 9. Check whether a number is a palindrome

**Level:** Easy

**Problem:** Decide whether a number reads the same forwards and backwards (for example 12321).

**Explanation:** Build the reversed number digit by digit (`reversed = reversed * 10 + digit`) and compare it with the original.

```python
def is_palindrome_number(n):
    original, reversed_n = n, 0
    while n > 0:
        reversed_n = reversed_n * 10 + n % 10
        n //= 10
    return original == reversed_n

for n in [12321, 1234, 7, 1001]:
    print(n, is_palindrome_number(n))

# ------------Expected Output---------

# 12321 True
# 1234 False
# 7 True
# 1001 True
```

---

## 10. Print a multiplication table

**Level:** Easy

**Problem:** Print the table of 7 from 1 to 10.

**Explanation:** A `for` loop over `range(1, 11)` supplies the multiplier. Format widths (`:2`, `:3`) keep the columns aligned.

```python
n = 7
for i in range(1, 11):
    print(f"{n} x {i:2} = {n * i:3}")

# ------------Expected Output---------

# 7 x  1 =   7
# 7 x  2 =  14
# 7 x  3 =  21
# 7 x  4 =  28
# 7 x  5 =  35
# 7 x  6 =  42
# 7 x  7 =  49
# 7 x  8 =  56
# 7 x  9 =  63
# 7 x 10 =  70
```

---

## 11. Print a right-angled triangle pattern with nested loops

**Level:** Easy

**Problem:** Print five rows of stars where row `i` has `i` stars.

**Explanation:** The outer loop controls the rows, the inner loop prints the stars of one row, and `end=""` keeps them on the same line. The empty `print()` then moves to the next row.

```python
rows = 5
for i in range(1, rows + 1):
    for _ in range(i):
        print("*", end="")
    print()

# ------------Expected Output---------

# *
# **
# ***
# ****
# *****
```

---

## 12. Generate the Fibonacci series

**Level:** Easy

**Problem:** Print the first 10 Fibonacci numbers.

**Explanation:** Keep the last two numbers in `a` and `b`. Each iteration records `a` and then moves the pair forward with `a, b = b, a + b`.

```python
n = 10
a, b = 0, 1
series = []

for _ in range(n):
    series.append(a)
    a, b = b, a + b

print(series)

# ------------Expected Output---------

# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

---

## 13. Find all Armstrong numbers between 100 and 999

**Level:** Medium

**Problem:** An Armstrong number equals the sum of the cubes of its digits (153 = 1³ + 5³ + 3³). Find all three-digit ones.

**Explanation:** Loop over the range, convert each number to a string to get its digits, cube and add them, and keep the number when the sum matches.

```python
armstrong = [
    n for n in range(100, 1000)
    if sum(int(d) ** 3 for d in str(n)) == n
]
print(armstrong)

# ------------Expected Output---------

# [153, 370, 371, 407]
```

---

## 14. Number guessing game with break, continue and for-else

**Level:** Medium

**Problem:** Simulate a guessing game: tell the player if each guess is too low or too high and stop on a correct guess.

**Explanation:** `continue` skips to the next guess, `break` ends the loop on success, and the `else` clause of the `for` loop runs only if the loop finished without `break`.

```python
secret = 42
guesses = [10, 80, 42, 99]

for attempt, guess in enumerate(guesses, start=1):
    if guess < secret:
        print(f"Attempt {attempt}: {guess} is too low")
        continue
    if guess > secret:
        print(f"Attempt {attempt}: {guess} is too high")
        continue
    print(f"Attempt {attempt}: {guess} is correct!")
    break
else:
    print("Out of guesses")

# ------------Expected Output---------

# Attempt 1: 10 is too low
# Attempt 2: 80 is too high
# Attempt 3: 42 is correct!
```

---

## 15. Check whether a number is prime

**Level:** Medium

**Problem:** Return `True` if a number is prime.

**Explanation:** A number has a divisor no larger than its square root if it is composite, so the loop only tests `i * i <= n`. Numbers below 2 are not prime.

```python
def is_prime(n: int) -> bool:
    if n < 2:
        return False
    i = 2
    while i * i <= n:
        if n % i == 0:
            return False
        i += 1
    return True

for n in [1, 2, 17, 18, 29]:
    print(n, is_prime(n))

# ------------Expected Output---------

# 1 False
# 2 True
# 17 True
# 18 False
# 29 True
```

---

## 16. Generate all primes up to n (Sieve of Eratosthenes)

**Level:** Medium

**Problem:** List all prime numbers up to 30.

**Explanation:** Start with every number marked prime, then for each prime `i` cross out its multiples from `i * i` onwards. What remains unmarked are the primes.

```python
def primes_upto(n: int) -> list[int]:  # Sieve of Eratosthenes
    sieve = [True] * (n + 1)
    sieve[:2] = [False, False]
    for i in range(2, int(n ** 0.5) + 1):
        if sieve[i]:
            sieve[i * i::i] = [False] * len(sieve[i * i::i])
    return [i for i, p in enumerate(sieve) if p]

print(primes_upto(30))

# ------------Expected Output---------

# [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

---

## 17. GCD and LCM of two numbers

**Level:** Medium

**Problem:** Compute the greatest common divisor and least common multiple of two numbers.

**Explanation:** Euclid’s algorithm replaces `(a, b)` with `(b, a % b)` until `b` is 0; the remaining `a` is the GCD. The LCM then follows from `a × b / gcd`.

```python
def gcd(a: int, b: int) -> int:
    while b:
        a, b = b, a % b
    return abs(a)

def lcm(a: int, b: int) -> int:
    return abs(a * b) // gcd(a, b) if a and b else 0

print(gcd(48, 18), lcm(4, 6))  # 6 12

# ------------Expected Output---------

# 6 12
```

---

## 18. Convert a number to an Excel column name

**Level:** Medium

**Problem:** Convert 1 → A, 26 → Z, 27 → AA, 28 → AB, and so on.

**Explanation:** It is a base-26 conversion without a zero digit, so each loop step subtracts 1 first, takes `n % 26` as the letter and divides by 26.

```python
def excel_column_name(n: int) -> str:
    res = []
    while n > 0:
        n -= 1
        res.append(chr(n % 26 + ord("A")))
        n //= 26
    return "".join(reversed(res))

for n in [1, 26, 28, 703]:
    print(n, "->", excel_column_name(n))

# ------------Expected Output---------

# 1 -> A
# 26 -> Z
# 28 -> AB
# 703 -> AAA
```
