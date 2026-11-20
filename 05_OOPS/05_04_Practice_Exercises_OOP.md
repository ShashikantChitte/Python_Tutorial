# Hands-On Practice Exercises – Python OOP

> [← Course index](../README.md)

Practice problems covering **Classes, Objects, Methods (`self`, `@classmethod`, `@staticmethod`), Constructors (`__init__`, `__str__`), Encapsulation, Abstraction, Inheritance, and Polymorphism**. Try each exercise before checking the **Solutions** section at the bottom.

## Part 1: Class, Object, Methods & Constructor

**Exercise 1.1 – Book Class**
1. Create a class `Book` with a class variable `category = "General"`.
2. Add `__init__(self, title, author, price)` to store instance variables.
3. Add `__str__(self)` so `print(book)` displays `"<title> by <author> - ₹<price>"`.
4. Add a `@staticmethod` `is_expensive(price)` that returns `True` if `price > 500`, else `False`.
5. Create two `Book` objects, print them, and test `Book.is_expensive()`.

```python
class Book:
    # 1. Class variable (shared by all instances)
    category = "General"

    # 2. Constructor / initializer
    def __init__(self, title, author, price):
        self.title = title
        self.author = author
        self.price = price

    # 3. String representation
    def __str__(self):
        return f"{self.title} by {self.author} - ₹{self.price}"

    # 4. Static method — no self, no cls
    @staticmethod
    def is_expensive(price):
        return price > 500


# 5. Create objects and test
book1 = Book("Wings of Fire", "A.P.J. Abdul Kalam", 350)
book2 = Book("Clean Code", "Robert C. Martin", 750)

print(book1)
print(book2)

print("Book 1 expensive?", Book.is_expensive(book1.price))
print("Book 2 expensive?", Book.is_expensive(book2.price))

# Also accessible via an instance, though a class/static call is idiomatic
print("Via instance:", book2.is_expensive(book2.price))

print("Category:", book1.category, "|", Book.category)
```

**Output:**

```text
Wings of Fire by A.P.J. Abdul Kalam - ₹350
Clean Code by Robert C. Martin - ₹750
Book 1 expensive? False
Book 2 expensive? True
Via instance: True
Category: General | General
```

## Part 2: Encapsulation

**Exercise 2.1 – BankAccount with Private Balance**
1. Create a class `BankAccount` with `owner` (public) and `__balance` (private, initialized to `0`).
2. Add `deposit(amount)` (adds to balance only if `amount > 0`).
3. Add `withdraw(amount)` (deducts only if `0 < amount <= __balance`, otherwise prints `"Insufficient funds"`).
4. Add `get_balance()` to safely return `__balance`.
5. Test depositing `1000`, withdrawing `400`, and attempting to withdraw `800`.

```python
class BankAccount:
    # 1. Constructor: public owner, private __balance
    def __init__(self, owner):
        self.owner = owner
        self.__balance = 0          # name-mangled to _BankAccount__balance

    # 2. Deposit — only positive amounts
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            print(f"Deposited ₹{amount}. Balance: ₹{self.__balance}")
        else:
            print("Deposit amount must be positive")

    # 3. Withdraw — validate against available balance
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            print(f"Withdrew ₹{amount}. Balance: ₹{self.__balance}")
        else:
            print("Insufficient funds")

    # 4. Safe read access
    def get_balance(self):
        return self.__balance


# 5. Test the account
acc = BankAccount("Aarav")
print("Owner:", acc.owner)
print("Initial balance:", acc.get_balance())

acc.deposit(1000)
acc.withdraw(400)
acc.withdraw(800)          # more than the remaining 600 → rejected
print("Final balance:", acc.get_balance())
```

**Output:**

```text
Owner: Aarav
Initial balance: 0
Deposited ₹1000. Balance: ₹1000
Withdrew ₹400. Balance: ₹600
Insufficient funds
Final balance: 600
```

## Part 3: Abstraction

**Exercise 3.1 – Abstract Shape Class**
1. Import `ABC` and `abstractmethod` from `abc`.
2. Create an abstract class `Shape(ABC)` with an abstract method `area(self)`.
3. Create two subclasses `Rectangle(width, height)` and `Circle(radius)` that implement `area()` (`3.14 * radius * radius`).
4. Instantiate both subclasses and print their areas.

```python
from abc import ABC, abstractmethod
import math

# 1 & 2. Abstract base class with an abstract method


class Shape(ABC):
    @abstractmethod
    def area(self):
        """Every concrete shape must implement this."""
        pass


# 3. Concrete subclasses
class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height


class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius


# 4. Instantiate and print areas
rect = Rectangle(10, 5)
circ = Circle(7)

print(f"Rectangle area: {rect.area()}")
print(f"Circle area: {circ.area():.2f}")
```

**Output:**

```text
Rectangle area: 50
Circle area: 153.86
```

## Part 4: Inheritance & Polymorphism

**Exercise 4.1 – Employee & Manager (`super()` and Method Overriding)**
1. Create a parent class `Employee` with `__init__(self, name, salary)` and a method `get_bonus(self)` that returns `salary * 0.10`.
2. Create a child class `Manager(Employee)` with `__init__(self, name, salary, department)` using `super().__init__(name, salary)`.
3. Override `get_bonus(self)` in `Manager` to return `salary * 0.20`.
4. Create one `Employee` and one `Manager` object and print their bonuses.

```python
# 1. Parent class
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def get_bonus(self):
        return self.salary * 0.10


# 2 & 3. Child class with super() and overridden get_bonus
class Manager(Employee):
    def __init__(self, name, salary, department):
        super().__init__(name, salary)      # reuse parent's __init__
        self.department = department

    def get_bonus(self):                    # override
        return self.salary * 0.20


# 4. Test both
emp = Employee("Ravi", 50000)
mgr = Manager("Neha", 80000, "Engineering")

print(f"{emp.name}: bonus = ₹{emp.get_bonus()}")
print(f"{mgr.name} ({mgr.department}): bonus = ₹{mgr.get_bonus()}")
```

**Output:**

```text
Ravi: bonus = ₹5000.0
Neha (Engineering): bonus = ₹16000.0
```

**Exercise 4.2 – Simulated Method Overloading**
Create a class `Greeter` with a method `greet(self, name=None, time_of_day=None)` that prints:
- `"Hello!"` if no arguments are passed
- `"Hello, <name>!"` if only `name` is passed
- `"Good <time_of_day>, <name>!"` if both are passed

```python
class Greeter:
    def greet(self, name=None, time_of_day=None):
        if name is None and time_of_day is None:
            print("Hello!")
        elif time_of_day is None:
            print(f"Hello, {name}!")
        else:
            print(f"Good {time_of_day}, {name}!")


g = Greeter()
g.greet()                        # no arguments
g.greet("Aarav")                 # only name
g.greet("Aarav", "morning")      # both
```

**Output:**

```text
Hello!
Hello, Aarav!
Good morning, Aarav!
```
