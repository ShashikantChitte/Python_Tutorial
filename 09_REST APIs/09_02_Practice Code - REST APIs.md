# Practice Code – REST APIs

> [← Course index](../README.md)

Calling REST APIs with `requests`: query parameters, `POST` / `PATCH` / `DELETE`, sessions with authentication, automatic retries and pagination. Problems 1–4 and 6 call free public practice services and therefore need an internet connection; problem 5 runs completely offline. Requires `pip install requests`.

**6 problems** · 2 Easy · 3 Medium · 1 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [GET request with query parameters](#1-get-request-with-query-parameters) | Easy |
| 2 | [Create a resource with POST](#2-create-a-resource-with-post) | Easy |
| 3 | [Update with PATCH and delete with DELETE](#3-update-with-patch-and-delete-with-delete) | Medium |
| 4 | [Reuse an authenticated Session](#4-reuse-an-authenticated-session) | Medium |
| 5 | [Build a session with automatic retries](#5-build-a-session-with-automatic-retries) | Hard |
| 6 | [Fetch all pages of a paginated API](#6-fetch-all-pages-of-a-paginated-api) | Medium |

---

## 1. GET request with query parameters

**Level:** Easy

**Problem:** Fetch the comments for `postId=1` from `https://jsonplaceholder.typicode.com/comments` using `params=`, verify the HTTP status and print the first 2 commenter e-mails.

**Explanation:** `params={"postId": 1}` appends `?postId=1` to the URL and takes care of encoding. `raise_for_status()` turns a 4xx / 5xx answer into an exception, `timeout=10` stops the program waiting forever, and `.json()` converts the body into a list of dictionaries. One `except RequestException` catches every network or HTTP failure. *(Needs an internet connection.)*

```python
import requests

url = "https://jsonplaceholder.typicode.com/comments"

try:
    res = requests.get(url, params={"postId": 1}, timeout=10)
    res.raise_for_status()
    comments = res.json()
    print("Status:", res.status_code)
    for c in comments[:2]:
        print(c["email"])
except requests.exceptions.RequestException as err:
    print("Request failed:", err)

# ------------Expected Output---------

# Status: 200
# Eliseo@gardner.biz
# Jayne_Kuhic@sydney.com
```

---

## 2. Create a resource with POST

**Level:** Easy

**Problem:** Send a `POST` request to `https://jsonplaceholder.typicode.com/posts` with a JSON payload containing `title`, `body` and `userId`, then print the newly assigned `id`.

**Explanation:** `json=payload` serializes the dictionary and sets `Content-Type: application/json`. A successful creation normally answers `201 Created` with the new object, including its generated `id`. JSONPlaceholder only simulates the creation, so the same `id` (101) comes back every time. *(Needs an internet connection.)*

```python
import requests

url = "https://jsonplaceholder.typicode.com/posts"
payload = {"title": "Learning Python APIs", "body": "Practice makes perfect.", "userId": 10}

res = requests.post(url, json=payload, timeout=10)
res.raise_for_status()
created = res.json()
print(f"Status: {res.status_code} | New Post ID: {created.get('id')}")

# ------------Expected Output---------

# Status: 201 | New Post ID: 101
```

---

## 3. Update with PATCH and delete with DELETE

**Level:** Medium

**Problem:** Update only the `title` of post 1 using `PATCH`, then send a `DELETE` request to the same URL and safely handle a potentially empty response body.

**Explanation:** `PATCH` sends only the changed field, whereas `PUT` would have to send the whole post. Many APIs answer `DELETE` with `204 No Content` and **no body**, and calling `.json()` on an empty body would raise an error, so the code checks the status code and `res.content` first. JSONPlaceholder replies `200` with `{}`, so the `else` branch runs here. *(Needs an internet connection.)*

```python
import requests

url = "https://jsonplaceholder.typicode.com/posts/1"

patch_res = requests.patch(url, json={"title": "Patched Title"}, timeout=10)
patch_res.raise_for_status()
print("Patched:", patch_res.json()["title"])

del_res = requests.delete(url, timeout=10)
del_res.raise_for_status()
if del_res.status_code == 204 or not del_res.content:
    print("Deleted with empty body.")
else:
    print("Delete status:", del_res.status_code, del_res.json())

# ------------Expected Output---------

# Patched: Patched Title
# Delete status: 200 {}
```

---

## 4. Reuse an authenticated Session

**Level:** Medium

**Problem:** Configure a `requests.Session()` with a custom `Authorization: Bearer demo-token-123` header and verify via `https://httpbin.org/headers` that the header is sent automatically.

**Explanation:** Headers placed in `session.headers` travel with **every** request made through that session, so the individual `get()` call needs no `headers=` argument. httpbin's `/headers` endpoint echoes back what it received, which proves the token was sent. The `with` block closes the connection pool afterwards. *(Needs an internet connection.)*

```python
import requests

with requests.Session() as session:
    session.headers.update({
        "Authorization": "Bearer demo-token-123",
        "Accept": "application/json",
    })

    res = session.get("https://httpbin.org/headers", timeout=10)
    res.raise_for_status()

    sent_headers = res.json().get("headers", {})
    print("Server received Auth:", sent_headers.get("Authorization"))

# ------------Expected Output---------

# Server received Auth: Bearer demo-token-123
```

---

## 5. Build a session with automatic retries

**Level:** Hard

**Problem:** Write a helper function that returns a `Session` configured to retry 3 times with exponential backoff on HTTP `429`, `500`, `502`, `503` and `504`, and prove that it works.

**Explanation:** `Retry` describes *when* to try again (`status_forcelist`), *how often* (`total`) and *how long to wait* (`backoff_factor`); an `HTTPAdapter` mounted on both `https://` and `http://` applies it to every request. `POST` is deliberately left out of `allowed_methods`, because retrying a `POST` can create duplicates. To make the result **provable** offline, the snippet starts a tiny local server that fails twice with `503` and then succeeds: the retry session ends with `200` after 3 requests. (Against a real service you would simply call `s.get("https://httpbin.org/status/200", timeout=10)`.)

```python
import threading
from http.server import BaseHTTPRequestHandler, HTTPServer

import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry


def get_retry_session() -> requests.Session:
    session = requests.Session()
    retry = Retry(
        total=3,
        backoff_factor=0.1,                       # 0 s, 0.2 s, 0.4 s between tries
        status_forcelist=[429, 500, 502, 503, 504],
        allowed_methods=["GET", "PUT", "DELETE", "HEAD"],
    )
    adapter = HTTPAdapter(max_retries=retry)
    session.mount("https://", adapter)
    session.mount("http://", adapter)
    return session


# --- a tiny test server that fails twice, then succeeds ---
calls = []

class Flaky(BaseHTTPRequestHandler):
    def do_GET(self):
        calls.append(1)
        self.send_response(503 if len(calls) <= 2 else 200)
        self.send_header("Content-Length", "0")
        self.end_headers()

    def log_message(self, *args):
        pass

server = HTTPServer(("127.0.0.1", 0), Flaky)
threading.Thread(target=server.serve_forever, daemon=True).start()

with get_retry_session() as s:
    r = s.get(f"http://127.0.0.1:{server.server_address[1]}/", timeout=10)
    print("Success:", r.status_code)
    print("Server handled", len(calls), "requests")

server.shutdown()
server.server_close()

# ------------Expected Output---------

# Success: 200
# Server handled 3 requests
```

---

## 6. Fetch all pages of a paginated API

**Level:** Medium

**Problem:** Fetch pages of todos from `https://jsonplaceholder.typicode.com/todos` using `_page` and `_limit=20`, stopping automatically after page 3 or when an empty list is returned.

**Explanation:** The loop asks for one page at a time through a `Session` (which re-uses the connection). An **empty** batch means there is nothing left, so the loop stops early; otherwise `range(1, max_pages + 1)` caps it at 3 pages. `time.sleep(0.1)` between requests keeps the program polite towards the API. Three pages of 20 items give 60 todos. *(Needs an internet connection.)*

```python
import time
import requests


def fetch_todos(max_pages: int = 3):
    collected = []
    with requests.Session() as session:
        for page in range(1, max_pages + 1):
            res = session.get(
                "https://jsonplaceholder.typicode.com/todos",
                params={"_page": page, "_limit": 20},
                timeout=10,
            )
            res.raise_for_status()
            batch = res.json()
            if not batch:                 # empty page -> stop early
                break
            collected.extend(batch)
            print(f"page {page}: {len(batch)} todos")
            time.sleep(0.1)
    return collected


items = fetch_todos(max_pages=3)
print(f"Total items fetched: {len(items)}")

# ------------Expected Output---------

# page 1: 20 todos
# page 2: 20 todos
# page 3: 20 todos
# Total items fetched: 60
```
