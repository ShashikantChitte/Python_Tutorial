# Python Collections – Tuple

> [← Course index](../README.md)

## What is a tuple?

A tuple is a collection which is:
- **Ordered** – items have an index.
- **Unchangeable (immutable)** – you cannot modify, add, or remove items.

Tuples are written inside **round brackets** `( )`.

## Creating a Tuple

> ⚠️ A tuple with **one** item needs a trailing comma: `("apple",)`. Without the comma it is just a string in brackets.

```python
print("--- Creating a tuple ---")
mytuple = ("apple", "banana", "cherry")
print(mytuple, type(mytuple))

not_a_tuple = ("apple")
one_item = ("apple",)
print("Without comma:", type(not_a_tuple), "| With comma:", type(one_item))
```

**Output:**

```text
--- Creating a tuple ---
('apple', 'banana', 'cherry') <class 'tuple'>
Without comma: <class 'str'> | With comma: <class 'tuple'>
```

## Accessing Items & Range of Indexes

```python
print("\n--- Accessing & slicing items ---")
mytuple = ("apple", "banana", "cherry", "orange", "kiwi", "melon")
print("mytuple[1]     =", mytuple[1])
print("mytuple[-1]    =", mytuple[-1])
print("mytuple[2:5]   =", mytuple[2:5])
print("mytuple[-4:-1] =", mytuple[-4:-1])
```

**Output:**

```text

--- Accessing & slicing items ---
mytuple[1]     = banana
mytuple[-1]    = melon
mytuple[2:5]   = ('cherry', 'orange', 'kiwi')
mytuple[-4:-1] = ('cherry', 'orange', 'kiwi')
```

## Functions, Looping, Checking & Unpacking

```python
print("\n--- count(), index(), and len() ---")
nums = ("apple", "banana", "apple", "cherry")
print("count('apple') =", nums.count("apple"))
print("index('apple') =", nums.index("apple"))
print("len            =", len(nums))

print("\n--- Loop & check ---")
for item in ("apple", "banana", "cherry"):
    print(item)
if "banana" in nums:
    print("Yes, banana exists")

print("\n--- Tuple Unpacking ---")
coordinates = (10, 20, 30)
x, y, z = coordinates
print("Unpacked:", x, y, z)
```

**Output:**

```text

--- count(), index(), and len() ---
count('apple') = 2
index('apple') = 0
len            = 4

--- Loop & check ---
apple
banana
cherry
Yes, banana exists

--- Tuple Unpacking ---
Unpacked: 10 20 30
```

## Immutability & Workaround to Modify a Tuple

Things a tuple does **not** allow: direct item assignment (`mytuple[1] = ...`) or `.remove()`.
Since tuples are immutable, **convert to a list, modify, and convert back** (`tuple -> list -> tuple`).

```python
print("\n--- Tuples are immutable ---")
mytuple = ("apple", "banana", "cherry")
try:
    mytuple[1] = "orange"
except TypeError as err:
    print("TypeError:", err)

print("\n--- Workaround: tuple -> list -> tuple ---")
temp_list = list(mytuple)
temp_list[0] = "orange"
mytuple = tuple(temp_list)
print("back to a tuple:", mytuple)
```

**Output:**

```text

--- Tuples are immutable ---
TypeError: 'tuple' object does not support item assignment

--- Workaround: tuple -> list -> tuple ---
back to a tuple: ('orange', 'banana', 'cherry')
```

## Joining & Repeating Tuples

```python
print("\n--- Joining & repeating tuples ---")
tuple1 = ("a", "b", "c")
tuple2 = (1, 2, 3)
print("Joined  :", tuple1 + tuple2)
print("Repeated:", tuple1 * 2)
```

**Output:**

```text

--- Joining & repeating tuples ---
Joined  : ('a', 'b', 'c', 1, 2, 3)
Repeated: ('a', 'b', 'c', 'a', 'b', 'c')
```

## Summary – List vs Tuple

| | List | Tuple |
|---|------|-------|
| Mutability | **Mutable** (changeable) | **Immutable** (unchangeable) |
| Brackets | `[ ]` | `( )` |
| Use when… | you need to update items | data should stay constant |
