# Encapsulation & Abstraction in Python

> [← Course index](../README.md)

## Part 1 – Encapsulation

### What is encapsulation?
- Wrapping **variables (data) and methods (functions) into a single unit** (a class).
- **Restricting direct access** to sensitive variables so they cannot be modified accidentally.

### Why do we need it?
1. **Data protection** – prevents accidental modification of important data.
2. **Controlled access** – decides how data is read or modified (using *getters*, *setters*, or `@property`).
3. **Security** – hides internal state from the outside world.

### Access Modifiers in Python

| Modifier | Syntax | Meaning |
|----------|--------|---------|
| **Public** (default) | `self.name` | can be accessed anywhere |
| **Protected** (convention) | `self._marks` | should not be accessed directly outside the class or subclasses |
| **Private** | `self.__balance` | cannot be accessed directly outside the class |

> 💡 *Under the hood:* Python renames a private variable `__balance` to `_Account__balance` (**name mangling**).

```python
print("--- Access modifiers: public, protected, private ---")
class Account:
    def __init__(self):
        self.owner = "Rahul"        # public
        self._branch = "Pune"       # protected (by convention only)
        self.__balance = 1000       # private

acc = Account()
print("public    :", acc.owner)
print("protected :", acc._branch, "(works, but by convention you should not touch it)")
try:
    print(acc.__balance)
except AttributeError as err:
    print("private   : AttributeError:", err)
print("name mangling ->", acc._Account__balance)
```

**Output:**

```text
--- Access modifiers: public, protected, private ---
public    : Rahul
protected : Pune (works, but by convention you should not touch it)
private   : AttributeError: 'Account' object has no attribute '__balance'
name mangling -> 1000
```

### Getters & Setters (and Pythonic `@property`)

```python
print("\n--- Encapsulation with getter and setter ---")
class Student:
    def __init__(self, name):
        self.name = name
        self.__marks = 0             # private variable

    def get_marks(self):             # getter
        return self.__marks

    def set_marks(self, marks):      # setter with validation
        if 0 <= marks <= 100:
            self.__marks = marks
        else:
            print("Invalid marks! Must be between 0 and 100.")

s1 = Student("Rahul")
s1.set_marks(90)
print("marks after set_marks(90)  :", s1.get_marks())
s1.set_marks(150)                    # rejected by the setter
print("marks after set_marks(150) :", s1.get_marks(), "(unchanged)")

print("\n--- Pythonic way: @property decorator ---")
class Product:
    def __init__(self, price):
        self.__price = price

    @property
    def price(self):                 # acts like a getter: p.price
        return self.__price

    @price.setter
    def price(self, value):          # acts like a setter: p.price = 500
        if value > 0:
            self.__price = value
        else:
            print("Price must be positive!")

p = Product(100)
p.price = 250
print("Product price:", p.price)
```

**Output:**

```text

--- Encapsulation with getter and setter ---
marks after set_marks(90)  : 90
Invalid marks! Must be between 0 and 100.
marks after set_marks(150) : 90 (unchanged)

--- Pythonic way: @property decorator ---
Product price: 250
```

## Part 2 – Abstraction

Data abstraction means **showing only the essential features and hiding complex internal details**.

### Abstract Base Class (ABC)
- An **ABC** achieves abstraction by defining a **common interface** (blueprint) for its subclasses.
- Abstract classes **cannot be instantiated directly** (raises `TypeError`).
- Every concrete subclass **must implement all abstract methods**.

```python
from abc import ABC, abstractmethod

print("\n--- Abstract Base Class (ABC) ---")
class Greet(ABC):
    @abstractmethod
    def say_hello(self):
        pass                          # abstract method - no implementation

class English(Greet):
    def say_hello(self):
        return "Hello!"

g = English()
print(g.say_hello())

print("\n--- Abstract class rules ---")
try:
    Greet()                           # cannot instantiate ABC
except TypeError as err:
    print("Direct instantiation TypeError:", err)

class Incomplete(Greet):
    pass                              # forgot say_hello()

try:
    Incomplete()                      # cannot instantiate incomplete subclass
except TypeError as err:
    print("Incomplete subclass TypeError :", err)
```

**Output:**

```text

--- Abstract Base Class (ABC) ---
Hello!

--- Abstract class rules ---
Direct instantiation TypeError: Can't instantiate abstract class Greet without an implementation for abstract method 'say_hello'
Incomplete subclass TypeError : Can't instantiate abstract class Incomplete without an implementation for abstract method 'say_hello'
```

### Real-World Example: Vehicle / Car

```python
print("\n--- Example: Vehicle / Car ---")
class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass

    @abstractmethod
    def stop(self):
        pass

class Car(Vehicle):
    def start(self):
        print("Car engine started...")

    def stop(self):
        print("Car engine stopped...")

c = Car()
c.start()
c.stop()
print("Car is a Vehicle?", isinstance(c, Vehicle))
```

**Output:**

```text

--- Example: Vehicle / Car ---
Car engine started...
Car engine stopped...
Car is a Vehicle? True
```

## Summary – Encapsulation vs Abstraction

| Feature | Encapsulation | Abstraction |
|---------|---------------|-------------|
| **Focus** | Hiding data (protecting variables) | Hiding complexity (showing essential interface) |
| **How in Python** | Private variables (`__var`), getters/setters, `@property` | Abstract Base Classes (`ABC`) and `@abstractmethod` |
