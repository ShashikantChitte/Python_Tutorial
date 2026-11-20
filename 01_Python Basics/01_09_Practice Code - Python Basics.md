# Practice Code – Python Basics

> [← Course index](../README.md)

Small, self-contained exercises on variables, operators, type casting and formatted output. Each problem is independent – copy it into a file or a notebook cell and run it.

**10 problems** · 10 Easy · 0 Medium · 0 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Swap two variables without a temporary variable](#1-swap-two-variables-without-a-temporary-variable) | Easy |
| 2 | [Convert Celsius to Fahrenheit and Kelvin](#2-convert-celsius-to-fahrenheit-and-kelvin) | Easy |
| 3 | [Simple interest calculator](#3-simple-interest-calculator) | Easy |
| 4 | [Split a restaurant bill with tip](#4-split-a-restaurant-bill-with-tip) | Easy |
| 5 | [Convert seconds to hh:mm:ss](#5-convert-seconds-to-hhmmss) | Easy |
| 6 | [Extract the digits of a three-digit number](#6-extract-the-digits-of-a-three-digit-number) | Easy |
| 7 | [Floor division, modulus and power (including negatives)](#7-floor-division-modulus-and-power-including-negatives) | Easy |
| 8 | [Type casting between int, float, str and bool](#8-type-casting-between-int-float-str-and-bool) | Easy |
| 9 | [Print an aligned receipt with f-strings](#9-print-an-aligned-receipt-with-f-strings) | Easy |
| 10 | [Identify the data type of different values](#10-identify-the-data-type-of-different-values) | Easy |

---

## 1. Swap two variables without a temporary variable

**Level:** Easy

**Problem:** Swap the values of `a` and `b`.

**Explanation:** Python evaluates the whole right-hand side first and then unpacks it, so `a, b = b, a` swaps the values without needing a third variable.

```python
a, b = 10, 20
print("Before:", a, b)

a, b = b, a
print("After :", a, b)

# ------------Expected Output---------

# Before: 10 20
# After : 20 10
```

---

## 2. Convert Celsius to Fahrenheit and Kelvin

**Level:** Easy

**Problem:** Convert a temperature in °C to °F and K and print both neatly.

**Explanation:** The formulas are `F = C × 9/5 + 32` and `K = C + 273.15`. An f-string format spec such as `:.1f` controls the number of decimals.

```python
celsius = 36.6
fahrenheit = celsius * 9 / 5 + 32
kelvin = celsius + 273.15

print(f"{celsius}°C = {fahrenheit:.1f}°F = {kelvin:.2f} K")

# ------------Expected Output---------

# 36.6°C = 97.9°F = 309.75 K
```

---

## 3. Simple interest calculator

**Level:** Easy

**Problem:** Calculate simple interest and the total amount for a principal, rate and number of years.

**Explanation:** Simple interest is `principal × rate × years / 100`. The `,` in `:,.2f` adds thousands separators and rounds to two decimals.

```python
principal = 15000
rate = 7.5      # percent per year
years = 3

interest = principal * rate * years / 100
total = principal + interest

print(f"Interest: {interest:,.2f}")
print(f"Total   : {total:,.2f}")

# ------------Expected Output---------

# Interest: 3,375.00
# Total   : 18,375.00
```

---

## 4. Split a restaurant bill with tip

**Level:** Easy

**Problem:** Add a tip to a bill and work out how much each person pays.

**Explanation:** Multiply the bill by `1 + tip/100` to include the tip, then divide by the number of people. Formatting keeps currency values at two decimals.

```python
bill = 2850.75
tip_percent = 10
people = 4

total = bill * (1 + tip_percent / 100)
per_person = total / people

print(f"Total with tip  : {total:,.2f}")
print(f"Each person pays: {per_person:,.2f}")

# ------------Expected Output---------

# Total with tip  : 3,135.83
# Each person pays: 783.96
```

---

## 5. Convert seconds to hh:mm:ss

**Level:** Easy

**Problem:** Turn a number of seconds into hours, minutes and seconds.

**Explanation:** `divmod(a, b)` returns the quotient and remainder together. Divide by 3600 for hours, then divide the remainder by 60 for minutes. `:02d` pads with a leading zero.

```python
total_seconds = 98765

hours, remainder = divmod(total_seconds, 3600)
minutes, seconds = divmod(remainder, 60)

print(f"{hours:02d}:{minutes:02d}:{seconds:02d}")

# ------------Expected Output---------

# 27:26:05
```

---

## 6. Extract the digits of a three-digit number

**Level:** Easy

**Problem:** Split `472` into hundreds, tens and ones and add them up.

**Explanation:** `// 100` gives the hundreds digit, `// 10 % 10` the tens digit and `% 10` the last digit. This is the arithmetic behind many digit problems.

```python
number = 472

hundreds = number // 100
tens = number // 10 % 10
ones = number % 10

print(hundreds, tens, ones)
print("Sum of digits:", hundreds + tens + ones)

# ------------Expected Output---------

# 4 7 2
# Sum of digits: 13
```

---

## 7. Floor division, modulus and power (including negatives)

**Level:** Easy

**Problem:** Show how `//`, `%`, `**` and `/` behave, especially with negative numbers.

**Explanation:** `//` rounds towards negative infinity, so `-17 // 5` is `-4`, and `%` always takes the sign of the divisor. True division `/` always returns a float.

```python
print(17 // 5, 17 % 5, 17 ** 2)
print(-17 // 5, -17 % 5)
print(17 / 5, type(17 / 5))
print(divmod(-17, 5))

# ------------Expected Output---------

# 3 2 289
# -4 3
# 3.4 <class 'float'>
# (-4, 3)
```

---

## 8. Type casting between int, float, str and bool

**Level:** Easy

**Problem:** Convert values between the basic types and observe the results.

**Explanation:** `int()` truncates (it does not round), `round()` rounds, and `bool()` treats empty or zero values as `False`. Note that `bool("False")` is `True` because any non-empty string is truthy.

```python
print(int("42") + 1)
print(float("3.14") * 2)
print(int(7.9), round(7.9))
print(str(100) + "%")
print(bool(0), bool(""), bool("False"), bool([]))

# ------------Expected Output---------

# 43
# 6.28
# 7 8
# 100%
# False False True False
```

---

## 9. Print an aligned receipt with f-strings

**Level:** Easy

**Problem:** Print a table of items with quantity, price and line total, followed by the grand total.

**Explanation:** In an f-string, `<` left-aligns, `>` right-aligns and the number sets the column width. Using the same widths for every row keeps the columns lined up.

```python
items = [("Notebook", 3, 45.5), ("Pen", 10, 5.25), ("Backpack", 1, 1299)]

print(f"{'Item':<12}{'Qty':>5}{'Price':>10}{'Total':>12}")
print("-" * 39)

grand = 0
for name, qty, price in items:
    total = qty * price
    grand += total
    print(f"{name:<12}{qty:>5}{price:>10.2f}{total:>12.2f}")

print("-" * 39)
print(f"{'Grand total':<27}{grand:>12.2f}")

# ------------Expected Output---------

# Item          Qty     Price       Total
# ---------------------------------------
# Notebook        3     45.50      136.50
# Pen            10      5.25       52.50
# Backpack        1   1299.00     1299.00
# ---------------------------------------
# Grand total                     1488.00
```

---

## 10. Identify the data type of different values

**Level:** Easy

**Problem:** Loop over a mixed list and print the type name of each value.

**Explanation:** `type(value).__name__` gives a readable type name, and `!r` prints the value with `repr()` so strings show their quotes. `isinstance(True, int)` is `True` because `bool` is a subclass of `int`.

```python
values = [42, 3.14, "hello", True, None, [1, 2], (1, 2), {"a": 1}, {1, 2}]

for v in values:
    print(f"{v!r:12} -> {type(v).__name__}")

print(isinstance(True, int))

# ------------Expected Output---------

# 42           -> int
# 3.14         -> float
# 'hello'      -> str
# True         -> bool
# None         -> NoneType
# [1, 2]       -> list
# (1, 2)       -> tuple
# {'a': 1}     -> dict
# {1, 2}       -> set
# True
```
