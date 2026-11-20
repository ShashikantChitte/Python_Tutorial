# Practice Code – Conditional Statements & Loops

> [← Course index](../README.md)

Try each task yourself first, then compare with the solution below it.

**Topics practised:** `if` / `if-else` / `if-elif-else`, `while`, `for` + `range()`, `break`, `continue`.

Every solution is a small **function** that returns its result, so it is easy to test. The last cell checks all of them.

### Quick Navigation
1. [`if`, `if-else`, and `if-elif-else` (Tasks 1–3)](#conditionals)
2. [`while` Loop (Tasks 4–5)](#while-loop)
3. [`for` Loop and `range()` (Tasks 6–10)](#for-loop)
4. [`break` and `continue` (Tasks 11–14)](#break-continue)
5. [Automated Self-Checks](#self-checks)

| Task # | Assumption made in the solutions |
| :---: | :--- |
| **3** | "Simple scale": **A** ≥ 90, **B** ≥ 80, **C** ≥ 70, **D** ≥ 60, otherwise **F**; scores outside 0–100 are rejected. |
| **4, 5** | Negative numbers are handled by working with the absolute value. |
| **6** | The table runs from `1` to `10`. |
| **13** | Menu: `1` Deposit, `2` Withdraw, `3` Exit. Non-positive amounts and withdrawals larger than the balance are refused. |

---

<a id="conditionals"></a>
## 1. `if`, `if-else`, and `if-elif-else`

### 1. Positive, Negative, or Zero

**Task:** Get a number from the user and print whether it's positive, negative, or zero.

```python
def positive_negative_zero(n):
    if n > 0:
        return "Positive"
    elif n < 0:
        return "Negative"
    else:
        return "Zero"


n = int(input("Enter a number: "))
print(positive_negative_zero(n))
```

**Output:**

```text
Enter a number: -7
Negative
```

### 2. Vowel or Consonant

**Task:** Take a single letter as input and check if it's a vowel (`a, e, i, o, u`) or a consonant.

```python
def vowel_or_consonant(letter):
    if len(letter) != 1 or not letter.isalpha():
        return "Please enter a single letter"
    if letter.lower() in "aeiou":
        return "Vowel"
    return "Consonant"


letter = input("Enter a letter: ")
print(vowel_or_consonant(letter))
```

**Output:**

```text
Enter a letter: e
Vowel
```

### 3. Grading System

**Task:** Get a student's score (0–100) and assign a letter grade (`A`, `B`, `C`, `D`, or `F`) based on a simple scale.

```python
def grade(score):
    if score < 0 or score > 100:
        return "Invalid score"
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    elif score >= 60:
        return "D"
    else:
        return "F"


score = int(input("Enter the score (0-100): "))
print("Grade:", grade(score))
```

**Output:**

```text
Enter the score (0-100): 82
Grade: B
```

---

<a id="while-loop"></a>
## 2. `while` Loop

### 4. Sum of Digits

**Task:** Get a number and calculate the sum of its digits using a `while` loop. For example, for the number `123`, the sum is `1 + 2 + 3 = 6`.

```python
def sum_of_digits(number):
    number = abs(number)
    total = 0
    while number > 0:
        total += number % 10        # last digit
        number //= 10               # drop the last digit
    return total


number = int(input("Enter a number: "))
print("Sum of digits:", sum_of_digits(number))
```

**Output:**

```text
Enter a number: 123
Sum of digits: 6
```

### 5. Reverse a Number

**Task:** Take a number and print its reverse. For example, reversing `1234` gives `4321`.

```python
def reverse_number(number):
    sign = -1 if number < 0 else 1
    number = abs(number)
    reversed_number = 0
    while number > 0:
        reversed_number = reversed_number * 10 + number % 10
        number //= 10
    return sign * reversed_number


number = int(input("Enter a number: "))
print("Reversed:", reverse_number(number))
```

**Output:**

```text
Enter a number: 1234
Reversed: 4321
```

---

<a id="for-loop"></a>
## 3. `for` Loop and `range()`

### 6. Multiplication Table

**Task:** Print the multiplication table for a number entered by the user, up to 10.

```python
def multiplication_table(n):
    lines = []
    for i in range(1, 11):
        lines.append(f"{n} x {i:<2} = {n * i}")
    return lines


n = int(input("Enter a number for its table: "))
for line in multiplication_table(n):
    print(line)
```

**Output:**

```text
Enter a number for its table: 7
7 x 1  = 7
7 x 2  = 14
7 x 3  = 21
7 x 4  = 28
7 x 5  = 35
7 x 6  = 42
7 x 7  = 49
7 x 8  = 56
7 x 9  = 63
7 x 10 = 70
```

### 7. Print a Square

**Task:** Take a number from the user and print a solid square of that size using a specific character, like `*`.

```python
def print_square(size, char="*"):
    if size <= 0:
        return []
    rows = []
    for _ in range(size):
        rows.append(char * size)
    return rows


size = int(input("Enter the size of the square: "))
for row in print_square(size, "*"):
    print(row)
```

**Output:**

```text
Enter the size of the square: 4
****
****
****
****
```

### 8. Sum of First `n` Numbers

**Task:** Get a number from the user and find the sum of all numbers from `1` up to that number.

```python
def sum_first_n(n):
    total = 0
    for i in range(1, n + 1):
        total += i
    return total


n = int(input("Enter n: "))
print("Sum of 1 to n:", sum_first_n(n))
```

**Output:**

```text
Enter n: 10
Sum of 1 to n: 55
```

### 9. Count Vowels in a Word

**Task:** Ask the user for a word and count how many vowels are in it.

```python
def count_vowels(word):
    count = 0
    for letter in word.lower():
        if letter in "aeiou":
            count += 1
    return count


word = input("Enter a word: ")
print("Vowels:", count_vowels(word))
```

**Output:**

```text
Enter a word: education
Vowels: 5
```

### 10. Fibonacci Sequence

**Task:** Print the first `n` terms of the Fibonacci sequence. Each number is the sum of the two preceding ones, usually starting with `0` and `1`. The sequence starts: `0, 1, 1, 2, 3, 5, 8, ...`

```python
def fibonacci(n):
    sequence = []
    a, b = 0, 1
    for _ in range(n):
        sequence.append(a)
        a, b = b, a + b
    return sequence


n = int(input("How many Fibonacci terms? "))
print(fibonacci(n))
```

**Output:**

```text
How many Fibonacci terms? 10
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

---

<a id="break-continue"></a>
## 4. `break` and `continue`

### 11. Input Validation with `break`

**Task:** Repeatedly ask the user to enter a number between `1` and `10`. Use a `while` loop and `break` to exit the loop only when a valid number is entered.

> 💡 Tasks 11 and 13 read several lines of input. To run them without typing, the helper below feeds a scripted list of answers.

```python
def scripted(*lines):
    """Return a read() function that feeds the given lines one by one (simulated typing)."""
    feed = iter(lines)

    def read(prompt=""):
        value = next(feed)
        print(f"{prompt}{value}")
        return value

    return read
```

```python
def input_validation_with_break(read=input):
    while True:
        text = read("Enter a number between 1 and 10: ")
        if text.isdigit() and 1 <= int(text) <= 10:
            break                                  # valid -> leave the loop
        print("  Invalid input, try again.")
    return int(text)


# The scripted "user" types: abc, 15, 0, 7.   To type yourself, use: input_validation_with_break(input)
valid = input_validation_with_break(scripted("abc", "15", "0", "7"))
print("Accepted:", valid)
```

**Output:**

```text
Enter a number between 1 and 10: abc
  Invalid input, try again.
Enter a number between 1 and 10: 15
  Invalid input, try again.
Enter a number between 1 and 10: 0
  Invalid input, try again.
Enter a number between 1 and 10: 7
Accepted: 7
```

### 12. Skip Multiples with `continue`

**Task:** Print all numbers from `1` to `20`, but skip the numbers that are multiples of `3` using the `continue` statement.

```python
def skip_multiples_of_3():
    numbers = []
    for i in range(1, 21):
        if i % 3 == 0:
            continue                               # skip multiples of 3
        numbers.append(i)
    return numbers


print(skip_multiples_of_3())
```

**Output:**

```text
[1, 2, 4, 5, 7, 8, 10, 11, 13, 14, 16, 17, 19, 20]
```

### 13. Simple ATM Simulator with `break`

**Task:** Create a simple loop that shows an "ATM" menu with options like "Deposit", "Withdraw", and "Exit". Use `break` to end the program when the user chooses "Exit".

```python
def atm_simulator(read=input, balance=0.0):
    while True:
        print("\n  ATM MENU  1) Deposit   2) Withdraw   3) Exit")
        choice = read("  Choose an option: ")
        if choice == "3":
            print("  Thank you, goodbye!")
            break                                  # Exit -> leave the loop
        elif choice in ("1", "2"):
            try:
                amount = float(read("  Amount: "))
            except ValueError:
                print("  Please enter a valid amount.")
                continue
            if amount <= 0:
                print("  Amount must be greater than zero.")
            elif choice == "1":
                balance += amount
                print(f"  Deposited {amount:.2f} -> balance {balance:.2f}")
            elif amount > balance:
                print(f"  Insufficient balance (balance {balance:.2f})")
            else:
                balance -= amount
                print(f"  Withdrew {amount:.2f} -> balance {balance:.2f}")
        else:
            print("  Invalid option.")
    return balance


# Scripted: deposit 500, withdraw 800, withdraw 200, bad option, exit.   To type yourself: atm_simulator(input)
final_balance = atm_simulator(scripted("1", "500", "2", "800", "2", "200", "9", "3"))
print(f"Final balance: {final_balance:.2f}")
```

**Output:**

```text

  ATM MENU  1) Deposit   2) Withdraw   3) Exit
  Choose an option: 1
  Amount: 500
  Deposited 500.00 -> balance 500.00

  ATM MENU  1) Deposit   2) Withdraw   3) Exit
  Choose an option: 2
  Amount: 800
  Insufficient balance (balance 500.00)

  ATM MENU  1) Deposit   2) Withdraw   3) Exit
  Choose an option: 2
  Amount: 200
  Withdrew 200.00 -> balance 300.00

  ATM MENU  1) Deposit   2) Withdraw   3) Exit
  Choose an option: 9
  Invalid option.

  ATM MENU  1) Deposit   2) Withdraw   3) Exit
  Choose an option: 3
  Thank you, goodbye!
Final balance: 300.00
```

### 14. Count `'a'`s and `'b'`s in a Word

**Task:** Get a word from the user. Iterate through the word and count the occurrences of the letter `'a'` and `'b'`, using `continue` to skip any other letters.

```python
def count_a_and_b(word):
    a_count = b_count = 0
    for letter in word.lower():
        if letter not in "ab":
            continue                               # skip every other letter
        if letter == "a":
            a_count += 1
        else:
            b_count += 1
    return a_count, b_count


word = input("Enter a word: ")
a_count, b_count = count_a_and_b(word)
print(f"a: {a_count}, b: {b_count}")
```

**Output:**

```text
Enter a word: Bubbalicious
a: 1, b: 3
```

---

<a id="self-checks"></a>
## 5. Check All Solutions

These `assert` statements verify every function above. If something is wrong, Python stops with an `AssertionError`.

```python
assert positive_negative_zero(5) == "Positive"
assert positive_negative_zero(-5) == "Negative"
assert positive_negative_zero(0) == "Zero"
assert vowel_or_consonant("A") == "Vowel"
assert vowel_or_consonant("z") == "Consonant"
assert vowel_or_consonant("12") == "Please enter a single letter"
assert grade(90) == "A" and grade(89) == "B" and grade(80) == "B" and grade(70) == "C"
assert grade(60) == "D" and grade(59) == "F" and grade(101) == "Invalid score"
assert sum_of_digits(123) == 6 and sum_of_digits(-405) == 9 and sum_of_digits(0) == 0
assert reverse_number(1234) == 4321 and reverse_number(-120) == -21
assert multiplication_table(3)[-1] == "3 x 10 = 30"
assert print_square(3, "#") == ["###", "###", "###"] and print_square(0) == []
assert sum_first_n(10) == 55 and sum_first_n(0) == 0
assert count_vowels("Education") == 5
assert fibonacci(7) == [0, 1, 1, 2, 3, 5, 8] and fibonacci(0) == []
assert skip_multiples_of_3() == [1, 2, 4, 5, 7, 8, 10, 11, 13, 14, 16, 17, 19, 20]
assert count_a_and_b("Abracadabra") == (5, 2)

print("\nAll self-checks passed.")
```

**Output:**

```text

All self-checks passed.
```
