# Files, JSON and Regex

> [← Practice Codes index](README.md)

Reading and parsing files (text, CSV, JSON, XML, YAML, Excel), log analysis, data diffing, schema validation and regular expressions.

**15 problems** · 4 Easy · 6 Medium · 5 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Read a large file line by line and count the words](#1-read-a-large-file-line-by-line-and-count-the-words) | Easy |
| 2 | [Read a CSV and compute column statistics](#2-read-a-csv-and-compute-column-statistics) | Easy |
| 3 | [Read and write YAML or config files](#3-read-and-write-yaml-or-config-files) | Easy |
| 4 | [Generate a random test dataset (names, emails, dates)](#4-generate-a-random-test-dataset-names-emails-dates) | Easy |
| 5 | [Parse a log file and count ERROR/WARN entries](#5-parse-a-log-file-and-count-errorwarn-entries) | Medium |
| 6 | [Find the top N most frequent IPs in an access log](#6-find-the-top-n-most-frequent-ips-in-an-access-log) | Medium |
| 7 | [Parse JSON and extract nested values safely](#7-parse-json-and-extract-nested-values-safely) | Medium |
| 8 | [Parse XML and extract specific attributes](#8-parse-xml-and-extract-specific-attributes) | Medium |
| 9 | [Extract emails, phone numbers, and dates with regex](#9-extract-emails-phone-numbers-and-dates-with-regex) | Medium |
| 10 | [Mask sensitive data (credit cards, passwords) in logs](#10-mask-sensitive-data-credit-cards-passwords-in-logs) | Medium |
| 11 | [Compare two CSV files and report differences](#11-compare-two-csv-files-and-report-differences) | Hard |
| 12 | [Flatten a nested JSON into dot-notation keys](#12-flatten-a-nested-json-into-dot-notation-keys) | Hard |
| 13 | [Compare two JSON objects and report the diff](#13-compare-two-json-objects-and-report-the-diff) | Hard |
| 14 | [Validate a JSON payload against a schema](#14-validate-a-json-payload-against-a-schema) | Hard |
| 15 | [Compare two Excel files with openpyxl or pandas](#15-compare-two-excel-files-with-openpyxl-or-pandas) | Hard |

---

## Easy

### 1. Read a large file line by line and count the words

> **Note:** Defines a helper only – call it with the path to a text file of your own.

```python
def count_words(path: str) -> int:
    total = 0
    with open(path, encoding="utf-8") as f:  # file iterator: constant memory
        for line in f:
            total += len(line.split())
    return total
```

### 2. Read a CSV and compute column statistics

> **Note:** Defines a helper only – call it with the path to a CSV file of your own.

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
```

### 3. Read and write YAML or config files

> **Note:** Needs `pip install pyyaml`. Writes `config.yaml` and `config.ini` into the working directory.

```python
# pip install pyyaml
import configparser
import yaml

# YAML
config = {"env": "qa", "retries": 3, "urls": {"api": "https://qa.example.com"}}
with open("config.yaml", "w") as f:
    yaml.safe_dump(config, f)
with open("config.yaml") as f:
    loaded = yaml.safe_load(f)

# INI
cp = configparser.ConfigParser()
cp["db"] = {"host": "localhost", "port": "5432"}
with open("config.ini", "w") as f:
    cp.write(f)
cp.read("config.ini")
print(loaded["urls"]["api"], cp["db"].getint("port"))

# ------------Expected Output---------

# https://qa.example.com 5432
```

### 4. Generate a random test dataset (names, emails, dates)

> **Note:** Needs `pip install faker`. Writes `users.csv` into the working directory.

```python
# pip install faker
import csv
from faker import Faker

fake = Faker()
Faker.seed(42)  # reproducible data

rows = [
    {
        "name": fake.name(),
        "email": fake.email(),
        "dob": fake.date_of_birth(minimum_age=18, maximum_age=65).isoformat(),
        "city": fake.city(),
    }
    for _ in range(100)
]
with open("users.csv", "w", newline="") as f:
    w = csv.DictWriter(f, fieldnames=rows[0].keys())
    w.writeheader()
    w.writerows(rows)
```

---

## Medium

### 5. Parse a log file and count ERROR/WARN entries

> **Note:** Defines a helper only – call it with the path to a log file of your own.

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
```

### 6. Find the top N most frequent IPs in an access log

> **Note:** Defines a helper only – call it with the path to an access log of your own.

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
```

### 7. Parse JSON and extract nested values safely

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

### 8. Parse XML and extract specific attributes

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

### 9. Extract emails, phone numbers, and dates with regex

```python
import re

text = "Contact: john.doe@test.com, +91-98765-43210, on 2026-10-11 or 11/10/2026."

emails = re.findall(r"[\w.+-]+@[\w-]+(?:\.[\w-]+)+", text)
phones = re.findall(r"\+?\d{1,3}[-\s]?\d{5}[-\s]?\d{5}", text)
dates = re.findall(r"\b\d{4}-\d{2}-\d{2}\b|\b\d{2}/\d{2}/\d{4}\b", text)
print(emails, phones, dates)

# ------------Expected Output---------

# ['john.doe@test.com'] ['+91-98765-43210'] ['2026-10-11', '11/10/2026']
```

### 10. Mask sensitive data (credit cards, passwords) in logs

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

## Hard

### 11. Compare two CSV files and report differences

> **Note:** The in-memory version comes from the Complete_Hard notebook (#17).

```python
import csv

def load(path, key):
    with open(path, newline="", encoding="utf-8") as f:
        return {row[key]: row for row in csv.DictReader(f)}

def diff_csv(file_a: str, file_b: str, key: str) -> dict:
    a, b = load(file_a, key), load(file_b, key)
    return {
        "only_in_a": sorted(a.keys() - b.keys()),
        "only_in_b": sorted(b.keys() - a.keys()),
        "changed": {
            k: {col: (a[k][col], b[k].get(col))
                for col in a[k] if a[k][col] != b[k].get(col)}
            for k in a.keys() & b.keys() if a[k] != b[k]
        },
    }
```

**Runnable in-memory version with sample data**

```python
import csv, io

def diff_csv_streams(fa, fb, key: str) -> dict:
    a = {row[key]: row for row in csv.DictReader(fa)}
    b = {row[key]: row for row in csv.DictReader(fb)}
    return {
        "only_in_a": sorted(a.keys() - b.keys()),
        "only_in_b": sorted(b.keys() - a.keys()),
        "changed": {
            k: {col: (a[k][col], b[k].get(col)) for col in a[k] if a[k][col] != b[k].get(col)}
            for k in a.keys() & b.keys() if a[k] != b[k]
        },
    }

print(diff_csv_streams(io.StringIO("id,val\n1,a\n2,b\n"), io.StringIO("id,val\n2,c\n3,d\n"), "id"))

# ------------Expected Output---------

# {'only_in_a': ['1'], 'only_in_b': ['3'], 'changed': {'2': {'val': ('b', 'c')}}}
```

### 12. Flatten a nested JSON into dot-notation keys

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

### 13. Compare two JSON objects and report the diff

```python
def json_diff(a, b, path=""):
    diffs = []
    if isinstance(a, dict) and isinstance(b, dict):
        for k in a.keys() | b.keys():
            p = f"{path}.{k}" if path else k
            if k not in a:
                diffs.append(("added", p, b[k]))
            elif k not in b:
                diffs.append(("removed", p, a[k]))
            else:
                diffs += json_diff(a[k], b[k], p)
    elif isinstance(a, list) and isinstance(b, list):
        for i in range(max(len(a), len(b))):
            p = f"{path}[{i}]"
            if i >= len(a):
                diffs.append(("added", p, b[i]))
            elif i >= len(b):
                diffs.append(("removed", p, a[i]))
            else:
                diffs += json_diff(a[i], b[i], p)
    elif a != b:
        diffs.append(("changed", path, (a, b)))
    return diffs

print(json_diff({"a": 1, "b": [1, 2]}, {"a": 2, "b": [1], "c": 3}))

# ------------Expected Output---------

# [('removed', 'b[1]', 2), ('added', 'c', 3), ('changed', 'a', (1, 2))]
```

### 14. Validate a JSON payload against a schema

> **Note:** Needs `pip install jsonschema`.

```python
from jsonschema import validate, ValidationError

schema = {
    "type": "object",
    "required": ["id", "email"],
    "properties": {
        "id": {"type": "integer", "minimum": 1},
        "email": {"type": "string", "pattern": r"^[^@]+@[^@]+\.[^@]+$"},
        "tags": {"type": "array", "items": {"type": "string"}},
    },
    "additionalProperties": False,
}

def is_valid(payload: dict) -> tuple[bool, str]:
    try:
        validate(payload, schema)
        return True, "ok"
    except ValidationError as e:
        return False, e.message

print(is_valid({"id": 1, "email": "a@b.com"}))
print(is_valid({"id": 0, "email": "bad"}))

# ------------Expected Output---------

# (True, 'ok')
# (False, '0 is less than the minimum of 1')
```

### 15. Compare two Excel files with openpyxl or pandas

> **Note:** Needs `pip install pandas openpyxl`. Defines helpers only – call them with two Excel files of your own.

```python
# pip install pandas openpyxl
import pandas as pd

def compare_excel(file_a: str, file_b: str, sheet=0, key=None) -> pd.DataFrame:
    a = pd.read_excel(file_a, sheet_name=sheet)
    b = pd.read_excel(file_b, sheet_name=sheet)
    if key:
        a, b = a.set_index(key), b.set_index(key)
    a, b = a.sort_index(), b.sort_index()
    return a.compare(b, result_names=("file_a", "file_b"))  # same shape/labels required

# For different shapes, outer-merge on a key with indicator=True:
def row_diff(file_a, file_b):
    a, b = pd.read_excel(file_a), pd.read_excel(file_b)
    merged = a.merge(b, how="outer", indicator=True)
    return merged[merged["_merge"] != "both"]
```
