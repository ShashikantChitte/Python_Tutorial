# Math and Number Logic

> [← Practice Codes index](README.md)

Number theory and classic logic warm-ups: FizzBuzz, GCD/LCM, primes, Roman numerals and spreadsheet column names.

**See also:** Factorial (recursion vs iteration) is in [Recursion and Dynamic Programming](12_06_Recursion_and_Dynamic_Programming.md).

**5 problems** · 3 Easy · 2 Medium · 0 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [FizzBuzz and variations](#1-fizzbuzz-and-variations) | Easy |
| 2 | [FizzBuzz (list comprehension) and Excel column name](#2-fizzbuzz-list-comprehension-and-excel-column-name) | Easy |
| 3 | [GCD and LCM of two numbers](#3-gcd-and-lcm-of-two-numbers) | Easy |
| 4 | [Check if a number is prime and generate primes up to n](#4-check-if-a-number-is-prime-and-generate-primes-up-to-n) | Medium |
| 5 | [Convert a Roman numeral to an integer](#5-convert-a-roman-numeral-to-an-integer) | Medium |

---

## Easy

### 1. FizzBuzz and variations

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

### 2. FizzBuzz (list comprehension) and Excel column name

```python
def fizz_buzz(n: int) -> list[str]:
    return [
        "FizzBuzz" if i % 15 == 0 else "Fizz" if i % 3 == 0 else "Buzz" if i % 5 == 0 else str(i)
        for i in range(1, n + 1)
    ]

def excel_column_name(n: int) -> str:
    res = []
    while n > 0:
        n -= 1
        res.append(chr(n % 26 + ord("A")))
        n //= 26
    return "".join(reversed(res))

print("FizzBuzz(15)  :", fizz_buzz(15))
print("Excel Col(28) :", excel_column_name(28))

#-----------Expected Output--------

# FizzBuzz(15)  : ['1', '2', 'Fizz', '4', 'Buzz', 'Fizz', '7', '8', 'Fizz', 'Buzz', '11', 'Fizz', '13', '14', 'FizzBuzz']
# Excel Col(28) : AB
```

### 3. GCD and LCM of two numbers

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

## Medium

### 4. Check if a number is prime and generate primes up to n

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

def primes_upto(n: int) -> list[int]:  # Sieve of Eratosthenes
    sieve = [True] * (n + 1)
    sieve[:2] = [False, False]
    for i in range(2, int(n ** 0.5) + 1):
        if sieve[i]:
            sieve[i * i::i] = [False] * len(sieve[i * i::i])
    return [i for i, p in enumerate(sieve) if p]

print(is_prime(29), primes_upto(30))

# ------------Expected Output---------

# True [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

### 5. Convert a Roman numeral to an integer

```python
def roman_to_int(s: str) -> int:
    vals = {"I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000}
    total = prev = 0
    for ch in reversed(s):
        total += -vals[ch] if vals[ch] < prev else vals[ch]
        prev = vals[ch]
    return total

print("Roman MCMXCIV  :", roman_to_int("MCMXCIV"))

# ------------Expected Output---------

# Roman MCMXCIV  : 1994
```
