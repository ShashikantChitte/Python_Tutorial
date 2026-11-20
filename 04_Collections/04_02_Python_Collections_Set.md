# Python Collections – Set

> [← Course index](../README.md)

## What is a set?

A set in Python is a collection that has special properties:
- **Unordered** – items do not have a fixed position (no indexing).
- **Mutable** – we can add or remove items (but cannot access or change by index).
- **No duplicates allowed** – duplicate values are automatically removed.
- Written with **curly braces** `{ }`.

## Creating a Set

> ⚠️ `True` equals `1` in Python, so a set cannot hold both: `{1, True}` has only **one** item.
> ⚠️ `{}` creates an empty dictionary; use `set()` for an empty set.

```python
print("--- Creating a set ---")
myset1 = {10, 20, 30, 40, 50}
myset2 = {"apple", "mango", "cherry"}
myset3 = {10, "apple", "A", True}        # mixed data types (True ignored because 10 != True, wait: 1 == True)
myset4 = set()                           # empty set
print(sorted(myset1))
print(sorted(myset2))
print(len(myset3), "items in myset3")
print(type({}), "|", type(set()))
print("No duplicates:", {1, 2, 3, 1, 2})
print("{1, True} length:", len({1, True}))
```

**Output:**

```text
--- Creating a set ---
[10, 20, 30, 40, 50]
['apple', 'cherry', 'mango']
4 items in myset3
<class 'dict'> | <class 'set'>
No duplicates: {1, 2, 3}
{1, True} length: 1
```

## Accessing Items & Checking Membership

- You **cannot** access or update items by index (sets are unordered).
- You **can loop** through the set and check membership with `in`.

```python
print("\n--- No indexing, but you can loop ---")
myset = {"apple", "banana", "cherry"}
try:
    myset[0]
except TypeError as err:
    print("TypeError:", err)

for x in sorted(myset):
    print(x)

print("'banana' in myset ->", "banana" in myset)
print("len(myset) ->", len(myset))
```

**Output:**

```text

--- No indexing, but you can loop ---
TypeError: 'set' object is not subscriptable
apple
banana
cherry
'banana' in myset -> True
len(myset) -> 3
```

## Adding & Removing Items

| Method | Behaviour |
|--------|-----------|
| `add(value)` | adds a single item |
| `update(iterable)` | adds multiple items |
| `remove(value)` | removes the item, **error** if not found (`KeyError`) |
| `discard(value)` | removes the item, **no error** if not found |
| `pop()` | removes an **arbitrary** item (sets are unordered) |

```python
print("\n--- Adding items: add() and update() ---")
myset = {"apple", "banana", "cherry"}
myset.add("orange")
print("add('orange')             :", sorted(myset))
myset.update(["mango", "grapes"])
print("update(['mango','grapes']):", sorted(myset))

print("\n--- Removing items ---")
myset.remove("banana")
print("remove('banana'):", sorted(myset))
try:
    myset.remove("xyz")
except KeyError as err:
    print("remove('xyz')  : KeyError", err)
myset.discard("xyz")
print("discard('xyz') : no error")

removed = myset.pop()
print("pop() removed:", removed, "| remaining:", sorted(myset))
```

**Output:**

```text

--- Adding items: add() and update() ---
add('orange')             : ['apple', 'banana', 'cherry', 'orange']
update(['mango','grapes']): ['apple', 'banana', 'cherry', 'grapes', 'mango', 'orange']

--- Removing items ---
remove('banana'): ['apple', 'cherry', 'grapes', 'mango', 'orange']
remove('xyz')  : KeyError 'xyz'
discard('xyz') : no error
pop() removed: grapes | remaining: ['apple', 'cherry', 'mango', 'orange']
```

## Copying, Clearing & Deleting a Set

```python
print("\n--- Copying, clear() and del ---")
myset1 = {"apple", "banana", "cherry"}
myset2 = myset1.copy()
myset3 = set(myset1)
print("Copies:", sorted(myset2), sorted(myset3))

myset1.clear()
print("after clear():", myset1)
del myset1
print("after del, 'myset1' exists?", "myset1" in globals())
```

**Output:**

```text

--- Copying, clear() and del ---
Copies: ['apple', 'banana', 'cherry'] ['apple', 'banana', 'cherry']
after clear(): set()
after del, 'myset1' exists? False
```

## Set Operations (Union, Intersection, Difference)

- **Union (`|` or `.union()`)** → combines all unique items from both sets.
- **Intersection (`&` or `.intersection()`)** → returns only common items.
- **Difference (`-` or `.difference()`)** → items in the first set but not the second.
- **Symmetric Difference (`^` or `.symmetric_difference()`)** → items in either set, but not both.

```python
print("\n--- Set Operations ---")
set1 = {"apple", "banana", "cherry"}
set2 = {"google", "microsoft", "apple"}

print("Union (|)          :", sorted(set1 | set2))
print("Intersection (&)   :", sorted(set1 & set2))
print("Difference (-)     :", sorted(set1 - set2))
print("Symmetric Diff (^) :", sorted(set1 ^ set2))
```

**Output:**

```text

--- Set Operations ---
Union (|)          : ['apple', 'banana', 'cherry', 'google', 'microsoft']
Intersection (&)   : ['apple']
Difference (-)     : ['banana', 'cherry']
Symmetric Diff (^) : ['banana', 'cherry', 'google', 'microsoft']
```

## Summary

- Set is **unordered** → no indexing, sorting, or reversing.
- **Mutable** → you can add or remove items.
- **No duplicates** → each item is unique.
