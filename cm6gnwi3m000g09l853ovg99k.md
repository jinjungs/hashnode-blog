---
title: "Ultimate Python Cheatsheet for Coding Interviews: Input, Logic, Strings, Sorting & More"
seoTitle: "Python Coding Interview Cheatsheet: Key Concepts"
seoDescription: "Comprehensive Python cheatsheet for coding interviews covering input/output, syntax, strings, sorting, data structures, and more essential concepts"
datePublished: Tue Jan 28 2025 15:58:30 GMT+0000 (Coordinated Universal Time)
cuid: cm6gnwi3m000g09l853ovg99k
slug: coding-test-essential-libraries
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750309819576/cb0dab59-c399-4820-b37e-0907e065488f.png
tags: python, coding, coding-interview, python-libraries, coding-test, python-cheatsheet

---

Right before a coding test, you don’t need a textbook — you need **a laser-focused cheat sheet.**

This post summarizes the Python essentials you’ll actually use in interviews: input/output, syntax, strings, sorting, and more.

**Skim it once, ace it fast.**

---

## 📥 1. Input & Output for Coding Interviews

> 📌 Note: Most competitive programming platforms ([HackerRank](https://www.hackerrank.com/), [Codeforces](https://codeforces.com/), etc. require using `input()` to read data. Only platforms like **LeetCode** provide function arguments directly and do not use standard input/output.

### 1.1. Basic Input

* Read a single integer or a list of space-separated integers.
    

```python
n = int(input())
# Input: 5

a, b = map(int, input().split())
# Input: 10 20
# Output: a = 10, b = 20

arr = list(map(int, input().split()))
# Input: 1 2 3 4 5
# Output: arr = [1, 2, 3, 4, 5]
```

### 1.2. Multiple Lines of Input

* Useful when input is structured across multiple lines.
    

```python
for _ in range(2):
    x, y = map(int, input().split())
# Input:
# 1 2
# 3 4
```

* If each line contains a string and you want to split it into characters (e.g. for grid input), use `list(input())` instead of `input()`:
    

```python
n = 3
arr = [list(input()) for _ in range(n)]
# Input:
# AAAA
# ABCA
# AAAA
# Output:
# [['A', 'A', 'A', 'A'],
#  ['A', 'B', 'C', 'A'],
#  ['A', 'A', 'A', 'A']]
```

* `*arr` collects the remaining values into a list during unpacking.
    

```python
N, *arr = map(int, input().split())
# Input: 4 10 20 30 40
# Output: N = 4, arr = [10, 20, 30, 40]
```

### 1.3. Fast Input with `sys.stdin.readline`

* Much faster than `input()` for large input. It includes a trailing newline, so use `.strip()`.
    

```python
import sys
line = sys.stdin.readline().strip()
# Input: hello world
# Output: 'hello world'
```

### 1.4. Reading All Input at Once

* Use this when the number of input lines is unknown or very large.
    

```python
import sys
data = sys.stdin.read().split()
# Input:
# 1 2 3
# 4 5 6
# Output: ['1', '2', '3', '4', '5', '6']
```

### 1.5. Print Patterns

* Versatile ways to print values and format output.
    

```python
arr = [1, 2, 3]
a, b = 10, 'hi'

print("Answer:", 42)              # Output: Answer: 42
print(*arr)                       # Output: 1 2 3
print(1, end=' ')                 # Output: 1 
print(f"a: {a}, b: {b}")          # Output: a: 10, b: hi
```

---

## ✍️ 2. Basic Python Syntax

### 2.1. List Comprehension

* Compact way to build or transform lists.
    

```python
[x * 2 for x in range(5)]
# Output: [0, 2, 4, 6, 8]
```

### 2.2. Slicing

* Extract parts of a list or string efficiently.
    

```python
arr = [1, 2, 3, 4, 5]
arr[::-1]   # Output: [5, 4, 3, 2, 1]
arr[1:4]    # Output: [2, 3, 4]
arr[1:]     # Output: [2, 3, 4, 5]
arr[:4]     # Output: [1, 2, 3, 4]
```

### 2.3. Tuple Unpacking

* Swap or extract multiple values in one line.
    

```python
a, b = 1, 2
a, b = b, a
# Output: a = 2, b = 1
```

### 2.4. Conditional Expression

* Python uses `a if condition else b` instead of the C-style ternary operator `? :`.
    

```python
def sign(n):
    return "positive" if n > 0 else "zero or negative"

# Output:
sign(3)    # 'positive'
sign(-1)   # 'zero or negative'
```

### 2.5. Control: for-else

* `for-else` runs `else` only if no `break` occurred.
    

```python
arr = [1, 2, 3]
for x in arr:
    if x == 0:
        break
else:
    print("not found")
# Output: not found
```

### 2.6. Lambda Functions

* `lambda` is a way to write small anonymous functions in a single line.
    
* Often used as a quick helper for `key=`, `map()`, or `filter()`.
    
* Returning a tuple when sorting means: **the list is sorted by the first element of the tuple; if equal, then by the second, and so on.** — e.g., first by length, then lexicographically.
    

```python
# Normal function
def square(x):
    return x * x

# Equivalent lambda
square = lambda x: x * x
print(square(3))  # Output: 9

# Used in sorting
words = ["house", "hello", "a"]
words.sort(key=lambda x: (len(x), x))
# Output: ['a', 'hello', 'house']
```

---

## 🔁 3. Iteration & Built-in Functions

### 3.1. `enumerate`

* Use when you need both index and value while looping.
    

```python
for i, val in enumerate(['a', 'b']):
    print(i, val)
# Output:
# 0 a
# 1 b
```

### 3.2. `zip`

* Loop through multiple lists in parallel.
    

```python
for a, b in zip([1, 2], ['a', 'b']):
    print(a, b)
# Output:
# 1 a
# 2 b
```

### 3.3. `any`, `all`

* Check for any or all True values in a list.
    

```python
any([False, True])   # Output: True
all([True, True])    # Output: True
```

---

## ⛏️ 4. Data Structures

### 4.0. Common Operations on Python Data Structures

| Structure | Add Element | Remove Element | Notes |
| --- | --- | --- | --- |
| `list` | `append(x)` | `pop()` | Stack (LIFO), O(1) |
| `deque` | `append(x)` | `popleft()` / `pop()` | Queue/Stack, O(1) from both ends |
| `set` | `add(x)` | `remove(x)` / `discard(x)` | No duplicates, unordered |
| `dict` | `d[k] = v` | `del d[k]` | Key-value pairs |
| `heapq` | `heappush(h, x)` | `heappop(h)` | Min-heap by default |
| 💡 Tip |  |  |  |

* `remove(x)` on a `set` raises an error if `x` is not present. Use `discard(x)` to avoid exceptions.
    
* Use `d.get(k, default)` to avoid `KeyError` if key not present.
    

### 4.1. Set

* Unordered collection of unique elements. Fast lookup.
    

```python
s = set()
s.add(1)
s.add(2)
s.remove(1)    # Remove existing element
s.discard(3)   # Safe remove (does nothing if not found)
print(2 in s)  # True
```

### 4.2. Dictionary

* Key-value store, great for counting or mapping.
    
* 💡 Tip: `d[x]` raises an error if `x` does not exist. Use `d.get(x)` or `d.get(x, default)` to safely retrieve values.
    

```python
d = {'a': 1, 'b': 2}
d['c'] = 3           # Add a new key-value pair
print(d['a'])        # Accessing existing key
# print(d['x'])      # KeyError if 'x' doesn't exist
print(d.get('x'))    # None (safe way to access)
print(d.get('x', 0)) # 0 (default fallback value)
```

* `collections.defaultdict` lets you define a default type for missing keys:
    

```python
from collections import defaultdict
d = defaultdict(int)
d['missing'] += 1  # Now d['missing'] is 1, not KeyError
```

* Get keys and values by `items()`, `keys()`, `values`.
    

```python
# keys and values – iterate through both key and value pairs
for k, v in d.items():
    print(k, v)

# keys only – iterate through all keys
for k in d.keys():
    print(k)

# values only – iterate through all values
for v in d.values():
    print(v)
```

### 4.3. List

* Use `list` for LIFO stack.
    

```python
stack = []
stack.append(1)
stack.append(2)
print(stack.pop())  # Output: 2
```

### 4.4. Deque (Double-ended Queue)

* Use `deque` for FIFO queue.
    

```python
from collections import deque
q = deque()
q.append(1)         # [1]
q.append(2)         # [1,2]
q.append(3)         # [1,2,3]
print(q.popleft())  # [2,3]   Output: 1
print(q.pop())      # [2]     Output: 3
```

### 4.5. Heap

* Heap is a priority queue that allows efficient access to the smallest element.
    
* min-heap by default in Python.
    

```python
import heapq
h = []
heapq.heappush(h, 3)
heapq.heappush(h, 1)
heapq.heappush(h, 2)
print(heapq.heappop(h))  # Output: 1 (smallest element)
```

---

## 🧵 5. String Manipulation Tips

### 5.1. String Search and Matching

* `s.find(sub)`: Returns the first occurrence of `sub` or `-1` if not found.
    
* `s.rfind(sub)`: Like `find`, but searches from the end.
    
* `s.count(sub)`: Count occurrences of a substring.
    

### 5.2. Stripping and Replacing

* `s.strip()`, `s.lstrip()`, `s.rstrip()`: Remove whitespace or specified characters.
    
* `s.replace(old, new)`: Replace all occurrences of `old` with `new`.
    

```python
s = " hello "
s.strip()            # Output: 'hello'
s.lstrip()           # Output: 'hello '
s.rstrip()           # Output: ' hello'
s.replace('h', 'm')  # Output: 'mello'
```

### 5.3. Splitting and Joining

* `s.split(sep)`: Split a string into a list of substrings.
    
* `'sep'.join(iterable)`: Join elements of an iterable with a string.
    

```python
"a b c".split()                # Output: ['a', 'b', 'c']
"a-b-c".split("-")             # Output: ['a', 'b', 'c']
"-".join(['a', 'b', 'c'])      # Output: 'a-b-c'
```

### 5.4. Slicing and Reversing

* `s[start:end:step]`: Slice a string.
    
* `s[::-1]`: Reverse a string.
    

```python
s = "abcdefg"
s[1:6:2] # Output: 'bdf'
s[::-1]  # Output: 'gfedcba'
```

### 5.5. Case Handling

* `s.lower()`, `s.upper()`: Convert all characters to lowercase / uppercase.
    
* `s.capitalize()`: Capitalize the first character.
    
* `s.title()`: Capitalize the first letter of each word.
    
* `s.swapcase()`: Swap uppercase to lowercase and vice versa.
    

```python
s = "hELlo WoRLd"
s.lower()      # Output: 'hello world'
s.upper()      # Output: 'HELLO WORLD'
s.capitalize() # Output: 'Hello world'
s.title()      # Output: 'Hello World'
s.swapcase()   # Output: 'HelLO wOrlD'
```

### 5.6. ASCII ↔ Char

* `ord(c)`: Return ASCII value of character `c`.
    
* `chr(i)`: Return character for ASCII value `i`.
    

```python
ord('a')  # Output: 97
chr(97)   # Output: 'a'
```

### 5.7. Character Checks

* `s.isdigit()`, `s.isalpha()`, `s.isalnum()`: Check if the string is all digits, letters, or alphanumeric.
    
* `s.isspace()`: Check if the string is only whitespace.
    
* `startswith()`, `endswith()`: Check if a string starts or ends with a substring.
    

```python
s = "abc123"
print(s.isalpha())       # Output: False (contains digits)
print(s.isdigit())       # Output: False (contains letters)
print(s.isalnum())       # Output: True  (only letters and digits)

print("abc".isalpha())   # Output: True
print("123".isdigit())   # Output: True

print("".isspace())      # Output: False (empty string is not whitespace)
print("   ".isspace())   # Output: True

s = "filename.py"
s.startswith("file")     # Output: True
s.endswith(".py")        # Output: True
```

### 5.8. Character Counting

* `Counter`: Count frequency of each character.
    

```python
from collections import Counter
Counter("aab")  # Output: Counter({'a': 2, 'b': 1})
```

---

## 🔤 6. String Sorting Techniques

### 6.1. In-place Sort with `sort()`

* `sort()` modifies the list in-place and returns `None`. Use it when you don't need the original list.
    

```python
# 1. basic
words = ["banana", "apple", "cherry"]
words.sort()                      # In-place sort
print(words)                      # Output: ['apple', 'banana', 'cherry']

# 2. reverse
words.sort(reverse=True)         # Reverse order
print(words)                     # Output: ['cherry', 'banana', 'apple']
```

### 6.2. New List Sort with `sorted()`

* `sorted()` returns a new sorted list, leaving the original untouched.
    

```python
# 1. basic
words = ["banana", "apple", "cherry"]
result = sorted(words)           # New sorted list
print(result)                    # Output: ['apple', 'banana', 'cherry']

# 2. reverse
resultReversed = sorted(words, reverse=True)
print(resultReversed)            # Output: ['cherry', 'banana', 'apple']

# 3. sort string
s = "bdca"
print("".join(sorted(s)))       # Output: 'abcd'
```

### 6.3. Sort by Length `(key=len)`

* Use `key=len` to sort by string length.
    

```python
words = ["hi", "hello", "a"]
words.sort(key=len)
print(words)  # Output: ['a', 'hi', 'hello']
```

### 6.4. Case-insensitive Sort `(key=str.lower)`

* Use `key=str.lower` to ignore case during sort.
    

```python
words = ["Banana", "apple", "Cherry"]
words.sort(key=str.lower)
print(words)  # Output: ['apple', 'Banana', 'Cherry']
```

### 6.5. Multi-level Sort with `lambda`

* Use `lambda` to return a tuple as the sorting key for multi-level sorting.
    

```python
words = ["apple", "dog", "app", "hi"]
words.sort(key=lambda x: (len(x), x))
print(words)  # Output: ['app', 'dog', 'hi', 'apple']
```

### 6.6. Custom Sort with `cmp_to_key`

* When sorting requires comparing **pairs of items directly**, cmp\_to\_key lets you define a custom comparator.
    
* Use this when tuple-based lambda sorting isn’t flexible enough.
    

```python
from functools import cmp_to_key

words = ["3", "30", "34", "5", "9"]

# Custom comparator function
def compare(x, y):
    if x + y > y + x:
        return -1  # x should come before y
    elif x + y < y + x:
        return 1   # y should come before x
    else:
        return 0   # order doesn't matter

words.sort(key=cmp_to_key(compare))
print(words)  # Output: ['9', '5', '34', '3', '30']
```

---

## 🧰 7. Useful Essential Libraries

### 7.1. math

* Great for number-related operations like GCD, factorial, sqrt.
    

```python
import math

print(math.gcd(12, 18))  # Output: 6
print(math.sqrt(25))     # Output: 5.0
print(math.lcm(4, 6))    # Output: 12 (Python 3.9+ only)
```

### 7.2. itertools

* Handy for generating permutations, combinations, and more.
    

```python
from itertools import permutations, combinations

print(list(permutations([1, 2, 3], 2)))
# Output: [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]

print(list(combinations([1, 2, 3], 2)))
# Output: [(1, 2), (1, 3), (2, 3)]
```

### 7.3. bisect

* Binary search helper for finding insertion points in sorted lists.
    

```python
import bisect

arr = [1, 2, 4, 4, 5]
print(bisect.bisect_left(arr, 4))   # Output: 2
print(bisect.bisect_right(arr, 4))  # Output: 4
```

💡 Tip

* `bisect_left` finds the first position (leftmost) where the value can be inserted.
    
* `bisect_right` gives the position just after the last occurrence.
    

---

## ✨ Summary

* Start with `input()` & `print()` and build up to efficient data handling.
    
* Use built-ins like `enumerate`, `zip`, `sorted`, and `Counter` wisely.
    
* String manipulation and sorting are key for parsing-heavy problems.
    

## 🔗 References

* [python lambda](https://www.w3schools.com/python/python_lambda.asp)
    
* [sorted() docs](https://docs.python.org/3/library/functions.html#sorted)
    
* [collections.Counter](https://docs.python.org/3/library/collections.html#collections.Counter)
    
* [heapq](https://docs.python.org/3/library/heapq.html)