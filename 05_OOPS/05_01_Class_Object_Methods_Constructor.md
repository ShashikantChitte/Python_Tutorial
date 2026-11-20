# Python Class, Object, Method, Static Method, and Constructor

> [← Course index](../README.md)

Python is an object-oriented programming (OOP) language. The main building blocks of OOP in Python are **classes, objects, methods, and constructors**.

## Class & Object

- **Class**: A **blueprint or template** to create objects. It contains variables (data) and methods (functions).
- **Object**: An **instance of a class**. You can create multiple independent objects from the same class.

```python
print("--- Class and object ---")
class MyClass:
    def myfun(self):
        pass                          # simple method, does nothing

    def display(self, name):
        print(name)

mc1 = MyClass()                       # object of MyClass
mc1.myfun()
mc1.display("scott")

obj1 = MyClass()
obj2 = MyClass()                      # many objects from one class
print("obj1 is obj2?", obj1 is obj2, "| both are", type(obj1).__name__)
```

**Output:**

```text
--- Class and object ---
scott
obj1 is obj2? False | both are MyClass
```

## Types of Methods

| Method Type | Decorator | First Parameter | How to Call |
|-------------|-----------|-----------------|-------------|
| **Instance Method** | None | `self` (refers to the object) | Using an object (`obj.m1()`) |
| **Class Method** | `@classmethod` | `cls` (refers to the class) | Using class name or object (`Demo.m2()`) |
| **Static Method** | `@staticmethod` | None (regular parameters) | Using class name or object (`Calc.m3()`) |

```python
print("\n--- Instance, Class, and Static methods ---")
class Demo:
    company = "TechCorp"              # class variable

    def m1(self):                     # instance method (needs object)
        print("Instance method called by:", self)

    @classmethod
    def m2(cls):                      # class method (can access class variables)
        print("Class method -> company:", cls.company)

    @staticmethod
    def m3(x, y):                     # static method (no self or cls)
        print("Static method -> sum:", x + y)

d = Demo()
d.m1()
Demo.m2()                             # called via class name
Demo.m3(10, 20)                       # called via class name
d.m3(100, 200)                        # also callable via object
```

**Output:**

```text

--- Instance, Class, and Static methods ---
Instance method called by: <__main__.Demo object at 0x7ff238183590>
Class method -> company: TechCorp
Static method -> sum: 30
Static method -> sum: 300
```

## Class Variables vs Instance Variables vs Local Variables

| Kind | Where declared | Shared? |
|------|----------------|---------|
| **Class variable** | inside the class, outside methods | shared by **all** objects |
| **Instance variable** | using `self.` inside methods | **different** for each object |
| **Local variable** | inside a method (normal function variable) | only inside that method |

```python
print("\n--- Class vs Local vs Global variables ---")
i, j = 15, 25                         # global variables

class Vars:
    a, b = 10, 20                     # class variables

    def add(self, x, y):
        print("local  :", x + y)           # 300
        print("class  :", self.a + self.b) # 30
        print("global :", i + j)           # 40

Vars().add(100, 200)

print("\n--- Same names for local, class and global variables ---")
a, b = 15, 25                         # global

class SameNames:
    a, b = 10, 20                     # class

    def add(self, a, b):              # local (parameters)
        print("local  :", a + b)
        print("class  :", self.a + self.b)
        print("global :", globals()["a"] + globals()["b"])

SameNames().add(100, 200)
```

**Output:**

```text

--- Class vs Local vs Global variables ---
local  : 300
class  : 30
global : 40

--- Same names for local, class and global variables ---
local  : 300
class  : 30
global : 40
```

```python
print("\n--- Class variable is shared, instance variable is per object ---")
class Counter:
    shared = 0                        # class variable

    def __init__(self, name):
        self.name = name              # instance variable

c1, c2 = Counter("first"), Counter("second")
Counter.shared = 5                    # change it once ...
print(c1.name, c1.shared, "|", c2.name, c2.shared, "(both see the change)")
```

**Output:**

```text

--- Class variable is shared, instance variable is per object ---
first 5 | second 5 (both see the change)
```

## Constructor (`__init__`)

- A special method (`__init__`) that is **called automatically** when an object is created.
- Used to **initialise instance variables** (similar to constructors in Java or C++).
- Can also take **default parameter values**.

```python
print("\n--- Constructor __init__ (runs automatically) ---")
class WithInit:
    def __init__(self):
        print("this is constructor..")

    def m1(self):
        print("hello...")

w = WithInit()                        # constructor invoked automatically
w.m1()                                # normal method called explicitly

print("\n--- Constructor with parameters ---")
class Emp:
    def __init__(self, eid, ename, sal=300000):  # default sal if not provided
        self.eid = eid
        self.ename = ename
        self.sal = sal

    def display(self):
        print(self.eid, self.ename, self.sal)

e1 = Emp(101, "John", 500000)
e2 = Emp(102, "David")                # uses default sal
e1.display()
e2.display()
```

**Output:**

```text

--- Constructor __init__ (runs automatically) ---
this is constructor..
hello...

--- Constructor with parameters ---
101 John 500000
102 David 300000
```

## `__str__()` Method

- A special method that defines **how an object is represented as a string**.
- When you use `print(object)`, Python calls `__str__()` internally.

```python
print("\n--- __str__ (what print(object) shows) ---")
class EmpWithStr(Emp):
    def __str__(self):
        return f"{self.ename} (ID: {self.eid})"   # must return a string

print(EmpWithStr(101, "John", 500000))
print("without __str__ the output looks like:", str(e1).split(" at ")[0] + " at 0x...>")
```

**Output:**

```text

--- __str__ (what print(object) shows) ---
John (ID: 101)
without __str__ the output looks like: <__main__.Emp object at 0x...>
```

## Key Takeaways

1. **Class** = blueprint, **Object** = real entity created from the class.
2. **Instance methods** use `self` and need an object; **class methods** use `@classmethod` and `cls`; **static methods** use `@staticmethod`.
3. **Class variables** are shared across all objects; **instance variables** are unique per object.
4. **Constructor (`__init__`)** runs automatically when an object is created.
5. **`__str__()`** defines the human-readable string representation of an object.
