---
title: "Understanding Shortest Path Algorithms: Dijkstra, Bellman-Ford, and Floyd-Warshall Explained"
seoTitle: "Shortest Path Algorithms Overview"
seoDescription: "Study Dijkstra, Bellman-Ford, and Floyd-Warshall algorithms for shortest path solutions in graphs with different constraints and applications"
datePublished: Thu Aug 14 2025 23:30:24 GMT+0000 (Coordinated Universal Time)
cuid: cmec18bel000702gs2yitavxk
slug: coding-test-shortest-path-algorithms
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755551337200/c6e8e60f-dbef-4544-86bf-17ab24fc228a.png
tags: dijkstra, bellman-ford, bellmanford, shortestpathalgorithms, floyd-warshall

---

## 1\. Introduction

Shortest path algorithms are fundamental in graph theory and computer science, with wide applications ranging from GPS navigation systems to network routing protocols. This article covers three major algorithms:

* **Dijkstra's Algorithm**: Efficient for single-source shortest paths in graphs with non-negative edge weights.
    
* **Bellman-Ford Algorithm**: Handles graphs with negative edge weights and detects negative weight cycles.
    
* **Floyd-Warshall Algorithm**: Computes shortest paths between all pairs of vertices.
    

---

## 2\. Dijkstra's Algorithm

### 2.1 Overview

Dijkstra's algorithm finds the shortest path from a single source to all other vertices in a weighted graph with **non-negative** edge weights.

### 2.2 Steps

1. Initialize distances from the source to all vertices as infinity, except the source (0).
    
2. Use a priority queue to pick the vertex with the smallest tentative distance.
    
3. Update distances to neighboring vertices if a shorter path is found.
    
4. Repeat until all vertices are processed.
    

### 2.3 Time Complexity

* Using a binary heap: **O((V + E) log V)**
    

### 2.4 Python Implementation

```python
from heapq import heappush, heappop
from math import inf
from typing import List, Tuple

def dijkstra(n: int, edges: List[Tuple[int, int, int]], src: int, undirected: bool = False) -> List[float]:
    g = [[] for _ in range(n)]
    for u, v, w in edges:
        g[u].append((v, w))
        if undirected:
            g[v].append((u, w))

    dist = [inf] * n
    dist[src] = 0
    pq = [(0, src)]

    while pq:
        d, u = heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heappush(pq, (nd, v))
    return dist
```

### 2.5 Example (Input & Output)

```python
n = 4
edges = [(0,1,2), (1,2,1), (0,3,4), (1,3,7)]
print(dijkstra(n, edges, src=0, undirected=True))
# Output: [0, 2, 3, 4]
```

---

## 3\. Bellman-Ford Algorithm

### 3.1 Overview

Bellman-Ford can handle **negative edge weights** and also detect **negative weight cycles**.

### 3.2 Steps

1. Initialize distances from the source to all vertices as infinity, except the source (0).
    
2. Relax all edges **V-1** times (V = number of vertices).
    
3. If a shorter path is found after V-1 iterations, a negative cycle exists.
    

### 3.3 Time Complexity

* **O(V \* E)**
    

### 3.4 Python Implementation

```python
from math import inf
from typing import List, Tuple

def bellman_ford(n: int, edges: List[Tuple[int, int, int]], src: int):
    dist = [inf] * n
    dist[src] = 0

    for _ in range(n - 1):
        updated = False
        for u, v, w in edges:
            if dist[u] != inf and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                updated = True
        if not updated:
            break

    has_neg_cycle = False
    for u, v, w in edges:
        if dist[u] != inf and dist[u] + w < dist[v]:
            has_neg_cycle = True
            break

    return dist, has_neg_cycle
```

### 3.5 Example (Input & Output)

```python
n = 5
edges = [
    (0, 1, 6), (0, 2, 7), (1, 2, 8), (1, 3, 5), (1, 4, -4),
    (2, 3, -3), (2, 4, 9), (3, 1, -2), (4, 3, 7), (3, 4, 2)
]
print(bellman_ford(n, edges, src=0))
# Output: ([0, 2, 7, 4, -2], False)
```

---

## 4\. Floyd-Warshall Algorithm

### 4.1 Overview

Floyd-Warshall computes shortest paths between **all pairs** of vertices. Works with negative weights but no negative cycles.

### 4.2 Steps

1. Initialize a distance matrix with direct edge weights.
    
2. For each vertex k, update all pairs (i, j) with:
    
    ```python
    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    ```
    
3. Repeat for all vertices k.
    

### 4.3 Time Complexity

* **O(V³)**
    

### 4.4 Python Implementation

```python
from math import inf
from typing import List

def floyd_warshall(adj: List[List[float]]) -> List[List[float]]:
    n = len(adj)
    dist = [row[:] for row in adj]
    for k in range(n):
        for i in range(n):
            dik = dist[i][k]
            if dik == float('inf'):
                continue
            for j in range(n):
                djk = dist[k][j]
                if djk == float('inf'):
                    continue
                nd = dik + djk
                if nd < dist[i][j]:
                    dist[i][j] = nd
    return dist
```

### 4.5 Example (Input & Output)

```python
INF = float('inf')
adj = [
    [0,   5,   INF, 10],
    [INF, 0,   3,   INF],
    [INF, INF, 0,   1],
    [INF, INF, INF, 0]
]
for row in floyd_warshall(adj):
    print(row)
# Output:
# [0, 5, 8, 9]
# [inf, 0, 3, 4]
# [inf, inf, 0, 1]
# [inf, inf, inf, 0]
```

---

## 5\. Comparison

| Algorithm | Negative Weights | Detect Negative Cycle | All-Pairs | Time Complexity |
| --- | --- | --- | --- | --- |
| Dijkstra | No | No | No | O((V+E) log V) |
| Bellman-Ford | Yes | Yes | No | O(V \* E) |
| Floyd-Warshall | Yes (No cycles) | No | Yes | O(V³) |

---

## ✨ Conclusion

Dijkstra, Bellman-Ford, and Floyd-Warshall are essential algorithms for solving shortest path problems, each with unique strengths:

* **Dijkstra**: Fastest for non-negative weights.
    
* **Bellman-Ford**: Works with negative weights and detects cycles.
    
* **Floyd-Warshall**: Best for all-pairs shortest paths.
    

Choosing the right algorithm depends on graph characteristics such as edge weights, density, and whether all-pairs or single-source shortest paths are required.

## 🔗 References

* [Dijkstra's algorithm in 3 minutes](https://www.youtube.com/watch?v=_lHSawdgXpI)
    
* [Bellman-Ford in 4 minutes — Theory](https://www.youtube.com/watch?v=9PHkk0UavIM)
    
* [Bellman-Ford in 5 minutes — Step by step example](https://www.youtube.com/watch?v=obWXjtg0L64)
    
* [Floyd–Warshall algorithm in 4 minutes](https://www.youtube.com/watch?v=4OQeCuLYj-4)