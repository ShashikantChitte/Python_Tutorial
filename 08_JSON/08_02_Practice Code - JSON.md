# Practice Code – JSON

> [← Course index](../README.md)

Parsing, writing, nesting, custom objects, validation and error fixing with the `json` module. Every problem creates its own sample data and cleans up afterwards, so you can run it as is. Problem 5 needs `pip install pydantic` (v2).

**6 problems** · 2 Easy · 3 Medium · 1 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Parse, modify and pretty-print a JSON string](#1-parse-modify-and-pretty-print-a-json-string) | Easy |
| 2 | [Write and read a JSON file, then filter the records](#2-write-and-read-a-json-file-then-filter-the-records) | Easy |
| 3 | [Extract nested JSON values safely](#3-extract-nested-json-values-safely) | Medium |
| 4 | [Serialize and restore a custom class (Decimal and datetime)](#4-serialize-and-restore-a-custom-class-decimal-and-datetime) | Hard |
| 5 | [Validate a JSON string with a Pydantic model](#5-validate-a-json-string-with-a-pydantic-model) | Medium |
| 6 | [Fix a broken JSON string](#6-fix-a-broken-json-string) | Medium |

---

## 1. Parse, modify and pretty-print a JSON string

**Level:** Easy

**Problem:** Parse the JSON string below, add the skill `"Docker"` to the list, update `"experience"` to `4`, and serialize it back to a sorted, 4-space indented JSON string.

**Explanation:** `json.loads()` turns the string into a `dict`, which can then be changed like any other dictionary. `json.dumps()` converts it back; `indent=4` makes it readable and `sort_keys=True` orders the keys alphabetically, so the output is the same every time.

```python
import json

raw_json = '{"name": "Sneha", "experience": 3, "skills": ["Python", "FastAPI"]}'

data = json.loads(raw_json)
data["skills"].append("Docker")
data["experience"] = 4

updated_json = json.dumps(data, indent=4, sort_keys=True)
print(updated_json)

# ------------Expected Output---------

# {
#     "experience": 4,
#     "name": "Sneha",
#     "skills": [
#         "Python",
#         "FastAPI",
#         "Docker"
#     ]
# }
```

---

## 2. Write and read a JSON file, then filter the records

**Level:** Easy

**Problem:** Save a list of Indian city weather records to `weather.json` using UTF-8 encoding, read it back, and print only the cities where the temperature exceeds `30`.

**Explanation:** `json.dump()` writes straight to an open file and `json.load()` reads it back. `encoding="utf-8"` together with `ensure_ascii=False` keeps non-English characters readable in the file. A list comprehension then filters the loaded records.

```python
import json
import os

records = [
    {"city": "Mumbai", "temp_c": 32},
    {"city": "Bengaluru", "temp_c": 26},
    {"city": "Jaipur", "temp_c": 35},
]

with open("weather.json", "w", encoding="utf-8") as f:
    json.dump(records, f, indent=2, ensure_ascii=False)

with open("weather.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)

hot_cities = [r["city"] for r in loaded if r["temp_c"] > 30]
print("Hot cities:", hot_cities)

os.remove("weather.json")

# ------------Expected Output---------

# Hot cities: ['Mumbai', 'Jaipur']
```

---

## 3. Extract nested JSON values safely

**Level:** Medium

**Problem:** Extract the user's pincode safely (defaulting to `"000000"` if `profile` or `address` is missing or `None`) and calculate the total price of all delivered items.

**Explanation:** Here `"profile"` exists but holds `None`, so `payload["user"]["profile"]["address"]` would raise a `TypeError`. Each level uses `.get(...) or {}`, which turns both a missing key and a `None` value into an empty dictionary, and the last `.get()` supplies the default. The total sums only the entries whose `delivered` flag is true.

```python
payload = {
    "user": {"id": 7, "profile": None},
    "cart": [
        {"item": "Monitor", "price": 15000, "delivered": True},
        {"item": "Cable", "price": 300, "delivered": False},
        {"item": "Stand", "price": 1200, "delivered": True},
    ],
}

user = payload.get("user") or {}
profile = user.get("profile") or {}
pincode = (profile.get("address") or {}).get("pincode", "000000")

delivered_total = sum(
    entry.get("price", 0)
    for entry in payload.get("cart", [])
    if entry.get("delivered")
)

print(f"PIN: {pincode} | Delivered Total: {delivered_total}")

# ------------Expected Output---------

# PIN: 000000 | Delivered Total: 16200
```

---

## 4. Serialize and restore a custom class (Decimal and datetime)

**Level:** Hard

**Problem:** Write a custom encoder and an `object_hook` decoder that convert an `Invoice` instance containing a `Decimal` and a `datetime` to JSON and rebuild it as an `Invoice` object.

**Explanation:** `json` cannot serialize `Invoice`, `Decimal` or `datetime`, so `default=` receives the object and returns a plain dictionary. The `"__invoice__"` marker lets `object_hook` recognise which dictionaries to turn back into an `Invoice`. The amount is stored as a **string** so the `Decimal` keeps its exact value (`2499.50`), and the date uses ISO format, which `datetime.fromisoformat()` reads back.

```python
import json
from datetime import datetime
from decimal import Decimal


class Invoice:
    def __init__(self, code: str, amount: Decimal, issued_at: datetime):
        self.code = code
        self.amount = amount
        self.issued_at = issued_at


def invoice_encoder(obj):
    if isinstance(obj, Invoice):
        return {"__invoice__": True, "code": obj.code,
                "amount": str(obj.amount), "issued_at": obj.issued_at.isoformat()}
    raise TypeError(f"Unsupported type: {type(obj)}")


def invoice_decoder(dct):
    if dct.get("__invoice__"):
        return Invoice(dct["code"], Decimal(dct["amount"]),
                       datetime.fromisoformat(dct["issued_at"]))
    return dct


inv = Invoice("INV-101", Decimal("2499.50"), datetime(2026, 11, 22, 14, 30))
raw = json.dumps(inv, default=invoice_encoder)
print(raw)

restored = json.loads(raw, object_hook=invoice_decoder)
print(isinstance(restored, Invoice), restored.amount, restored.issued_at)

# ------------Expected Output---------

# {"__invoice__": true, "code": "INV-101", "amount": "2499.50", "issued_at": "2026-11-22T14:30:00"}
# True 2499.50 2026-11-22 14:30:00
```

---

## 5. Validate a JSON string with a Pydantic model

**Level:** Medium

**Problem:** Create a Pydantic `Product` model where `sku` must be at least 4 characters and `price` must be positive. Validate a raw JSON string and catch `ValidationError`. *(Requires `pip install pydantic` v2.)*

**Explanation:** `Field(min_length=4)` and `Field(gt=0)` declare the rules once, and `model_validate_json()` parses and checks the text in one step. The valid product becomes a typed object. The second payload breaks both rules, and the `ValidationError` lists each field together with the reason.

```python
from pydantic import BaseModel, Field, ValidationError


class Product(BaseModel):
    sku: str = Field(min_length=4)
    name: str
    price: float = Field(gt=0)
    in_stock: bool = True


for raw_product in (
    '{"sku": "KB01", "name": "Wireless Keyboard", "price": 1899.0}',
    '{"sku": "KB", "name": "Broken Keyboard", "price": -5}',
):
    try:
        item = Product.model_validate_json(raw_product)
        print("Valid:", item.sku, item.price, item.in_stock)
    except ValidationError as err:
        for problem in err.errors():
            print(f"Invalid {problem['loc'][0]}: {problem['msg']}")

# ------------Expected Output---------

# Valid: KB01 1899.0 True
# Invalid sku: String should have at least 4 characters
# Invalid price: Input should be greater than 0
```

---

## 6. Fix a broken JSON string

**Level:** Medium

**Problem:** The string below fails `json.loads()` because of single quotes, Python booleans and a trailing comma. Safely convert it into a valid Python dictionary and a clean JSON string.

**Explanation:** The text is not JSON at all, but it **is** a valid Python literal. `ast.literal_eval()` parses Python literals without executing code, which makes it safe (never use `eval()` on outside data). `json.dumps()` then writes proper JSON: double quotes, lowercase `true`, no trailing comma. This only works for Python-style text; for real JSON that merely contains `true` / `null`, use `json.loads()`.

```python
import ast
import json

broken_str = "{'user': 'Vikram', 'active': True, 'roles': ['admin', 'editor',]}"

try:
    json.loads(broken_str)
except json.JSONDecodeError as err:
    print("json.loads failed:", err.msg)

parsed_dict = ast.literal_eval(broken_str)
clean_json = json.dumps(parsed_dict)
print(clean_json)

# ------------Expected Output---------

# json.loads failed: Expecting property name enclosed in double quotes
# {"user": "Vikram", "active": true, "roles": ["admin", "editor"]}
```
