# Python REST APIs

> [← Course index](../README.md)

A **REST API** lets programs talk to a web service over HTTP: every *resource* (a post, a user, an order) has a URL, the **HTTP method** says what to do with it (`GET` to read, `POST` to create, …), and data travels as **JSON**, which you met in the [previous lesson](../08_JSON/08_01_JSON.md). In Python the standard tool for this is the third-party `requests` library:

```bash
pip install requests
```

> 🌐 The first examples call two free practice services: [JSONPlaceholder](https://jsonplaceholder.typicode.com) (fake posts, comments and todos) and [httpbin](https://httpbin.org) (echoes back what you send). They need an internet connection. **JSONPlaceholder pretends to save your changes but does not store anything**, so a `POST` returns a new `id` and a later `GET` will not find it. The retry, session and pagination examples run against a tiny **local server** defined in the next section, so those work offline and behave the same every time.

> 📝 The outputs shown were captured with Python 3.12 and `requests` 2.x. Data from public services can change, so your values may differ slightly.

## A Tiny Local Practice Server

Some behaviour (failing servers, cookies, pagination) cannot be demonstrated reliably on a public service, so the code below starts a small HTTP server in a background thread. It is **not needed to understand the lessons**; it simply provides these endpoints on `BASE` (`http://127.0.0.1:<free port>`):

| Endpoint | Behaviour |
|----------|-----------|
| `/flaky?key=k&fail=2` | Answers **503** for the first `fail` requests with that key, then **200** |
| `/always503` | Always answers **503** |
| `/slow` | Waits 1 second before answering |
| `/basic` | Requires HTTP Basic credentials `dev` / `secret` |
| `/login` (POST) and `/dashboard` | Cookie login: `/login` sets a session cookie that `/dashboard` requires |
| `/items?page=N&limit=L` | 25 items, paginated, with a `next` URL in the JSON body **and** a `Link` header |

<details>
<summary>Show the practice server code (optional reading)</summary>

```python
import base64
import json
import threading
import time
from collections import Counter
from http.server import BaseHTTPRequestHandler, ThreadingHTTPServer
from urllib.parse import urlparse, parse_qs

hits = Counter()                                     # requests seen per key


class PracticeHandler(BaseHTTPRequestHandler):
    def log_message(self, *args):                    # keep the console quiet
        pass

    def send_json(self, status, body, headers=None):
        raw = json.dumps(body).encode()
        self.send_response(status)
        self.send_header("Content-Type", "application/json")
        self.send_header("Content-Length", str(len(raw)))
        for key, value in (headers or {}).items():
            self.send_header(key, value)
        self.end_headers()
        self.wfile.write(raw)

    def handle_any(self):
        try:
            self.route()
        except (BrokenPipeError, ConnectionResetError):   # client gave up (e.g. a timeout)
            pass

    def route(self):
        url = urlparse(self.path)
        q = {k: v[0] for k, v in parse_qs(url.query).items()}
        base = f"http://{self.headers['Host']}"

        if url.path == "/flaky":
            hits[q["key"]] += 1
            if hits[q["key"]] <= int(q.get("fail", 2)):
                return self.send_json(503, {"error": "temporarily unavailable"})
            return self.send_json(200, {"status": "ok", "attempt": hits[q["key"]]})

        if url.path == "/always503":
            hits["always503"] += 1
            return self.send_json(503, {"error": "down"})

        if url.path == "/slow":
            time.sleep(1)
            return self.send_json(200, {"status": "finally"})

        if url.path == "/basic":
            expected = "Basic " + base64.b64encode(b"dev:secret").decode()
            if self.headers.get("Authorization") == expected:
                return self.send_json(200, {"authenticated": True})
            return self.send_json(401, {"error": "bad credentials"})

        if url.path == "/login" and self.command == "POST":
            return self.send_json(200, {"login": "ok"}, {"Set-Cookie": "sid=abc123; Path=/"})

        if url.path == "/dashboard":
            if "sid=abc123" in (self.headers.get("Cookie") or ""):
                return self.send_json(200, {"welcome": "dev@example.com"})
            return self.send_json(401, {"error": "not logged in"})

        if url.path == "/items":
            page, limit = int(q.get("page", 1)), int(q.get("limit", 10))
            everything = [{"id": n} for n in range(1, 26)]
            batch = everything[(page - 1) * limit: page * limit]
            has_next = page * limit < len(everything)
            next_url = f"{base}/items?page={page + 1}&limit={limit}" if has_next else None
            headers = {"Link": f'<{next_url}>; rel="next"'} if next_url else {}
            return self.send_json(200, {"results": batch, "next": next_url}, headers)

        self.send_json(404, {"error": "not found"})

    do_GET = do_POST = handle_any


server = ThreadingHTTPServer(("127.0.0.1", 0), PracticeHandler)    # port 0 = any free port
threading.Thread(target=server.serve_forever, daemon=True).start()
BASE = f"http://127.0.0.1:{server.server_address[1]}"
print("Practice server is running.")
```

**Output:**

```text
Practice server is running.
```

</details>

## HTTP Methods & CRUD Operations

`requests` has one function per HTTP method. Passing a dictionary to **`json=`** serializes it to JSON *and* sets the `Content-Type` header; `response.json()` parses a JSON response body back into Python.

| Method | Function | Purpose | Typical Success Status |
|--------|----------|---------|------------------------|
| `GET` | `requests.get()` | Retrieve data, with query parameters (`params=`) | `200 OK` |
| `POST` | `requests.post()` | Create a new resource with a JSON body (`json=`) | `201 Created` or `200 OK` |
| `PUT` | `requests.put()` | Replace an **entire** existing resource | `200 OK` or `204 No Content` |
| `PATCH` | `requests.patch()` | Partially update specific fields | `200 OK` or `204 No Content` |
| `DELETE` | `requests.delete()` | Remove a resource by ID | `204 No Content` or `200 OK` |

> ⏱️ **Always pass `timeout=`.** Without it, `requests` can wait forever for a server that never answers. A single number (`timeout=10`) limits both connecting and waiting for the response.

### GET with Query Parameters

`params=` builds the query string (`?postId=1`) for you, including correct URL encoding. The response object carries the status code, headers and body.

```python
import requests

url = "https://jsonplaceholder.typicode.com/comments"

try:
    res = requests.get(url, params={"postId": 1}, timeout=10)
    res.raise_for_status()                      # raises for 4xx / 5xx answers
    comments = res.json()

    print("Final URL   :", res.url)
    print("Status      :", res.status_code)
    print("Content-Type:", res.headers["Content-Type"])
    print("Comments    :", len(comments))
    for c in comments[:2]:
        print(" -", c["email"])
except requests.exceptions.RequestException as err:
    print("Request failed:", err)
```

**Output:**

```text
Final URL   : https://jsonplaceholder.typicode.com/comments?postId=1
Status      : 200
Content-Type: application/json; charset=utf-8
Comments    : 5
 - Eliseo@gardner.biz
 - Jayne_Kuhic@sydney.com
```

### POST: Creating a Resource

`json=` is all you need to send a JSON body. The server normally answers with the created object, including the **new `id`** it assigned.

```python
url = "https://jsonplaceholder.typicode.com/posts"
payload = {"title": "Learning Python APIs", "body": "Practice makes perfect.", "userId": 10}

res = requests.post(url, json=payload, timeout=10)
res.raise_for_status()
created = res.json()
print(f"Status: {res.status_code} | New Post ID: {created.get('id')}")
```

**Output:**

```text
Status: 201 | New Post ID: 101
```

### PUT, PATCH & DELETE (Handling `204 No Content`)

- **PUT** sends the *complete* new version of the resource. Fields you leave out may be removed.
- **PATCH** sends *only the fields that change*.

> ⚠️ **Pitfall:** many APIs answer `PUT` or `DELETE` with **`204 No Content`** and an **empty body**. Calling `response.json()` on an empty body raises a `JSONDecodeError`, so check `response.status_code == 204` or `not response.content` first. The helper below does that check once so you can reuse it.

```python
def safe_json(response):
    """Return the parsed JSON body, or None when the response has no body."""
    if response.status_code == 204 or not response.content:
        return None
    return response.json()


url = "https://jsonplaceholder.typicode.com/posts/1"

put_res = requests.put(url, json={"id": 1, "title": "New Title", "body": "Full text", "userId": 1}, timeout=10)
put_res.raise_for_status()
print("PUT   ->", put_res.status_code, "| title:", safe_json(put_res)["title"])

patch_res = requests.patch(url, json={"title": "Patched Title"}, timeout=10)
patch_res.raise_for_status()
print("PATCH ->", patch_res.status_code, "| title:", safe_json(patch_res)["title"])

del_res = requests.delete(url, timeout=10)
del_res.raise_for_status()
body = safe_json(del_res)
if body is None:
    print("DELETE -> deleted with an empty body.")
else:
    print("DELETE ->", del_res.status_code, "| body:", body)
```

**Output:**

```text
PUT   -> 200 | title: New Title
PATCH -> 200 | title: Patched Title
DELETE -> 200 | body: {}
```

> 💡 JSONPlaceholder answers `DELETE` with `200` and an empty JSON object `{}`, while many other APIs answer `204`. The `safe_json()` check handles both, which is why code that talks to different APIs should never assume one convention.

## Handling Errors

A request can fail in three different ways, and each raises a different exception. They all inherit from `requests.exceptions.RequestException`, so one `except` clause can catch everything, or you can catch the specific ones first.

| Exception | When it happens |
|-----------|-----------------|
| `requests.exceptions.HTTPError` | The server answered, but with a 4xx / 5xx status, **raised only by `raise_for_status()`** |
| `requests.exceptions.Timeout` | No answer within `timeout=` seconds |
| `requests.exceptions.ConnectionError` | The server could not be reached (no internet, wrong host, refused connection) |
| `requests.exceptions.RequestException` | The parent of all of the above, so it catches any of them |

> ⚠️ `requests` does **not** raise for a `404` or `500` on its own: you get a normal response object. Without `raise_for_status()` (or a check of `status_code`) your code may happily parse an error page as data.

```python
def get_json(url, **kwargs):
    """GET a URL and return its JSON, translating each failure into a clear message."""
    try:
        res = requests.get(url, timeout=kwargs.pop("timeout", 10), **kwargs)
        res.raise_for_status()
        return res.json()
    except requests.exceptions.HTTPError as err:
        print(f"HTTP {err.response.status_code}: the server rejected the request")
    except requests.exceptions.Timeout:
        print("Timeout: the server took too long")
    except requests.exceptions.ConnectionError:
        print("ConnectionError: could not reach the server")
    return None


print("--- 404: the server answers, but with an error status ---")
get_json("https://jsonplaceholder.typicode.com/posts/9999")

print("\n--- Timeout: /slow needs 1 second but we only allow 0.3 ---")
get_json(f"{BASE}/slow", timeout=0.3)

print("\n--- ConnectionError: nothing listens on port 1 ---")
get_json("http://127.0.0.1:1/")

print("\n--- Success ---")
print(get_json("https://jsonplaceholder.typicode.com/posts/1")["id"])
```

**Output:**

```text
--- 404: the server answers, but with an error status ---
HTTP 404: the server rejected the request

--- Timeout: /slow needs 1 second but we only allow 0.3 ---
Timeout: the server took too long

--- ConnectionError: nothing listens on port 1 ---
ConnectionError: could not reach the server

--- Success ---
1
```

## Authentication with `requests.Session()`

A **`Session`** remembers settings between requests. It is the right tool whenever you make more than one call to the same service, for two reasons:

1. **Less typing**: headers, cookies and credentials set on the session are sent with **every** request automatically.
2. **Faster**: the session re-uses the underlying TCP connection instead of reconnecting each time.

Using it as a context manager (`with requests.Session() as session:`) closes the connection pool for you.

### Pattern A: Bearer Token / API Key Header

Most modern APIs expect `Authorization: Bearer <token>`. httpbin's `/headers` endpoint echoes back the headers it received, which proves the session attached ours.

```python
with requests.Session() as session:
    session.headers.update({
        "Authorization": "Bearer demo-token-123",
        "Accept": "application/json",
    })

    res = session.get("https://httpbin.org/headers", timeout=10)   # no headers= needed here
    res.raise_for_status()

    sent_headers = res.json().get("headers", {})
    print("Server received Auth  :", sent_headers.get("Authorization"))
    print("Server received Accept:", sent_headers.get("Accept"))
```

**Output:**

```text
Server received Auth  : Bearer demo-token-123
Server received Accept: application/json
```

> 🔐 Never hard-code real tokens in your source. Read them from the environment instead: `TOKEN = os.environ.get("API_TOKEN")`, so the secret stays out of your code and out of Git.

### Pattern B: HTTP Basic Authentication

For APIs that use a username and password, set `session.auth` (or pass `auth=(user, password)` to a single call). `requests` builds the `Authorization: Basic ...` header for you.

```python
with requests.Session() as session:
    session.auth = ("dev", "secret")
    print("Right credentials:", session.get(f"{BASE}/basic", timeout=10).json())

    session.auth = ("dev", "wrong")
    res = session.get(f"{BASE}/basic", timeout=10)
    print("Wrong credentials:", res.status_code, res.json())
```

**Output:**

```text
Right credentials: {'authenticated': True}
Wrong credentials: 401 {'error': 'bad credentials'}
```

### Pattern C: Cookie-Based Login

Some APIs log you in once and then recognise you by a **cookie**. A `Session` stores every `Set-Cookie` header it receives and sends the cookie back on later requests, so you log in only once. A plain `requests.get()` has no memory, so the same call fails:

```python
with requests.Session() as session:
    login = session.post(f"{BASE}/login", json={"email": "dev@example.com", "password": "secret"}, timeout=10)
    login.raise_for_status()
    print("Cookies stored by the session:", session.cookies.get_dict())

    dashboard = session.get(f"{BASE}/dashboard", timeout=10)       # cookie sent automatically
    print("With the session   :", dashboard.status_code, dashboard.json())

without = requests.get(f"{BASE}/dashboard", timeout=10)            # no session, no cookie
print("Without the session:", without.status_code, without.json())
```

**Output:**

```text
Cookies stored by the session: {'sid': 'abc123'}
With the session   : 200 {'welcome': 'dev@example.com'}
Without the session: 401 {'error': 'not logged in'}
```

## Automatic Retries & Rate-Limit Backoff

Networks and servers fail for short moments: a `503 Service Unavailable` during a deployment, a `429 Too Many Requests` when you call too fast. Retrying a few times with a growing pause fixes most of these. You do not need to write that loop yourself: mount an **`HTTPAdapter`** configured with urllib3's **`Retry`** on the session.

| `Retry` option | Meaning |
|----------------|---------|
| `total` | Maximum number of retries |
| `backoff_factor` | Pause growth between retries (see the schedule below) |
| `status_forcelist` | Status codes that should trigger a retry, normally `429, 500, 502, 503, 504` |
| `allowed_methods` | HTTP methods that may be retried (default: the safe/idempotent ones: `GET`, `HEAD`, `PUT`, `DELETE`, `OPTIONS`, `TRACE`) |
| `respect_retry_after_header` | Obey the server's `Retry-After` header (on by default), important for `429` |
| `raise_on_status` | `True` (default): raise `RetryError` when retries run out. `False`: return the last response instead |

**Backoff schedule.** The first retry happens immediately, then the pause is `backoff_factor × 2^(n−1)` seconds, so:

| `backoff_factor` | 1st retry | 2nd retry | 3rd retry |
|-----------------:|----------:|----------:|----------:|
| 0.5 | 0 s | 1 s | 2 s |
| 1 | 0 s | 2 s | 4 s |

```text
backoff_factor=1  ->  wait 0s, 2s, 4s   (not 1s, 2s, 4s)
```

```python
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry


def get_retry_session(total=3, backoff_factor=0.5, token=None) -> requests.Session:
    """Return a Session that retries transient failures with exponential backoff."""
    session = requests.Session()
    if token:
        session.headers.update({"Authorization": f"Bearer {token}"})

    retry = Retry(
        total=total,
        backoff_factor=backoff_factor,
        status_forcelist=[429, 500, 502, 503, 504],
        allowed_methods=["HEAD", "GET", "PUT", "DELETE", "OPTIONS"],
        respect_retry_after_header=True,
    )
    adapter = HTTPAdapter(max_retries=retry)
    session.mount("https://", adapter)
    session.mount("http://", adapter)
    return session
```

The local server's `/flaky` endpoint fails twice and then succeeds. A plain request gives up at the first failure; the retry session keeps going and gets the good answer. (`backoff_factor=0.05` keeps the demo fast.)

```python
print("--- Plain requests.get: fails on the first 503 ---")
res = requests.get(f"{BASE}/flaky?key=plain&fail=2", timeout=5)
print("Status:", res.status_code, "| server saw", hits["plain"], "request")

print("\n--- Retry session: 2 failures are retried automatically ---")
with get_retry_session(backoff_factor=0.05) as s:
    res = s.get(f"{BASE}/flaky?key=retry&fail=2", timeout=5)
    print("Status:", res.status_code, res.json(), "| server saw", hits["retry"], "requests")
```

**Output:**

```text
--- Plain requests.get: fails on the first 503 ---
Status: 503 | server saw 1 request

--- Retry session: 2 failures are retried automatically ---
Status: 200 {'status': 'ok', 'attempt': 3} | server saw 3 requests
```

When the server **never** recovers, the retries run out. By default `requests` then raises `RetryError`. Pass `raise_on_status=False` to `Retry` if you would rather receive the last response and decide yourself.

```python
with get_retry_session(total=2, backoff_factor=0.05) as s:
    try:
        s.get(f"{BASE}/always503", timeout=5)
    except requests.exceptions.RetryError as err:
        print("RetryError:", err.args[0].reason)
    print("Server saw", hits["always503"], "requests (1 try + 2 retries)")

lenient = requests.Session()
lenient.mount("http://", HTTPAdapter(max_retries=Retry(
    total=2, backoff_factor=0.05, status_forcelist=[503], raise_on_status=False)))
print("raise_on_status=False ->", lenient.get(f"{BASE}/always503", timeout=5).status_code)
```

**Output:**

```text
RetryError: too many 503 error responses
Server saw 3 requests (1 try + 2 retries)
raise_on_status=False -> 503
```

> **Why `POST` is not in `allowed_methods`:** if a `POST` times out, the server may already have processed it. Retrying would then create a **duplicate** record (two orders, two payments). Only enable `POST` retries if the API supports an `Idempotency-Key` header that lets the server recognise repeats. The demo below shows that a failing `POST` is sent exactly once:

```python
with get_retry_session(backoff_factor=0.05) as s:
    res = s.post(f"{BASE}/flaky?key=post&fail=2", json={"order": 1}, timeout=5)
    print("POST status:", res.status_code, "| server saw", hits["post"], "request (not retried)")
```

**Output:**

```text
POST status: 503 | server saw 1 request (not retried)
```

## Handling API Pagination

Large datasets are split across **pages** so no single response is huge. You keep requesting pages until the API says there are no more. The loop should always use a `Session` and a small `time.sleep()` pause to respect rate limits. Which pattern to use depends on how the API describes its pages, so read its documentation.

### Pattern A: Page / Offset Loop

Request `page=1, 2, 3, …` and stop when a page comes back **empty**. (APIs differ in the parameter names, e.g. `page`/`limit`, `_page`/`_limit` or `offset`/`limit`, and in where the list sits: a bare list or inside a key such as `"results"`.)

```python
import time


def fetch_all_pages(session, base_url, page_size=10, delay=0.05):
    all_items = []
    page = 1
    while True:
        res = session.get(base_url, params={"page": page, "limit": page_size}, timeout=10)
        res.raise_for_status()
        batch = res.json().get("results", [])
        if not batch:                       # empty page -> no more data
            break
        all_items.extend(batch)
        print(f"page {page}: {len(batch)} items")
        page += 1
        time.sleep(delay)                   # be polite to the API
    return all_items


with requests.Session() as session:
    items = fetch_all_pages(session, f"{BASE}/items")
print("Total items:", len(items))
```

**Output:**

```text
page 1: 10 items
page 2: 10 items
page 3: 5 items
Total items: 25
```

### Pattern B: Next-URL / Cursor Traversal

Many APIs include the address of the next page in the response, either in the body (`"next"`) or as an opaque **cursor**. You never build page numbers yourself: just follow the link until it is `None`.

```python
def fetch_by_next_url(session, start_url):
    all_items = []
    next_url = start_url
    while next_url:
        res = session.get(next_url, timeout=10)
        res.raise_for_status()
        payload = res.json()
        all_items.extend(payload.get("results", []))
        next_url = payload.get("next")      # None on the final page
    return all_items


with requests.Session() as session:
    items = fetch_by_next_url(session, f"{BASE}/items?page=1&limit=10")
print("Total items:", len(items), "| last id:", items[-1]["id"])
```

**Output:**

```text
Total items: 25 | last id: 25
```

### Pattern C: The `Link` Header

Some APIs (GitHub is the best-known) put the next-page address in the standard HTTP **`Link` header** instead of the body. `requests` parses it for you into `response.links`.

### Bonus: a generator keeps memory low

Instead of collecting every item in a list, **`yield`** them one page at a time. The caller can process or stop early without the whole dataset ever being in memory.

```python
def iter_items(session, start_url):
    """Yield items one by one, following the Link header until there is no 'next'."""
    url = start_url
    while url:
        res = session.get(url, timeout=10)
        res.raise_for_status()
        yield from res.json()["results"]
        url = res.links.get("next", {}).get("url")      # parsed 'Link' header


with requests.Session() as session:
    first_item = next(iter_items(session, f"{BASE}/items?page=1&limit=10"))
    print("First item (only one page was fetched so far):", first_item)

    total = sum(1 for _ in iter_items(session, f"{BASE}/items?page=1&limit=10"))
    print("Items via the Link header:", total)
```

**Output:**

```text
First item (only one page was fetched so far): {'id': 1}
Items via the Link header: 25
```

## Clean Up & Key Takeaways

1. Use `requests.get/post/put/patch/delete`, send JSON with `json=`, build query strings with `params=`, and read the answer with `response.json()`.
2. **Always** set `timeout=` and call `response.raise_for_status()`; catch `requests.exceptions.RequestException` (or its specific children).
3. `PUT` / `DELETE` often return **no body** (`204`): check before calling `.json()`.
4. Use `requests.Session()` for several calls: it re-uses connections and automatically sends headers, auth and cookies.
5. Add an `HTTPAdapter` with `Retry` for `429` / `5xx` errors, and keep `POST` out of `allowed_methods` unless the API supports idempotency keys. `backoff_factor=1` waits 0 s, 2 s, 4 s.
6. For large datasets paginate with a page loop, a `next` URL or the `Link` header, pause briefly between pages, and consider a generator.

Stop the local practice server:

```python
server.shutdown()
server.server_close()
print("Practice server stopped.")
```

**Output:**

```text
Practice server stopped.
```
