# Python Collections – List

> [← Course index](../README.md)

## What is a list?

A list is a collection which is:
- **Ordered** – items have a defined order and can be accessed using indexes.
- **Changeable (mutable)** – you can add, update, or remove items.
- **Allows duplicates** – the same value can appear many times.

Lists are written inside **square brackets** `[ ]`.

## Creating a List

```python
print("--- Creating a list ---")
mylist1 = [10, 20, 30, 40, 50]
mylist2 = ["apple", "banana", "cherry"]
mylist3 = ["apple", 10, "banana", 20]       # mixed data types
mylist4 = list()                            # empty list (or [])
print(mylist1)
print(mylist2)
print(mylist3)
print(mylist4)
```

**Output:**

```text
--- Creating a list ---
[10, 20, 30, 40, 50]
['apple', 'banana', 'cherry']
['apple', 10, 'banana', 20]
[]
```

## Accessing List Items & Slicing

- Indexing starts from `0`.
- Negative indexing starts from `-1` (the last element).
- Slicing uses `[start:stop:step]` where `stop` is excluded.

```python
print("\n--- Accessing & slicing items ---")
mylist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
print("mylist[0]     =", mylist[0])
print("mylist[-1]    =", mylist[-1])
print("mylist[2:5]   =", mylist[2:5])
print("mylist[-4:-1] =", mylist[-4:-1])
print("mylist[::2]   =", mylist[::2])      # every 2nd item
```

**Output:**

```text

--- Accessing & slicing items ---
mylist[0]     = apple
mylist[-1]    = mango
mylist[2:5]   = ['cherry', 'orange', 'kiwi']
mylist[-4:-1] = ['orange', 'kiwi', 'melon']
mylist[::2]   = ['apple', 'cherry', 'kiwi', 'mango']
```

## Changing an Item Value

```python
print("\n--- Changing an item value ---")
mylist = ["apple", "banana", "cherry"]
print("before:", mylist)
mylist[1] = "orange"
print("after :", mylist)
```

**Output:**

```text

--- Changing an item value ---
before: ['apple', 'banana', 'cherry']
after : ['apple', 'orange', 'cherry']
```

## Loop Through a List & Check if an Item Exists

```python
print("\n--- Loop through a list ---")
mylist = ["apple", "banana", "cherry"]
for item in mylist:
    print(item)

print("\n--- Check if an item exists ---")
if "apple" in mylist:
    print("Yes, apple is present")
print("'kiwi' in mylist ->", "kiwi" in mylist)
```

**Output:**

```text

--- Loop through a list ---
apple
banana
cherry

--- Check if an item exists ---
Yes, apple is present
'kiwi' in mylist -> False
```

## List Functions: `len()`, `count()`, and `index()`

```python
print("\n--- len(), count(), and index() ---")
nums = [10, 20, 30, 10, 40, 10]
print("len   :", len(nums))
print("count :", nums.count(10))
print("index :", nums.index(30))      # index of first match
```

**Output:**

```text

--- len(), count(), and index() ---
len   : 6
count : 3
index : 2
```

## Sorting & Reversing

> **Note:** You **cannot sort heterogeneous data** (mixed types).
> 💡 `reverse()` simply flips the current order – the list does **not** need to be sorted first.

```python
print("\n--- Sorting & reversing ---")
mylist = ["cherry", "mango", "banana", "apple"]
mylist.sort()
print("sort()             :", mylist)
mylist.sort(reverse=True)
print("sort(reverse=True) :", mylist)
mylist.reverse()
print("reverse()          :", mylist)

mixed = ["a", 1, "x"]
try:
    mixed.sort()
except TypeError as err:
    print("TypeError:", err)
```

**Output:**

```text

--- Sorting & reversing ---
sort()             : ['apple', 'banana', 'cherry', 'mango']
sort(reverse=True) : ['mango', 'cherry', 'banana', 'apple']
reverse()          : ['apple', 'banana', 'cherry', 'mango']
TypeError: '<' not supported between instances of 'int' and 'str'
```

## Adding & Removing Items

| Way | Removes by | Notes |
|-----|-----------|-------|
| `remove(value)` | value | first match only |
| `pop(index)` | index | returns the removed item; no index → last item |
| `del list[index]` | index | `del list` deletes the whole list |
| `clear()` | – | empties the list, keeps the variable |

```python
print("\n--- Adding items: append() and insert() ---")
mylist = ["apple", "banana"]
mylist.append("cherry")          # at the end
mylist.insert(1, "orange")       # at index 1
print("after add:", mylist)

print("\n--- Removing items ---")
mylist.remove("banana")          # by value
print("remove('banana') :", mylist)

removed = mylist.pop(0)          # by index (returns removed item)
print("pop(0)           :", mylist, "| removed:", removed)

del mylist[0]                    # del keyword
print("del mylist[0]    :", mylist)

mylist.clear()
print("clear()          :", mylist)

del mylist                       # deletes the whole list variable
print("after 'del mylist' it exists?", "mylist" in globals())
```

**Output:**

```text

--- Adding items: append() and insert() ---
after add: ['apple', 'orange', 'banana', 'cherry']

--- Removing items ---
remove('banana') : ['apple', 'orange', 'cherry']
pop(0)           : ['orange', 'cherry'] | removed: apple
del mylist[0]    : ['cherry']
clear()          : []
after 'del mylist' it exists? False
```

## Copying & Joining Lists

> ⚠️ `mylist2 = mylist1` does **not** copy – both names point to the **same** list.
> **Key point:** Lists are **mutable** → you can modify them anytime.

```python
print("\n--- Copying a list ---")
mylist1 = ["apple", "banana"]
mylist2 = mylist1.copy()         # approach 1
mylist3 = list(mylist1)          # approach 2
mylist1.append("cherry")
print("original:", mylist1)
print("copy()  :", mylist2)
print("list()  :", mylist3)

print("\n--- Joining lists ---")
list1 = ["a", "b", "c"]
list2 = [1, 2, 3]
print("approach 1 (+)      :", list1 + list2)
list1.extend(list2)
print("approach 2 (extend) :", list1)
```

**Output:**

```text

--- Copying a list ---
original: ['apple', 'banana', 'cherry']
copy()  : ['apple', 'banana']
list()  : ['apple', 'banana']

--- Joining lists ---
approach 1 (+)      : ['a', 'b', 'c', 1, 2, 3]
approach 2 (extend) : ['a', 'b', 'c', 1, 2, 3]
```
