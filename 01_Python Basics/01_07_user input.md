# User Input

> [← Course index](../README.md)

Use the built-in `input()` function to accept keyboard input from a user.

* **Always Returns `str`:** Even if the user types digits like `100`, Python reads it as text (`"100"`).
* **Type Conversion Needed:** Wrap `input()` with `int()` or `float()` when you need to perform math on the entered value.
* **Prompt Message:** Pass an optional string inside `input("Prompt: ")` to guide the user.

```python
print("--- Taking User Input (input() always returns a string) ---")
name = input("Enter name: ")
num1 = input("Enter first number: ")
num2 = input("Enter second number: ")

print("Hello,", name)
print("Type of num1:", type(num1))
print("num1 + num2 (as strings) =", num1 + num2)                 # e.g., '100' + '200' -> '100200'
print("int(num1) + int(num2)    =", int(num1) + int(num2))       # e.g., 100 + 200 -> 300

# Converting directly at the time of input:
dec1 = float(input("Enter first decimal: "))
dec2 = float(input("Enter second decimal: "))
print("Float sum =", dec1 + dec2)
```

**Output:**

```text
--- Taking User Input (input() always returns a string) ---
Enter name: John
Enter first number: 100
Enter second number: 200
Hello, John
Type of num1: <class 'str'>
num1 + num2 (as strings) = 100200
int(num1) + int(num2)    = 300
Enter first decimal: 10.5
Enter second decimal: 20.0
Float sum = 30.5
```
