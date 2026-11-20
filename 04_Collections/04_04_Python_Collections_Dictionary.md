# Python Dictionary

> [← Course index](../README.md)

A dictionary in Python is a collection of data stored in **`key : value`** pairs.

**Properties of a dictionary**
- **Ordered** (from Python 3.7+) – items maintain insertion order.
- **Mutable (changeable)** – we can add, update, or remove items.
- **Indexed by keys** – values are accessed using keys (not positions).
- **No duplicate keys** – each key must be unique.
- Written using **curly braces** `{ }`.

## 1. Creating a Dictionary

- **Approach 1:** using `{}`
- **Approach 2:** using the `dict()` constructor
- Values can be of **any data type**; duplicate keys overwrite earlier values.

```python
print("--- 1. Creating a dictionary ---")
mydic = {"brand": "Ford", "model": "Aspire", "year": 2024}           # approach 1: {}
print(mydic)

mydic = dict(name="John", age=36, country="Norway")                  # approach 2: dict()
print(mydic)

mydic = {
    "brand": "Ford",
    "electric": True,
    "year": 2025,
    "colors": ["red", "white", "blue"],       # values can be of any type
}
print(mydic)
print("Duplicate key check:", {"brand": "Ford", "brand": "Hyundai"})
```

**Output:**

```text
--- 1. Creating a dictionary ---
{'brand': 'Ford', 'model': 'Aspire', 'year': 2024}
{'name': 'John', 'age': 36, 'country': 'Norway'}
{'brand': 'Ford', 'electric': True, 'year': 2025, 'colors': ['red', 'white', 'blue']}
Duplicate key check: {'brand': 'Hyundai'}
```

## 2. Accessing Items

> `mydic["missing"]` raises `KeyError`, while `mydic.get("missing")` quietly returns `None` (or a custom default).

```python
print("\n--- 2. Accessing items ---")
mydic = {"brand": "Ford", "model": "Aspire", "year": 2024}
print('mydic["model"]              =', mydic["model"])
print('mydic.get("model")          =', mydic.get("model"))
print('mydic.get("missing")        =', mydic.get("missing"))
print('mydic.get("color", "N/A")   =', mydic.get("color", "N/A"))
try:
    mydic["missing"]
except KeyError as err:
    print('mydic["missing"] -> KeyError:', err)
```

**Output:**

```text

--- 2. Accessing items ---
mydic["model"]              = Aspire
mydic.get("model")          = Aspire
mydic.get("missing")        = None
mydic.get("color", "N/A")   = N/A
mydic["missing"] -> KeyError: 'missing'
```

## 3. Keys, Values, Items & Key Existence

```python
print("\n--- 3. keys(), values(), items(), len() ---")
print("Keys  :", mydic.keys())
print("Values:", mydic.values())
print("Items :", mydic.items())
print("Length:", len(mydic))

print("\n--- Check if a key exists ---")
if "model" in mydic:
    print("Yes, 'model' exists")
print('"color" in mydic ->', "color" in mydic)
```

**Output:**

```text

--- 3. keys(), values(), items(), len() ---
Keys  : dict_keys(['brand', 'model', 'year'])
Values: dict_values(['Ford', 'Aspire', 2024])
Items : dict_items([('brand', 'Ford'), ('model', 'Aspire'), ('year', 2024)])
Length: 3

--- Check if a key exists ---
Yes, 'model' exists
"color" in mydic -> False
```

## 4. Adding & Updating Items

```python
print("\n--- 4. Adding & updating items ---")
mydic["color"] = "red"                       # adds a new key
mydic.update({"year": 2025, "color": "white"})  # updates existing keys
mydic.setdefault("fuel", "Petrol")           # adds only if key does not exist
print("updated:", mydic)
```

**Output:**

```text

--- 4. Adding & updating items ---
updated: {'brand': 'Ford', 'model': 'Aspire', 'year': 2025, 'color': 'white', 'fuel': 'Petrol'}
```

## 5. Removing Items

| Way | What it does |
|-----|--------------|
| `pop(key)` | removes the specific key |
| `popitem()` | removes the **last inserted** item (random in < 3.7) |
| `del mydic[key]` | removes a specific key |
| `del mydic` | deletes the **entire** dictionary |
| `clear()` | empties the dictionary |

```python
print("\n--- 5. Removing items ---")
mydic = {"brand": "Ford", "model": "Aspire", "year": 2024}
mydic.pop("model")
print("pop('model')   :", mydic)

mydic = {"brand": "Ford", "model": "Aspire", "year": 2024}
last = mydic.popitem()
print("popitem()      :", mydic, "| removed:", last)

mydic = {"brand": "Ford", "model": "Aspire", "year": 2024}
del mydic["model"]
print("del mydic[...] :", mydic)

mydic.clear()
print("clear()        :", mydic)

mydic = {"brand": "Ford"}
del mydic
print("after 'del mydic' it exists?", "mydic" in globals())
```

**Output:**

```text

--- 5. Removing items ---
pop('model')   : {'brand': 'Ford', 'year': 2024}
popitem()      : {'brand': 'Ford', 'model': 'Aspire'} | removed: ('year', 2024)
del mydic[...] : {'brand': 'Ford', 'year': 2024}
clear()        : {}
after 'del mydic' it exists? False
```

## 6. Copying, Looping & Nested Dictionaries

```python
print("\n--- 6. Copying a dictionary ---")
mydic1 = {"brand": "Ford", "model": "Aspire", "year": 2024}
mydic2 = mydic1.copy()                       # approach 1
mydic3 = dict(mydic1)                        # approach 2
mydic1["year"] = 2030
print("original:", mydic1)
print("copy()  :", mydic2)

print("\n--- Looping through a dictionary ---")
mydic = {"brand": "Ford", "model": "Aspire", "year": 2025}
for key, value in mydic.items():
    print("  ", key, "->", value)

print("\n--- Nested dictionary ---")
students = {
    "s1": {"name": "Alice", "marks": 85},
    "s2": {"name": "Bob", "marks": 92}
}
print("Student 2 name:", students["s2"]["name"])
```

**Output:**

```text

--- 6. Copying a dictionary ---
original: {'brand': 'Ford', 'model': 'Aspire', 'year': 2030}
copy()  : {'brand': 'Ford', 'model': 'Aspire', 'year': 2024}

--- Looping through a dictionary ---
   brand -> Ford
   model -> Aspire
   year -> 2025

--- Nested dictionary ---
Student 2 name: Bob
```

## Summary & Master Collection Comparison

- A dictionary stores data in `key : value` pairs (ordered, mutable, unique keys).
- Great for storing structured data (like JSON).

| Collection | Brackets | Ordered? | Mutable? | Duplicates Allowed? |
| :--- | :---: | :---: | :---: | :---: |
| **List** | `[ ]` | Yes | Yes | Yes |
| **Tuple** | `( )` | Yes | **No** | Yes |
| **Set** | `{ }` | **No** | Yes (add/remove) | **No** |
| **Dictionary** | `{k: v}` | Yes (3.7+) | Yes | Keys: **No** / Values: Yes |
