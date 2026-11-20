# Practice Code – File Handling

> [← Course index](../README.md)

Reading and writing text, CSV, JSON and XML files, parsing logs and working with paths. Every problem creates its own sample data and cleans up afterwards, so you can run it as is.

**19 problems** · 10 Easy · 7 Medium · 2 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Write text to a file and read it back](#1-write-text-to-a-file-and-read-it-back) | Easy |
| 2 | [Append to an existing file](#2-append-to-an-existing-file) | Easy |
| 3 | [Read a file line by line with line numbers](#3-read-a-file-line-by-line-with-line-numbers) | Easy |
| 4 | [Read a large file and count the words](#4-read-a-large-file-and-count-the-words) | Medium |
| 5 | [Search a file for a keyword](#5-search-a-file-for-a-keyword) | Easy |
| 6 | [Copy a file](#6-copy-a-file) | Easy |
| 7 | [Check if a file exists, get its size and delete it](#7-check-if-a-file-exists-get-its-size-and-delete-it) | Easy |
| 8 | [Move the file pointer with seek() and tell()](#8-move-the-file-pointer-with-seek-and-tell) | Medium |
| 9 | [Write and read a JSON file](#9-write-and-read-a-json-file) | Easy |
| 10 | [Write and read a CSV file](#10-write-and-read-a-csv-file) | Easy |
| 11 | [Read a CSV and compute column statistics](#11-read-a-csv-and-compute-column-statistics) | Medium |
| 12 | [Parse a log file and count ERROR and WARN entries](#12-parse-a-log-file-and-count-error-and-warn-entries) | Medium |
| 13 | [Find the top N most frequent IP addresses in an access log](#13-find-the-top-n-most-frequent-ip-addresses-in-an-access-log) | Medium |
| 14 | [Parse JSON and extract nested values safely](#14-parse-json-and-extract-nested-values-safely) | Medium |
| 15 | [Flatten a nested JSON document into dot-notation keys](#15-flatten-a-nested-json-document-into-dot-notation-keys) | Hard |
| 16 | [Parse XML and extract specific attributes](#16-parse-xml-and-extract-specific-attributes) | Medium |
| 17 | [Mask sensitive data in log lines](#17-mask-sensitive-data-in-log-lines) | Hard |
| 18 | [List files in a folder that match a pattern](#18-list-files-in-a-folder-that-match-a-pattern) | Easy |
| 19 | [Handle a missing file gracefully](#19-handle-a-missing-file-gracefully) | Easy |

---

## 1. Write text to a file and read it back

**Level:** Easy

**Problem:** Create a file, write two lines and read the whole content.

**Explanation:** `open(..., "w")` creates (or overwrites) the file. The `with` statement closes it automatically, even if an error occurs.

```python
import os

with open("notes.txt", "w", encoding="utf-8") as f:
    f.write("Line one\n")
    f.write("Line two\n")

with open("notes.txt", encoding="utf-8") as f:
    print(f.read())

os.remove("notes.txt")

# ------------Expected Output---------

# Line one
# Line two
```

---

## 2. Append to an existing file

**Level:** Easy

**Problem:** Add a new line to the end of a file without erasing it.

**Explanation:** Mode `"a"` appends instead of overwriting. `readlines()` returns the lines as a list.

```python
import os

with open("log.txt", "w", encoding="utf-8") as f:
    f.write("first entry\n")

with open("log.txt", "a", encoding="utf-8") as f:
    f.write("second entry\n")

with open("log.txt", encoding="utf-8") as f:
    print(f.readlines())

os.remove("log.txt")

# ------------Expected Output---------

# ['first entry\n', 'second entry\n']
```

---

## 3. Read a file line by line with line numbers

**Level:** Easy

**Problem:** Print each line of a file together with its line number.

**Explanation:** Iterating over the file object reads one line at a time, so even huge files use little memory. `enumerate(..., start=1)` supplies the numbers and `rstrip()` removes the newline.

```python
import os

with open("poem.txt", "w", encoding="utf-8") as f:
    f.write("Roses are red\nViolets are blue\nPython is fun\n")

with open("poem.txt", encoding="utf-8") as f:
    for number, line in enumerate(f, start=1):
        print(number, line.rstrip())

os.remove("poem.txt")

# ------------Expected Output---------

# 1 Roses are red
# 2 Violets are blue
# 3 Python is fun
```

---

## 4. Read a large file and count the words

**Level:** Medium

**Problem:** Count the words of a file without loading it completely into memory.

**Explanation:** The function iterates over the file line by line, so memory use stays constant. Each line is split into words and the lengths are added up.

```python
def count_words(path: str) -> int:
    total = 0
    with open(path, encoding="utf-8") as f:  # file iterator: constant memory
        for line in f:
            total += len(line.split())
    return total

with open("sample.txt", "w", encoding="utf-8") as f:
    f.write("Python makes file handling simple\nand readable\n")

print(count_words("sample.txt"))

import os
os.remove("sample.txt")

# ------------Expected Output---------

# 7
```

---

## 5. Search a file for a keyword

**Level:** Easy

**Problem:** Print every line of a file that contains a keyword, ignoring case.

**Explanation:** Lower-case both the line and the keyword before the `in` test so the search is case-insensitive.

```python
import os

with open("server.log", "w", encoding="utf-8") as f:
    f.write("Server started\nError: disk full\nRequest served\nerror: timeout\n")

keyword = "error"
with open("server.log", encoding="utf-8") as f:
    for line in f:
        if keyword in line.lower():
            print(line.rstrip())

os.remove("server.log")

# ------------Expected Output---------

# Error: disk full
# error: timeout
```

---

## 6. Copy a file

**Level:** Easy

**Problem:** Create a copy of a file and confirm the content is identical.

**Explanation:** `shutil.copyfile(source, destination)` copies the content. Reading both files and comparing proves the copy is exact.

```python
import os
import shutil

with open("original.txt", "w", encoding="utf-8") as f:
    f.write("important data\n")

shutil.copyfile("original.txt", "backup.txt")

with open("original.txt", encoding="utf-8") as a, open("backup.txt", encoding="utf-8") as b:
    print(a.read() == b.read())

os.remove("original.txt")
os.remove("backup.txt")

# ------------Expected Output---------

# True
```

---

## 7. Check if a file exists, get its size and delete it

**Level:** Easy

**Problem:** Test for a file, read its size in bytes and remove it safely.

**Explanation:** `os.path.exists` tests for the path, `os.path.getsize` returns the size in bytes and `os.remove` deletes the file.

```python
import os

with open("temp.txt", "w", encoding="utf-8") as f:
    f.write("12345")

print(os.path.exists("temp.txt"))
print(os.path.getsize("temp.txt"))

os.remove("temp.txt")
print(os.path.exists("temp.txt"))

# ------------Expected Output---------

# True
# 5
# False
```

---

## 8. Move the file pointer with seek() and tell()

**Level:** Medium

**Problem:** Read part of a file, find the current position and jump to another position.

**Explanation:** `tell()` returns the current byte position and `seek(n)` moves to position `n`. Mode `"w+"` allows both writing and reading.

```python
import os

with open("data.txt", "w+", encoding="utf-8") as f:
    f.write("Hello, World")
    f.seek(0)
    print(f.read(5))
    print(f.tell())
    f.seek(7)
    print(f.read())

os.remove("data.txt")

# ------------Expected Output---------

# Hello
# 5
# World
```

---

## 9. Write and read a JSON file

**Level:** Easy

**Problem:** Save a Python dictionary as JSON and load it back.

**Explanation:** `json.dump` writes Python data as JSON text and `json.load` converts it back to dictionaries and lists. `indent=2` makes the file readable.

```python
import json
import os

data = {"name": "Ann", "skills": ["Python", "Selenium"], "active": True}

with open("profile.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2)

with open("profile.json", encoding="utf-8") as f:
    loaded = json.load(f)

print(loaded)
print(loaded["skills"][1])

os.remove("profile.json")

# ------------Expected Output---------

# {'name': 'Ann', 'skills': ['Python', 'Selenium'], 'active': True}
# Selenium
```

---

## 10. Write and read a CSV file

**Level:** Easy

**Problem:** Write rows to a CSV file and read them back as dictionaries.

**Explanation:** `csv.writer` writes rows; `csv.DictReader` reads each row as a dictionary keyed by the header. Use `newline=""` so the csv module controls line endings.

```python
import csv
import os

rows = [["name", "marks"], ["Ann", 90], ["Bob", 75]]

with open("students.csv", "w", newline="", encoding="utf-8") as f:
    csv.writer(f).writerows(rows)

with open("students.csv", newline="", encoding="utf-8") as f:
    for row in csv.DictReader(f):
        print(row["name"], row["marks"])

os.remove("students.csv")

# ------------Expected Output---------

# Ann 90
# Bob 75
```

---

## 11. Read a CSV and compute column statistics

**Level:** Medium

**Problem:** Calculate the count, minimum, maximum, mean and median of a numeric column.

**Explanation:** `DictReader` gives each row as a dictionary, the column values are converted to floats and the `statistics` module computes the mean and median.

```python
import csv
import statistics

def column_stats(path: str, column: str) -> dict:
    with open(path, newline="", encoding="utf-8") as f:
        values = [float(row[column]) for row in csv.DictReader(f) if row[column]]
    return {
        "count": len(values),
        "min": min(values),
        "max": max(values),
        "mean": statistics.mean(values),
        "median": statistics.median(values),
    }

with open("prices.csv", "w", newline="", encoding="utf-8") as f:
    f.write("item,price\npen,10\nbook,250\nbag,1200\nbottle,300\n")

print(column_stats("prices.csv", "price"))

import os
os.remove("prices.csv")

# ------------Expected Output---------

# {'count': 4, 'min': 10.0, 'max': 1200.0, 'mean': 440.0, 'median': 275.0}
```

---

## 12. Parse a log file and count ERROR and WARN entries

**Level:** Medium

**Problem:** Count how many log lines are errors and how many are warnings.

**Explanation:** A compiled regex finds the level word in each line and a `Counter` tallies them while the file is read line by line.

```python
import re
from collections import Counter

LEVEL_RE = re.compile(r"\b(ERROR|WARN(?:ING)?)\b")

def count_levels(path: str) -> Counter:
    counts = Counter()
    with open(path, encoding="utf-8") as f:
        for line in f:
            m = LEVEL_RE.search(line)
            if m:
                counts[m.group(1)] += 1
    return counts

with open("app.log", "w", encoding="utf-8") as f:
    f.write(
        "2026-11-01 10:00:01 INFO Server started\n"
        "2026-11-01 10:00:05 WARN Disk usage 85%\n"
        "2026-11-01 10:01:10 ERROR Database timeout\n"
        "2026-11-01 10:02:30 ERROR Login failed\n"
        "2026-11-01 10:03:00 WARNING Slow response\n"
    )

print(count_levels("app.log"))

import os
os.remove("app.log")

# ------------Expected Output---------

# Counter({'ERROR': 2, 'WARN': 1, 'WARNING': 1})
```

---

## 13. Find the top N most frequent IP addresses in an access log

**Level:** Medium

**Problem:** List the IPs that appear most often at the start of log lines.

**Explanation:** A regex captures the IP at the beginning of each line, a `Counter` counts them and `most_common(n)` returns the top ones.

```python
import re
from collections import Counter

IP_RE = re.compile(r"^(\d{1,3}(?:\.\d{1,3}){3})")

def top_ips(path: str, n: int = 5) -> list[tuple[str, int]]:
    c = Counter()
    with open(path, encoding="utf-8") as f:
        for line in f:
            m = IP_RE.match(line)
            if m:
                c[m.group(1)] += 1
    return c.most_common(n)

with open("access.log", "w", encoding="utf-8") as f:
    f.write(
        '10.0.0.1 - GET /home\n'
        '10.0.0.2 - GET /login\n'
        '10.0.0.1 - GET /cart\n'
        '10.0.0.3 - GET /home\n'
        '10.0.0.1 - POST /pay\n'
        '10.0.0.2 - GET /logout\n'
    )

print(top_ips("access.log", 2))

import os
os.remove("access.log")

# ------------Expected Output---------

# [('10.0.0.1', 3), ('10.0.0.2', 2)]
```

---

## 14. Parse JSON and extract nested values safely

**Level:** Medium

**Problem:** Read a deeply nested value by a dotted path and return a default when something is missing.

**Explanation:** The helper walks the path one key at a time using `reduce`. If a key or list index is missing it returns the default instead of raising an error.

```python
import json
from functools import reduce

def safe_get(data, path: str, default=None, sep="."):
    def step(cur, key):
        if isinstance(cur, dict):
            return cur.get(key, default)
        if isinstance(cur, list) and key.isdigit() and int(key) < len(cur):
            return cur[int(key)]
        return default
    result = reduce(step, path.split(sep), data)
    return result

doc = json.loads('{"user": {"orders": [{"id": 7}]}}')
print(safe_get(doc, "user.orders.0.id"))      # 7
print(safe_get(doc, "user.address.city", "-")) # -

# ------------Expected Output---------

# 7
# -
```

---

## 15. Flatten a nested JSON document into dot-notation keys

**Level:** Hard

**Problem:** Convert nested dictionaries and lists into one flat dictionary such as `a.c.0`.

**Explanation:** The function calls itself for every nested dictionary or list, building the key path as it goes. Plain values are stored under their full path.

```python
def flatten_json(obj, parent="", sep=".") -> dict:
    items = {}
    if isinstance(obj, dict):
        for k, v in obj.items():
            items.update(flatten_json(v, f"{parent}{sep}{k}" if parent else k, sep))
    elif isinstance(obj, list):
        for i, v in enumerate(obj):
            items.update(flatten_json(v, f"{parent}{sep}{i}" if parent else str(i), sep))
    else:
        items[parent] = obj
    return items

print(flatten_json({"a": {"b": 1, "c": [2, 3]}}))
# {'a.b': 1, 'a.c.0': 2, 'a.c.1': 3}

# ------------Expected Output---------

# {'a.b': 1, 'a.c.0': 2, 'a.c.1': 3}
```

---

## 16. Parse XML and extract specific attributes

**Level:** Medium

**Problem:** Read attributes and child text from an XML document and filter with an XPath-style query.

**Explanation:** `ElementTree` parses the XML string into a tree. `findall`, `get` and `findtext` read elements, attributes and text; `[@lang='fr']` filters by attribute value.

```python
import xml.etree.ElementTree as ET

xml = """
<catalog>
  <book id="b1" lang="en"><title>Python</title></book>
  <book id="b2" lang="fr"><title>Tests</title></book>
</catalog>
"""
root = ET.fromstring(xml)
for book in root.findall("book"):
    print(book.get("id"), book.get("lang"), book.findtext("title"))

# XPath-style filter
print([b.get("id") for b in root.findall(".//book[@lang='fr']")])  # ['b2']

# ------------Expected Output---------

# b1 en Python
# b2 fr Tests
# ['b2']
```

---

## 17. Mask sensitive data in log lines

**Level:** Hard

**Problem:** Hide card numbers, passwords and bearer tokens before logs are stored.

**Explanation:** Three regular expressions replace card numbers (keeping the last four digits), password values and bearer tokens. Replacing the secret part while keeping the key makes the log still readable.

```python
import re

def mask_sensitive(line: str) -> str:
    # card numbers: keep last 4
    line = re.sub(r"\b(?:\d[ -]?){12}(\d{4})\b", r"****-****-****-\1", line)
    # password=..., "password": "..."
    line = re.sub(r'(?i)(password"?\s*[:=]\s*"?)[^\s",&]+', r"\1*****", line)
    # bearer tokens
    line = re.sub(r"(?i)(bearer\s+)[\w.-]+", r"\1*****", line)
    return line

print(mask_sensitive('card=4111 1111 1111 1234 password=Secret123 Bearer abc.def.ghi'))

# ------------Expected Output---------

# card=****-****-****-1234 password=***** Bearer *****
```

---

## 18. List files in a folder that match a pattern

**Level:** Easy

**Problem:** List only the `.txt` files of a folder.

**Explanation:** `Path.glob("*.txt")` returns the matching paths. A temporary directory keeps the example self-cleaning.

```python
import tempfile
from pathlib import Path

with tempfile.TemporaryDirectory() as folder:
    for name in ["a.txt", "b.csv", "c.txt"]:
        (Path(folder) / name).write_text("x")

    print(sorted(p.name for p in Path(folder).glob("*.txt")))
    print(sorted(p.name for p in Path(folder).iterdir()))

# ------------Expected Output---------

# ['a.txt', 'c.txt']
# ['a.txt', 'b.csv', 'c.txt']
```

---

## 19. Handle a missing file gracefully

**Level:** Easy

**Problem:** Try to open a file that does not exist and continue with a default.

**Explanation:** Opening a missing file raises `FileNotFoundError`. Catching it lets the program report the problem and carry on.

```python
try:
    with open("missing_file.txt", encoding="utf-8") as f:
        content = f.read()
except FileNotFoundError:
    print("File not found - using default content")
    content = "default"

print(content)

# ------------Expected Output---------

# File not found - using default content
# default
```
