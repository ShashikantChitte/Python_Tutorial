# Practice Code – Object-Oriented Programming

> [← Course index](../README.md)

Classes, encapsulation, inheritance, polymorphism, dunder methods and classic data-structure classes. Each problem is independent and runnable.

**16 problems** · 5 Easy · 8 Medium · 3 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Bank account with encapsulation](#1-bank-account-with-encapsulation) | Easy |
| 2 | [Class variables versus instance variables](#2-class-variables-versus-instance-variables) | Easy |
| 3 | [Readable objects with __str__ and __repr__](#3-readable-objects-with-__str__-and-__repr__) | Easy |
| 4 | [Alternative constructor with @classmethod and a @staticmethod helper](#4-alternative-constructor-with-classmethod-and-a-staticmethod-helper) | Medium |
| 5 | [Validate data with a property setter](#5-validate-data-with-a-property-setter) | Medium |
| 6 | [Inheritance and method overriding](#6-inheritance-and-method-overriding) | Easy |
| 7 | [Polymorphism with an abstract base class](#7-polymorphism-with-an-abstract-base-class) | Medium |
| 8 | [An abstract class cannot be instantiated](#8-an-abstract-class-cannot-be-instantiated) | Medium |
| 9 | [Operator overloading with dunder methods](#9-operator-overloading-with-dunder-methods) | Medium |
| 10 | [Composition: a Car has an Engine](#10-composition-a-car-has-an-engine) | Medium |
| 11 | [Implement a stack using a class](#11-implement-a-stack-using-a-class) | Easy |
| 12 | [Implement a queue using two stacks](#12-implement-a-queue-using-two-stacks) | Medium |
| 13 | [Implement a linked list and reverse it](#13-implement-a-linked-list-and-reverse-it) | Medium |
| 14 | [Implement an LRU cache](#14-implement-an-lru-cache) | Hard |
| 15 | [Thread-safe singleton](#15-thread-safe-singleton) | Hard |
| 16 | [A context manager class using __enter__ and __exit__](#16-a-context-manager-class-using-__enter__-and-__exit__) | Hard |

---

## 1. Bank account with encapsulation

**Level:** Easy

**Problem:** Create an account whose balance cannot be changed directly and rejects invalid deposits or withdrawals.

**Explanation:** A double-underscore attribute (`__balance`) is name-mangled, so outside code cannot reach it by accident. The `balance` property exposes a read-only view, and the methods validate every change.

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.__balance = balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount

    def withdraw(self, amount):
        if amount > self.__balance:
            raise ValueError("Insufficient funds")
        self.__balance -= amount

    @property
    def balance(self):
        return self.__balance


account = BankAccount("Ann", 1000)
account.deposit(500)
account.withdraw(300)
print(account.balance)

try:
    account.withdraw(5000)
except ValueError as error:
    print("Error:", error)

# ------------Expected Output---------

# 1200
# Error: Insufficient funds
```

---

## 2. Class variables versus instance variables

**Level:** Easy

**Problem:** Count how many objects of a class have been created.

**Explanation:** A class variable (`count`) is shared by every instance, while `self.name` belongs to one object. Updating it through the class name (`Student.count`) changes the shared value.

```python
class Student:
    school = "ABC School"   # class variable
    count = 0               # class variable

    def __init__(self, name):
        self.name = name    # instance variable
        Student.count += 1


s1 = Student("Ann")
s2 = Student("Bob")
print(Student.count, s1.school, s2.name)

# ------------Expected Output---------

# 2 ABC School Bob
```

---

## 3. Readable objects with __str__ and __repr__

**Level:** Easy

**Problem:** Make `print(book)` friendly and make the debug representation unambiguous.

**Explanation:** `__str__` is used by `print()` and `str()`, `__repr__` is used by the console and for debugging. A good `__repr__` looks like the code that would recreate the object.

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

    def __str__(self):
        return f"{self.title} by {self.author}"

    def __repr__(self):
        return f"Book({self.title!r}, {self.author!r})"


book = Book("Clean Code", "Robert Martin")
print(book)
print(repr(book))

# ------------Expected Output---------

# Clean Code by Robert Martin
# Book('Clean Code', 'Robert Martin')
```

---

## 4. Alternative constructor with @classmethod and a @staticmethod helper

**Level:** Medium

**Problem:** Create a `Temperature` from Fahrenheit and add a helper that does not need an object.

**Explanation:** A `@classmethod` receives the class (`cls`) and is ideal for alternative constructors. A `@staticmethod` receives neither the class nor the instance – it is just a function grouped inside the class.

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    @classmethod
    def from_fahrenheit(cls, fahrenheit):
        return cls((fahrenheit - 32) * 5 / 9)

    @staticmethod
    def is_freezing(celsius):
        return celsius <= 0


t = Temperature.from_fahrenheit(212)
print(t.celsius)
print(Temperature.is_freezing(-5))

# ------------Expected Output---------

# 100.0
# True
```

---

## 5. Validate data with a property setter

**Level:** Medium

**Problem:** Reject negative ages when an attribute is set.

**Explanation:** A property lets you run code on attribute access. The setter validates the new value, so the object can never hold an invalid age even though callers still write `person.age = 30`.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age          # goes through the setter

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value


p = Person("Ann", 30)
p.age = 31
print(p.age)

try:
    p.age = -5
except ValueError as error:
    print("Error:", error)

# ------------Expected Output---------

# 31
# Error: Age cannot be negative
```

---

## 6. Inheritance and method overriding

**Level:** Easy

**Problem:** Create `Dog` and `Cat` classes that reuse an `Animal` base class but speak differently.

**Explanation:** Child classes inherit attributes and methods. `super().__init__()` runs the parent constructor, and redefining `speak()` in a child overrides the parent version.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name} makes a sound"


class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof"


class Cat(Animal):
    def __init__(self, name, indoor=True):
        super().__init__(name)
        self.indoor = indoor

    def speak(self):
        return f"{self.name} says Meow"


print(Animal("Generic").speak())
print(Dog("Rex").speak())
print(Cat("Tom").speak())

# ------------Expected Output---------

# Generic makes a sound
# Rex says Woof
# Tom says Meow
```

---

## 7. Polymorphism with an abstract base class

**Level:** Medium

**Problem:** Calculate the area of different shapes through one common interface.

**Explanation:** An abstract base class (`ABC`) declares `area()` as required. Each subclass implements it, and the loop calls `area()` on every shape without caring about its exact type – that is polymorphism.

```python
from abc import ABC, abstractmethod
import math


class Shape(ABC):
    @abstractmethod
    def area(self):
        ...


class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return math.pi * self.radius ** 2


class Rectangle(Shape):
    def __init__(self, width, height):
        self.width, self.height = width, height

    def area(self):
        return self.width * self.height


for shape in [Circle(3), Rectangle(4, 5)]:
    print(f"{type(shape).__name__}: {shape.area():.2f}")

# ------------Expected Output---------

# Circle: 28.27
# Rectangle: 20.00
```

---

## 8. An abstract class cannot be instantiated

**Level:** Medium

**Problem:** Show that creating an object of an abstract class fails.

**Explanation:** A class with an unimplemented `@abstractmethod` cannot be instantiated; Python raises `TypeError`. This forces subclasses to provide the missing behaviour.

```python
from abc import ABC, abstractmethod


class Payment(ABC):
    @abstractmethod
    def pay(self, amount):
        ...


try:
    Payment()
except TypeError as error:
    print(type(error).__name__)

# ------------Expected Output---------

# TypeError
```

---

## 9. Operator overloading with dunder methods

**Level:** Medium

**Problem:** Make two `Vector` objects addable with `+` and comparable with `==`.

**Explanation:** `__add__` defines `+`, `__eq__` defines `==` and `__repr__` controls how the object prints. Each returns a new object instead of changing the operands.

```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"


print(Vector(1, 2) + Vector(3, 4))
print(Vector(1, 2) == Vector(1, 2))
print(Vector(1, 2) == Vector(2, 1))

# ------------Expected Output---------

# Vector(4, 6)
# True
# False
```

---

## 10. Composition: a Car has an Engine

**Level:** Medium

**Problem:** Build a `Car` out of an `Engine` object instead of inheriting from it.

**Explanation:** Composition models a "has-a" relationship: the car holds an engine object and delegates work to it. It is more flexible than inheritance when the relationship is not "is-a".

```python
class Engine:
    def __init__(self, horsepower):
        self.horsepower = horsepower
        self.running = False

    def start(self):
        self.running = True
        return "Engine started"


class Car:
    def __init__(self, model, horsepower):
        self.model = model
        self.engine = Engine(horsepower)

    def start(self):
        return f"{self.model}: {self.engine.start()} ({self.engine.horsepower} hp)"


print(Car("Swift", 90).start())

# ------------Expected Output---------

# Swift: Engine started (90 hp)
```

---

## 11. Implement a stack using a class

**Level:** Easy

**Problem:** Build a `Stack` with `push`, `pop`, `peek`, `is_empty` and `len()` support.

**Explanation:** A list stores the items: `append()` pushes and `pop()` removes from the end (last in, first out). Raising `IndexError` on an empty stack mirrors how Python’s own list behaves.

```python
class Stack:
    def __init__(self):
        self._data = []

    def push(self, x): self._data.append(x)

    def pop(self):
        if self.is_empty():
            raise IndexError("pop from empty stack")
        return self._data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("peek from empty stack")
        return self._data[-1]

    def is_empty(self): return not self._data
    def __len__(self): return len(self._data)

stack = Stack()
for value in [1, 2, 3]:
    stack.push(value)

print(stack.peek())
print(stack.pop())
print(len(stack), stack.is_empty())

# ------------Expected Output---------

# 3
# 3
# 2 False
```

---

## 12. Implement a queue using two stacks

**Level:** Medium

**Problem:** Create a first-in-first-out queue using only two lists used as stacks.

**Explanation:** New items go onto the "in" stack. When a dequeue is needed and the "out" stack is empty, everything is moved across, which reverses the order so the oldest item comes out first.

```python
class QueueTwoStacks:
    def __init__(self):
        self._in, self._out = [], []

    def enqueue(self, x):
        self._in.append(x)

    def dequeue(self):
        if not self._out:
            while self._in:
                self._out.append(self._in.pop())
        if not self._out:
            raise IndexError("dequeue from empty queue")
        return self._out.pop()

q = QueueTwoStacks()
q.enqueue(1); q.enqueue(2)
print(q.dequeue(), q.dequeue())  # 1 2

# ------------Expected Output---------

# 1 2
```

---

## 13. Implement a linked list and reverse it

**Level:** Medium

**Problem:** Create a singly linked list with `append`, `reverse` and `to_list`.

**Explanation:** Each `Node` stores a value and a reference to the next node. Reversing walks the list once and flips every `next` pointer.

```python
class Node:
    def __init__(self, val, next=None):
        self.val, self.next = val, next

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, val):
        node = Node(val)
        if not self.head:
            self.head = node
            return
        cur = self.head
        while cur.next:
            cur = cur.next
        cur.next = node

    def reverse(self):
        prev, cur = None, self.head
        while cur:
            cur.next, prev, cur = prev, cur, cur.next
        self.head = prev

    def to_list(self):
        out, cur = [], self.head
        while cur:
            out.append(cur.val)
            cur = cur.next
        return out

ll = LinkedList()
for v in [1, 2, 3]:
    ll.append(v)
ll.reverse()
print(ll.to_list())  # [3, 2, 1]

# ------------Expected Output---------

# [3, 2, 1]
```

---

## 14. Implement an LRU cache

**Level:** Hard

**Problem:** Build a cache that evicts the least recently used item when it is full.

**Explanation:** `OrderedDict` remembers insertion order. Every access moves the key to the end, so the first key is always the least recently used and is dropped when the capacity is exceeded.

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.data = OrderedDict()

    def get(self, key):
        if key not in self.data:
            return -1
        self.data.move_to_end(key)
        return self.data[key]

    def put(self, key, value):
        self.data[key] = value
        self.data.move_to_end(key)
        if len(self.data) > self.cap:
            self.data.popitem(last=False)

c = LRUCache(2)
c.put(1, "a"); c.put(2, "b"); c.get(1); c.put(3, "c")
print(c.get(2))  # -1 (evicted)

# ------------Expected Output---------

# -1
```

---

## 15. Thread-safe singleton

**Level:** Hard

**Problem:** Make sure only one `ConfigManager` object ever exists.

**Explanation:** `__new__` creates the instance only the first time and stores it on the class. A lock plus a second check keeps this safe when several threads create the object at once.

```python
import threading

class ConfigManager:
    _instance = None
    _lock = threading.Lock()
    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    inst = super().__new__(cls)
                    inst.settings = {"env": "qa", "timeout": 10}
                    cls._instance = inst
        return cls._instance
    def get(self, key, default=None):
        return self.settings.get(key, default)

first = ConfigManager()
second = ConfigManager()

print(first is second)
print(first.get("env"), first.get("timeout"))

# ------------Expected Output---------

# True
# qa 10
```

---

## 16. A context manager class using __enter__ and __exit__

**Level:** Hard

**Problem:** Open a database connection that commits on success and rolls back when an error occurs.

**Explanation:** `__enter__` returns the connection for the `with` block. `__exit__` runs afterwards: it rolls back if an exception happened, otherwise commits, then closes the connection. Returning `False` lets exceptions continue to propagate.

```python
import sqlite3

class DBConnection:
    def __init__(self, path=":memory:"):
        self.path = path

    def __enter__(self):
        self.conn = sqlite3.connect(self.path)
        self.conn.row_factory = sqlite3.Row
        return self.conn

    def __exit__(self, exc_type, exc, tb):
        if exc_type:
            self.conn.rollback()
        else:
            self.conn.commit()
        self.conn.close()
        return False   # don't swallow exceptions

with DBConnection() as conn:
    conn.execute("CREATE TABLE users (id INTEGER, name TEXT)")
    conn.execute("INSERT INTO users VALUES (1, 'Ann')")
    row = conn.execute("SELECT * FROM users").fetchone()
    print(row["id"], row["name"])

# ------------Expected Output---------

# 1 Ann
```
