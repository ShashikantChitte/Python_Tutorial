# Hands-On Practice Exercises – Python Collections

> [← Course index](../README.md)

Practice problems covering **Lists, Sets, Tuples, and Dictionaries**. Try solving each task in the `# Your code here` cells before checking the **Solutions** section at the bottom.

## Part 1: Lists

**Exercise 1.1 – List Operations & Slicing**
1. Create a list `nums = [15, 42, 8, 23, 42, 4, 16]`.
2. Print the first and last element using indexing.
3. Print every second element using slicing.
4. Count how many times `42` appears and find the index of `23`.
5. Append `99`, remove `8`, sort in descending order, and print the final list.

```python
# 1. Create the list
nums = [15, 42, 8, 23, 42, 4, 16]

# 2. Print first and last element using indexing
print("First element:", nums[0])
print("Last element:", nums[-1])

# 3. Print every second element using slicing
print("Every second element:", nums[::2])

# 4. Count occurrences of 42 and find index of 23
print("Count of 42:", nums.count(42))
print("Index of 23:", nums.index(23))

# 5. Append 99, remove 8, sort descending, and print
nums.append(99)
nums.remove(8)
nums.sort(reverse=True)
print("Final list:", nums)
```

**Output:**

```text
First element: 15
Last element: 16
Every second element: [15, 8, 42, 16]
Count of 42: 2
Index of 23: 3
Final list: [99, 42, 42, 23, 16, 15, 4]
```

**Exercise 1.2 – Safe Copying**
1. Create `cart1 = ["milk", "bread", "eggs"]`.
2. Make a true copy named `cart2` (so modifying `cart2` does **not** change `cart1`).
3. Add `"butter"` to `cart2` and print both lists to verify they are different.

```python
# 1. Create the original list
cart1 = ["milk", "bread", "eggs"]

# 2. Make a true (shallow) copy so cart1 stays independent
cart2 = cart1.copy()          # alternatives: cart1[:]  or  list(cart1)

# 3. Modify the copy and verify they are different
cart2.append("butter")

print("cart1:", cart1)
print("cart2:", cart2)
```

**Output:**

```text
cart1: ['milk', 'bread', 'eggs']
cart2: ['milk', 'bread', 'eggs', 'butter']
```

## Part 2: Sets

**Exercise 2.1 – Remove Duplicates & Set Math**
1. Given `raw_ids = [101, 102, 103, 101, 104, 102, 105]`, convert it into a set `unique_ids` to remove duplicates.
2. Add `106` and safely discard `999` (without raising an error).
3. Given `batch_a = {"Alice", "Bob", "Charlie"}` and `batch_b = {"Charlie", "David", "Eve"}`, find:
   - All students across both batches (**Union**)
   - Students present in both batches (**Intersection**)
   - Students only in `batch_a` (**Difference**)

```python
# 1. Convert the list to a set to remove duplicates
raw_ids = [101, 102, 103, 101, 104, 102, 105]
unique_ids = set(raw_ids)
print("unique_ids:", unique_ids)

# 2. Add 106 and safely discard 999 (no error if absent)
unique_ids.add(106)
# discard() never raises; remove() would raise KeyError
unique_ids.discard(999)
print("After add/discard:", unique_ids)

# 3. Set operations on batches
batch_a = {"Alice", "Bob", "Charlie"}
batch_b = {"Charlie", "David", "Eve"}

print("Union:",        batch_a | batch_b)   # or batch_a.union(batch_b)
print("Intersection:", batch_a & batch_b)   # or batch_a.intersection(batch_b)
print("Only in A:",    batch_a - batch_b)   # or batch_a.difference(batch_b)
```

**Output:**

```text
unique_ids: {101, 102, 103, 104, 105}
After add/discard: {101, 102, 103, 104, 105, 106}
Union: {'Bob', 'Charlie', 'Eve', 'David', 'Alice'}
Intersection: {'Charlie'}
Only in A: {'Bob', 'Alice'}
```

## Part 3: Tuples

**Exercise 3.1 – Unpacking & Tuple Workaround**
1. Create a single-item tuple `singleton = ("Python",)` and print its `type()`.
2. Create `point = (10, 20, 30)` and unpack it into `x`, `y`, and `z`.
3. Given `colors = ("red", "green", "blue")`, change `"green"` to `"yellow"` using the `tuple -> list -> tuple` workaround and print the updated tuple.

```python
# 1. Single-item tuple (the trailing comma is essential)
singleton = ("Python",)
print("singleton:", singleton)
print("type:", type(singleton))        # <class 'tuple'>

# 2. Unpack a 3-tuple into x, y, z
point = (10, 20, 30)
x, y, z = point
print("x =", x, "y =", y, "z =", z)

# 3. Tuple -> list -> tuple workaround to change "green" to "yellow"
colors = ("red", "green", "blue")
temp = list(colors)                    # tuples are immutable, so convert
temp[1] = "yellow"                     # modify by index
colors = tuple(temp)                   # convert back
print("updated colors:", colors)
```

**Output:**

```text
singleton: ('Python',)
type: <class 'tuple'>
x = 10 y = 20 z = 30
updated colors: ('red', 'yellow', 'blue')
```

## Part 4: Dictionaries

**Exercise 4.1 – Student Record Management**
1. Create a dictionary `student = {"name": "Aarav", "age": 20, "course": "Python"}`.
2. Safely fetch `"grade"` using `.get()` with a default value of `"Not Assigned"`.
3. Update `"age"` to `21` and add `"city": "Pune"`.
4. Loop through `.items()` and print each `key -> value` pair.
5. Remove `"course"` using `.pop()` and print the updated dictionary.

```python
# 1. Create the dictionary
student = {"name": "Aarav", "age": 20, "course": "Python"}
print("Initial:", student)

# 2. Safely fetch "grade" with a default
grade = student.get("grade", "Not Assigned")
print("Grade:", grade)                      # "Not Assigned" (key absent)

# 3. Update age and add a new key
student["age"] = 21
student["city"] = "Pune"
print("After update:", student)

# 4. Loop through key -> value pairs
for key, value in student.items():
    print(f"{key} -> {value}")

# 5. Remove "course" with .pop() and print the result
removed = student.pop("course")
print("Removed:", removed)
print("Final:", student)
```

**Output:**

```text
Initial: {'name': 'Aarav', 'age': 20, 'course': 'Python'}
Grade: Not Assigned
After update: {'name': 'Aarav', 'age': 21, 'course': 'Python', 'city': 'Pune'}
name -> Aarav
age -> 21
course -> Python
city -> Pune
Removed: Python
Final: {'name': 'Aarav', 'age': 21, 'city': 'Pune'}
```

**Exercise 4.2 – Mini Challenge: Word Frequency Counter**
Given `words = ["apple", "banana", "apple", "cherry", "banana", "apple"]`, use a dictionary to count how many times each word appears.
*Expected output:* `{'apple': 3, 'banana': 2, 'cherry': 1}`

```python
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]

# Build the frequency dictionary
freq = {}
for word in words:
    freq[word] = freq.get(word, 0) + 1

print(freq)
```

**Output:**

```text
{'apple': 3, 'banana': 2, 'cherry': 1}
```
