---
title: "Understanding Python's DFS and BFS Techniques"
seoTitle: "Python Search Techniques: DFS vs BFS"
seoDescription: "Master BFS/DFS techniques for grid and graph problems to excel in coding interviews and contests"
datePublished: Fri Aug 08 2025 22:57:22 GMT+0000 (Coordinated Universal Time)
cuid: cme3fepwv000c02jp519e7s4m
slug: coding-test-dfs-bfs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754693425610/f887834f-6b51-47cd-abdf-9457977d3405.png
tags: algorithms, bfs, dfs, coding-test

---

This note summarizes essential techniques and patterns when solving BFS/DFS problems, especially in grid-based problems and graph traversal. Many of these ideas come up repeatedly in coding interviews or contest problems.

---

## 🚩 General DFS Rules

* Always use `dx = [0,1,0,-1]`, `dy = [1,0,-1,0]` for consistent **right, down, left, up** movement.
    
* Always check boundaries:
    

```python
if 0 <= nx < N and 0 <= ny < M:
```

* Use `visited[x][y]` (2D grid) or `visited[node]` (graph) to prevent infinite recursion.
    
* For backtracking, remember to revert visited status:
    

```python
visited[x][y] = True
DFS(...)
visited[x][y] = False  # backtrack
```

* If visited is a shared structure like a list, careful not to `copy()` it at every step unless necessary (can cause TLE).
    

---

## 🚩 BFS Patterns

* Use `deque` for BFS queue:
    

```python
from collections import deque
q = deque()
```

* When you want to process BFS **level by level**, use:
    

```python
while q:
    for _ in range(len(q)):
        x = q.popleft()
        ...
```

* Visited update should happen **when appending to the queue**, not when popping. This avoids revisiting same nodes multiple times.
    

✅ Good:

```python
visited[nx][ny] = True
q.append((nx, ny))
```

❌ Bad:

```python
q.append((nx, ny))
...
visited[nx][ny] = True
```

---

## 🚩 DFS in Grid to Collect Regions (e.g., Islands, Blocks)

```python
def dfs(x, y, group):
    visited[x][y] = True
    group.append((x, y))
    for i in range(4):
        nx, ny = x + dx[i], y + dy[i]
        if 0 <= nx < N and 0 <= ny < M and not visited[nx][ny] and grid[nx][ny] == 1:
            dfs(nx, ny, group)
```

---

## 🔄 Rotate & Normalize Techniques (for Shape Matching)

Used in problems like puzzle fitting, game boards, island matching, etc.

```python
def rotate(points):
    return [(y, -x) for (x, y) in points]

def move(points):
    min_x = min(p[0] for p in points)
    min_y = min(p[1] for p in points)
    return [(x - min_x, y - min_y) for (x, y) in points]
```

Use `rotate` multiple times to generate 4 directions.  
Use `move` to normalize to top-left origin for comparison.

---

## ✨ Final Tips

### ✅ Python Tip: Mutable types in recursion

In Python, integers are immutable. If you try to increment a global integer in DFS like below:

```python
time = 0

def dfs():
    time += 1  # ❌ Error: UnboundLocalError!
```

It will raise an `UnboundLocalError` because `time` is assumed as a local variable.

Instead, wrap it in a list:

```python
time = [0]  # mutable!

def dfs():
    time[0] += 1  # ✅ Works!
```

**Summary**

| Method | Description | Works? |
| --- | --- | --- |
| `time = 0; time += 1` | Treated as local variable → causes error | ❌ |
| `time = [0]; time[0] +=1` | List is mutable → shared and updated in recursion | ✅ |