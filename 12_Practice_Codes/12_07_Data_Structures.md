# Data Structures

> [← Practice Codes index](README.md)

Stacks, queues, linked lists, trees, graphs and caches implemented from scratch.

**10 problems** · 1 Easy · 4 Medium · 5 Hard

| # | Problem | Level |
|--:|---------|-------|
| 1 | [Implement a stack using a list](#1-implement-a-stack-using-a-list) | Easy |
| 2 | [Implement a queue using two stacks](#2-implement-a-queue-using-two-stacks) | Medium |
| 3 | [Implement a linked list and reverse it](#3-implement-a-linked-list-and-reverse-it) | Medium |
| 4 | [Detect a cycle in a linked list (Floyd's tortoise and hare)](#4-detect-a-cycle-in-a-linked-list-floyds-tortoise-and-hare) | Medium |
| 5 | [Next smaller element (monotonic stack)](#5-next-smaller-element-monotonic-stack) | Medium |
| 6 | [Implement an LRU cache](#6-implement-an-lru-cache) | Hard |
| 7 | [Level-order traversal of a binary tree](#7-level-order-traversal-of-a-binary-tree) | Hard |
| 8 | [Validate a binary search tree](#8-validate-a-binary-search-tree) | Hard |
| 9 | [Find the shortest path in a graph using BFS](#9-find-the-shortest-path-in-a-graph-using-bfs) | Hard |
| 10 | [Celebrity problem – O(n)](#10-celebrity-problem--on) | Hard |

---

## Easy

### 1. Implement a stack using a list

```python
class Stack:
    def __init__(self):
        self._data = []

    def push(self, x): self._data.append(x)

    def pop(self):
        if self.is_empty():
            raise IndexError("pop from empty stack")
        return self._data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("peek from empty stack")
        return self._data[-1]

    def is_empty(self): return not self._data
    def __len__(self): return len(self._data)
```

---

## Medium

### 2. Implement a queue using two stacks

```python
class QueueTwoStacks:
    def __init__(self):
        self._in, self._out = [], []

    def enqueue(self, x):
        self._in.append(x)

    def dequeue(self):
        if not self._out:
            while self._in:
                self._out.append(self._in.pop())
        if not self._out:
            raise IndexError("dequeue from empty queue")
        return self._out.pop()

q = QueueTwoStacks()
q.enqueue(1); q.enqueue(2)
print(q.dequeue(), q.dequeue())  # 1 2

# ------------Expected Output---------

# 1 2
```

### 3. Implement a linked list and reverse it

```python
class Node:
    def __init__(self, val, next=None):
        self.val, self.next = val, next

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, val):
        node = Node(val)
        if not self.head:
            self.head = node
            return
        cur = self.head
        while cur.next:
            cur = cur.next
        cur.next = node

    def reverse(self):
        prev, cur = None, self.head
        while cur:
            cur.next, prev, cur = prev, cur, cur.next
        self.head = prev

    def to_list(self):
        out, cur = [], self.head
        while cur:
            out.append(cur.val)
            cur = cur.next
        return out

ll = LinkedList()
for v in [1, 2, 3]:
    ll.append(v)
ll.reverse()
print(ll.to_list())  # [3, 2, 1]

# ------------Expected Output---------

# [3, 2, 1]
```

### 4. Detect a cycle in a linked list (Floyd's tortoise and hare)

```python
def has_cycle(head) -> bool:  # Floyd's tortoise and hare
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast:
            return True
    return False
```

### 5. Next smaller element (monotonic stack)

```python
def next_smaller_element(arr):
    stack, res = [], [-1] * len(arr)
    for i in range(len(arr) - 1, -1, -1):
        while stack and stack[-1] >= arr[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(arr[i])
    return res

print("Next Smaller Elem :", next_smaller_element([4, 8, 5, 2, 25]))

# ------------Expected Output---------

# Next Smaller Elem : [2, 5, 2, -1, -1]
```

---

## Hard

### 6. Implement an LRU cache

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.data = OrderedDict()

    def get(self, key):
        if key not in self.data:
            return -1
        self.data.move_to_end(key)
        return self.data[key]

    def put(self, key, value):
        self.data[key] = value
        self.data.move_to_end(key)
        if len(self.data) > self.cap:
            self.data.popitem(last=False)

c = LRUCache(2)
c.put(1, "a"); c.put(2, "b"); c.get(1); c.put(3, "c")
print(c.get(2))  # -1 (evicted)

# ------------Expected Output---------

# -1
```

### 7. Level-order traversal of a binary tree

```python
from collections import deque

class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val, self.left, self.right = val, left, right

def level_order(root):
    if not root:
        return []
    result, q = [], deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        result.append(level)
    return result

tree = TreeNode(3, TreeNode(9), TreeNode(20, TreeNode(15), TreeNode(7)))
print(level_order(tree))  # [[3], [9, 20], [15, 7]]

# ------------Expected Output---------

# [[3], [9, 20], [15, 7]]
```

### 8. Validate a binary search tree

```python
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val, self.left, self.right = val, left, right

def is_valid_bst(node, low=float("-inf"), high=float("inf")) -> bool:
    if not node:
        return True
    if not (low < node.val < high):
        return False
    return is_valid_bst(node.left, low, node.val) and is_valid_bst(node.right, node.val, high)

bst = TreeNode(5, TreeNode(3), TreeNode(8))

print("Is Valid BST      :", is_valid_bst(bst))

# ------------Expected Output---------

# Is Valid BST      : True
```

### 9. Find the shortest path in a graph using BFS

```python
from collections import deque

def shortest_path(graph: dict, start, goal):
    q = deque([[start]])
    visited = {start}
    while q:
        path = q.popleft()
        node = path[-1]
        if node == goal:
            return path
        for nxt in graph.get(node, []):
            if nxt not in visited:
                visited.add(nxt)
                q.append(path + [nxt])
    return None

g = {"A": ["B", "C"], "B": ["D"], "C": ["D"], "D": ["E"], "E": []}
print(shortest_path(g, "A", "E"))  # ['A', 'B', 'D', 'E']

# ------------Expected Output---------

# ['A', 'B', 'D', 'E']
```

### 10. Celebrity problem – O(n)

```python
def celebrity(matrix):
    n = len(matrix)
    stack = list(range(n))
    while len(stack) > 1:
        a, b = stack.pop(), stack.pop()
        stack.append(b if matrix[a][b] == 1 else a)
    cand = stack[0]
    for i in range(n):
        if i != cand and (matrix[cand][i] == 1 or matrix[i][cand] == 0):
            return -1
    return cand

print(celebrity([[0, 1, 1], [0, 0, 1], [0, 0, 0]]))

# ------------Expected Output---------

# 2
```
