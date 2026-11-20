# Inheritance and Polymorphism in Python

> [← Course index](../README.md)

## Part 1 – Inheritance

- Inheritance lets a class (**child / derived**) reuse the properties and methods of another class (**parent / base**).
- Promotes **code reusability** and **extensibility**.

### Types of Inheritance in Python

| Type | Meaning |
|------|---------|
| **Single** | one child inherits from one parent (`A → B`) |
| **Multilevel** | a class inherits from another *derived* class (`A → B → C`) |
| **Hierarchical** | multiple children inherit from the same parent (`A → B` and `A → C`) |
| **Multiple** | a child inherits from more than one parent (`A, B → C`) |

## Example 1: Single Inheritance

```python
print("--- Example 1: single inheritance ---")
class A:
    x, y = 10, 20
    def m1(self):
        print("Parent m1:", self.x + self.y)

class B(A):                         # B inherits from A
    a, b = 200, 100
    def m2(self):
        print("Child m2 :", self.a - self.b)

bobj = B()
bobj.m1()                           # 30 (from parent)
bobj.m2()                           # 100 (from child)
```

**Output:**

```text
--- Example 1: single inheritance ---
Parent m1: 30
Child m2 : 100
```

## Example 2: Multilevel Inheritance (`A → B → C`)

```python
print("\n--- Example 2: multilevel inheritance ---")
class A:
    x, y = 10, 20
    def m1(self):
        print(self.x + self.y)

class B(A):
    a, b = 200, 100
    def m2(self):
        print(self.a - self.b)

class C(B):
    i, j = 5, 2
    def m3(self):
        print(self.i * self.j)

cobj = C()
cobj.m1()                           # 30
cobj.m2()                           # 100
cobj.m3()                           # 10
```

**Output:**

```text

--- Example 2: multilevel inheritance ---
30
100
10
```

## Example 3: Hierarchical Inheritance (`B` and `C` share parent `A`)

```python
print("\n--- Example 3: hierarchical inheritance ---")
class A:
    x, y = 10, 20
    def m1(self):
        print(self.x + self.y)

class B(A):
    a, b = 200, 100
    def m2(self):
        print(self.a - self.b)

class C(A):
    i, j = 5, 2
    def m3(self):
        print(self.i * self.j)

bobj = B()
bobj.m1(); bobj.m2()
cobj = C()
cobj.m1(); cobj.m3()
```

**Output:**

```text

--- Example 3: hierarchical inheritance ---
30
100
30
10
```

## Example 4: Multiple Inheritance (`C` inherits from both `A` and `B`)

> Python searches parents left to right using **Method Resolution Order (MRO)**: `C.__mro__`.

```python
print("\n--- Example 4: multiple inheritance ---")
class A:
    x, y = 10, 20
    def m1(self):
        print(self.x + self.y)

class B:
    a, b = 200, 100
    def m2(self):
        print(self.a - self.b)

class C(A, B):                      # inherits from both
    i, j = 5, 2
    def m3(self):
        print(self.i * self.j)

cobj = C()
cobj.m1()
cobj.m2()
cobj.m3()
print("Method Resolution Order (MRO):", [k.__name__ for k in C.__mro__])
```

**Output:**

```text

--- Example 4: multiple inheritance ---
30
100
10
Method Resolution Order (MRO): ['C', 'A', 'B', 'object']
```

## Example 5: Using `super()` (Methods & Constructors)

> `super()` calls the parent class method or `__init__` constructor from the child class.

```python
print("\n--- Example 5: super() with methods and constructors ---")
class Person:
    def __init__(self, name):
        self.name = name

    def show(self):
        print("Name:", self.name)

class Employee(Person):
    def __init__(self, name, emp_id):
        super().__init__(name)      # calls Person.__init__
        self.emp_id = emp_id

    def show(self):
        super().show()              # calls Person.show
        print("Emp ID:", self.emp_id)

emp = Employee("Alice", 101)
emp.show()
```

**Output:**

```text

--- Example 5: super() with methods and constructors ---
Name: Alice
Emp ID: 101
```

## Example 6: Accessing Parent Variables

```python
print("\n--- Example 6: accessing parent variables ---")
class A:
    a, b = 10, 20

class B(A):
    i, j = 100, 200
    def m(self, x, y):
        print("Local :", x + y)
        print("Child :", self.i + self.j)
        print("Parent:", self.a + self.b)

B().m(1000, 2000)
```

**Output:**

```text

--- Example 6: accessing parent variables ---
Local : 3000
Child : 300
Parent: 30
```

## Part 2 – Polymorphism

- Polymorphism means **"many forms"** — the same method or operator name behaves differently based on context.

1. **Variable & Method Overriding** – child class redefines a parent variable or method.
2. **Method Overloading (simulated)** – same method name with different numbers of arguments using default values or `*args`.
3. **Operator Overloading** – built-in operators like `+` work differently on numbers (`2 + 3 = 5`) vs strings (`"a" + "b" = "ab"`).

## Example 7: Variable & Method Overriding

```python
print("\n--- Example 7a: variable overriding ---")
class Parent:
    name = "Scott"

class Child(Parent):
    name = "John"                   # overrides parent variable
    def parent_name(self):
        return super().name

child = Child()
print(child.name, "| parent's value is still", child.parent_name())

print("\n--- Example 7b: method overriding ---")
class Bank:
    def rateOfInterest(self):
        return 0

class XBank(Bank):
    def rateOfInterest(self):
        return 10.5

class YBank(Bank):
    def rateOfInterest(self):
        return 12.5

for bank in (Bank(), XBank(), YBank()):
    print(type(bank).__name__, "->", bank.rateOfInterest())
```

**Output:**

```text

--- Example 7a: variable overriding ---
John | parent's value is still Scott

--- Example 7b: method overriding ---
Bank -> 0
XBank -> 10.5
YBank -> 12.5
```

## Example 8: Method Overloading (Simulated)

```python
print("\n--- Example 8: method overloading (simulated) ---")
class Human:
    def sayHello(self, name=None):
        if name is not None:
            print("Hello " + name)
        else:
            print("Hello")

h = Human()
h.sayHello()
h.sayHello("David")

class Calculation:
    def add(self, a=0, b=0, c=0):
        print(a + b + c)

calobj = Calculation()
calobj.add()                        # 0
calobj.add(10, 20)                  # 30
calobj.add(100, 200, 300)           # 600
```

**Output:**

```text

--- Example 8: method overloading (simulated) ---
Hello
Hello David
0
30
600
```

## Summary

- **Inheritance** → reuse parent class methods/variables (`Single`, `Multilevel`, `Hierarchical`, `Multiple`).
- **`super()`** → access parent methods, variables, or `__init__` constructor.
- **Polymorphism** → same interface, different behaviours (variable/method overriding & simulated overloading).
