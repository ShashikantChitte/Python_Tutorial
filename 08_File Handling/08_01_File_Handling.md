# Python File Handling

> [← Course index](../README.md)

File handling in Python allows us to **create, read, write, update, and delete** files and directories.

> 📝 Every example below runs inside a temporary `workspace` folder so it works identically on Windows, macOS, and Linux without leaving stray files behind.

## Opening a File (`open()` Modes)

```python
with open("filename.txt", "mode", encoding="utf-8") as file:
    # work with file (closes automatically when the block exits)
```

| Mode | Meaning |
|------|---------|
| `'r'` | **Read** (default). Raises `FileNotFoundError` if the file does not exist. |
| `'w'` | **Write**. Creates a new file if missing, or **overwrites** existing content. |
| `'a'` | **Append**. Creates a new file if missing, or adds content at the end **without overwriting**. |
| `'x'` | **Create**. Creates a new file, or raises `FileExistsError` if it already exists. |
| `'b'` | **Binary mode** (e.g., `'rb'`, `'wb'`) for images, PDFs, or non-text files. |

## Reusable Helper Functions & Safe Workspace Setup

All file and folder operations (`write_to_file`, `append_to_file`, `read_file`, `rename_file`, `delete_file`, `create_directory`, `check_directory_exists`, `rename_directory`, `remove_directory`, `list_directory`) are wrapped in clean helper functions below.

```python
import os
import shutil
import tempfile


def write_to_file(file_path, content):
    """'w' mode: creates the file, or OVERWRITES it if it exists."""
    with open(file_path, "w", encoding="utf-8") as file:
        file.write(content)
    print(f"Content written to {os.path.basename(file_path)}")


def append_to_file(file_path, content):
    """'a' mode: adds to the end without deleting old content."""
    with open(file_path, "a", encoding="utf-8") as file:
        file.write(content)
    print(f"Content appended to {os.path.basename(file_path)}")


def read_file(file_path, mode="all"):
    """Read the whole file ('all'), one line ('line'), or a list of lines ('lines')."""
    with open(file_path, "r", encoding="utf-8") as file:
        if mode == "all":
            return file.read()
        elif mode == "line":
            return file.readline()
        elif mode == "lines":
            return file.readlines()
        raise ValueError("Invalid mode. Use 'all', 'line', or 'lines'.")


def rename_file(source, target):
    os.rename(source, target)
    print(f"File renamed: {os.path.basename(source)} -> {os.path.basename(target)}")


def delete_file(file_path):
    if os.path.exists(file_path):
        os.remove(file_path)
        print(f"File {os.path.basename(file_path)} deleted")
    else:
        print(f"File {os.path.basename(file_path)} does not exist")


def create_directory(dir_path):
    os.makedirs(dir_path, exist_ok=True)
    print(f"Directory {os.path.basename(dir_path)} created")


def check_directory_exists(dir_path):
    return os.path.exists(dir_path)


def rename_directory(source, target):
    os.rename(source, target)
    print(f"Directory renamed: {os.path.basename(source)} -> {os.path.basename(target)}")


def remove_directory(dir_path, force=False):
    if force:
        shutil.rmtree(dir_path)                 # removes the folder even if it has files
    else:
        os.rmdir(dir_path)                      # only works on an EMPTY folder
    print(f"Directory {os.path.basename(dir_path)} removed")


workspace = tempfile.mkdtemp()                      # temporary folder for all examples
path = lambda name: os.path.join(workspace, name)   # build a path inside it
print("Temporary workspace ready.")
```

**Output:**

```text
Temporary workspace ready.
```

## Writing (`'w'`) and Appending (`'a'`) to a File

- `'w'` creates a new file or **wipes** existing content.
- `'a'` preserves old content and adds new text at the end.
- Using `with open(...)` guarantees the file is closed automatically, even if an error happens.

```python
print("--- Writing ('w'): creates the file ---")
write_to_file(path("myfile.txt"), "Welcome to Python\nFile Handling")

print("\n--- 'w' again OVERWRITES the old content ---")
write_to_file(path("overwrite.txt"), "first version")
write_to_file(path("overwrite.txt"), "second version")
print("overwrite.txt content ->", repr(read_file(path("overwrite.txt"))))

print("\n--- Appending ('a'): keeps the old content ---")
append_to_file(path("myfile.txt"), "\nThis line is appended.")
```

**Output:**

```text
--- Writing ('w'): creates the file ---
Content written to myfile.txt

--- 'w' again OVERWRITES the old content ---
Content written to overwrite.txt
Content written to overwrite.txt
overwrite.txt content -> 'second version'

--- Appending ('a'): keeps the old content ---
Content appended to myfile.txt
```

## Reading from a File (`read()`, `readline()`, `readlines()`, and Looping)

- `read()` → returns the entire file as one string.
- `readline()` → reads a single line at a time.
- `readlines()` → returns a list of all lines.
- `for line in file:` → memory-efficient way to read large files line by line.

```python
print("--- Reading: read(), readline(), readlines() ---")
print("read()      ->", repr(read_file(path("myfile.txt"), "all")))
print("readline()  ->", repr(read_file(path("myfile.txt"), "line")))
print("readlines() ->", read_file(path("myfile.txt"), "lines"))

print("\n--- Line-by-line loop (best for large files) ---")
with open(path("myfile.txt"), "r", encoding="utf-8") as f:
    for idx, line in enumerate(f, start=1):
        print(f"Line {idx}: {line.strip()}")
```

**Output:**

```text
--- Reading: read(), readline(), readlines() ---
read()      -> 'Welcome to Python\nFile Handling\nThis line is appended.'
readline()  -> 'Welcome to Python\n'
readlines() -> ['Welcome to Python\n', 'File Handling\n', 'This line is appended.']

--- Line-by-line loop (best for large files) ---
Line 1: Welcome to Python
Line 2: File Handling
Line 3: This line is appended.
```

## Exclusive Create (`'x'`) & Handling Missing Files (`'r'`)

```python
print("--- Mode 'x': create only if the file does NOT exist ---")
try:
    with open(path("myfile.txt"), "x", encoding="utf-8") as file:
        file.write("never written")
except FileExistsError:
    print("FileExistsError: myfile.txt already exists")

print("\n--- Mode 'r': the file must exist ---")
try:
    open(path("missing.txt"), "r", encoding="utf-8")
except FileNotFoundError:
    print("FileNotFoundError: missing.txt does not exist")
```

**Output:**

```text
--- Mode 'x': create only if the file does NOT exist ---
FileExistsError: myfile.txt already exists

--- Mode 'r': the file must exist ---
FileNotFoundError: missing.txt does not exist
```

## Renaming & Deleting Files

```python
print("--- Renaming a file ---")
rename_file(path("myfile.txt"), path("myfile1.txt"))

print("\n--- Deleting a file ---")
delete_file(path("myfile1.txt"))
delete_file(path("myfile1.txt"))        # second time: safely reports not found
```

**Output:**

```text
--- Renaming a file ---
File renamed: myfile.txt -> myfile1.txt

--- Deleting a file ---
File myfile1.txt deleted
File myfile1.txt does not exist
```

## Working with Directories (`os` & `shutil`)

| Task | Function |
|------|----------|
| Create directory | `os.mkdir(path)` or `os.makedirs(path, exist_ok=True)` |
| Check existence | `os.path.exists(path)` |
| List contents | `os.listdir(path)` |
| Rename directory | `os.rename(src, dst)` |
| Remove **empty** directory | `os.rmdir(path)` |
| Remove **non-empty** directory | `shutil.rmtree(path)` |
| Current working directory | `os.getcwd()` |

```python
print("--- Creating / checking / listing / renaming / removing a directory ---")
create_directory(path("mydir"))
print("exists?  ", check_directory_exists(path("mydir")))
print("contents:", os.listdir(workspace))
rename_directory(path("mydir"), path("mydir1"))
remove_directory(path("mydir1"))
print("exists after removal?", check_directory_exists(path("mydir1")))

print("\n--- rmdir() only removes EMPTY folders; use shutil.rmtree for non-empty ---")
create_directory(path("full"))
write_to_file(os.path.join(path("full"), "a.txt"), "data")
try:
    remove_directory(path("full"))
except OSError:
    print("OSError: folder is not empty -> using force=True (shutil.rmtree)")
    remove_directory(path("full"), force=True)

print("\nCurrent working directory:", os.getcwd())
```

**Output:**

```text
--- Creating / checking / listing / renaming / removing a directory ---
Directory mydir created
exists?   True
contents: ['mydir', 'overwrite.txt']
Directory renamed: mydir -> mydir1
Directory mydir1 removed
exists after removal? False

--- rmdir() only removes EMPTY folders; use shutil.rmtree for non-empty ---
Directory full created
Content written to a.txt
OSError: folder is not empty -> using force=True (shutil.rmtree)
Directory full removed

Current working directory: /path/to/Python_Notebooks/08_File Handling
```

## Hands-On Practice Exercises

Try solving these in the code cell below (using `path("filename")`) before checking the solutions!

1. **Notes Logger:** Write 3 lines (`"Task 1"`, `"Task 2"`, `"Task 3"`) to `path("tasks.txt")`, then append `"Task 4"` on a new line.
2. **Line Counter:** Read `path("tasks.txt")` using `readlines()` and print the total number of lines.
3. **Cleanup:** Delete `path("tasks.txt")` and verify with `os.path.exists()` that it is gone.

```python
# Your practice code here
```

### Solutions

```python
# 1. Notes Logger
write_to_file(path("tasks.txt"), "Task 1\nTask 2\nTask 3")
append_to_file(path("tasks.txt"), "\nTask 4")

# 2. Line Counter
lines = read_file(path("tasks.txt"), "lines")
print("Total tasks:", len(lines))

# 3. Cleanup
delete_file(path("tasks.txt"))
print("Still exists?", os.path.exists(path("tasks.txt")))
```

**Output:**

```text
Content written to tasks.txt
Content appended to tasks.txt
Total tasks: 4
File tasks.txt deleted
Still exists? False
```

## Clean Up & Key Takeaways

Delete the temporary workspace used by the examples:

1. Always open files using `with open(..., encoding="utf-8") as f:` so they close automatically.
2. Remember the four core modes: `'r'` (read), `'w'` (overwrite), `'a'` (append), and `'x'` (exclusive create).
3. Use `os.path.exists()` before reading or deleting, and `shutil.rmtree()` for non-empty folders.

```python
shutil.rmtree(workspace)
print("Temporary workspace deleted:", not os.path.exists(workspace))
```

**Output:**

```text
Temporary workspace deleted: True
```
