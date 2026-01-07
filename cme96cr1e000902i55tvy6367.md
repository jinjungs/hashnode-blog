---
title: "A Guide to Minimum Spanning Trees: Kruskal's vs. Prim's Algorithm"
seoTitle: "Kruskal's vs. Prim's: Spanning Tree Algorithms"
seoDescription: "A comprehensive guide to compare Kruskal's and Prim's algorithms for finding Minimum Spanning Trees in graphs"
datePublished: Tue Aug 12 2025 23:30:31 GMT+0000 (Coordinated Universal Time)
cuid: cme96cr1e000902i55tvy6367
slug: algorithms-minimum-spanning-trees-kruskals
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755815332768/f3037e23-651d-40fc-8775-d1099729bd44.png
tags: algorithms, codinginterview, coding-test, kruskal, primsalgorithm, kruskalalgorithm

---

## Minimum Spanning Tree (MST)

A **Minimum Spanning Tree** of a connected, undirected, weighted graph is a subset of edges that:

1. Connects all vertices without cycles.
    
2. Has the minimum possible total edge weight.
    

---

## 1\. Kruskal’s Algorithm

**Idea:** Kruskal’s algorithm builds the MST by sorting all edges in non-decreasing order of weight and adding them one by one, skipping those that form a cycle.

**Steps:**

1. Sort edges by weight.
    
2. Initialize each vertex as its own set (Union–Find).
    
3. Add edges that connect different sets.
    
4. Stop when MST has `V - 1` edges.
    

**Time Complexity:** O(Elog⁡E)

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    def union(self, x, y):
        rx, ry = self.find(x), self.find(y)
        if rx != ry:
            self.parent[ry] = rx
            return True
        return False

def kruskal(n, edges):
    uf = UnionFind(n)
    mst = []
    edges.sort(key=lambda x: x[2])
    for u, v, w in edges:
        if uf.union(u, v):
            mst.append((u, v, w))
        if len(mst) == n - 1:
            break
    return mst
```

---

## 2\. Prim’s Algorithm

**Idea:** Prim’s algorithm grows the MST from a starting vertex, always adding the smallest weight edge that connects the tree to a new vertex.

**Steps:**

1. Pick a starting vertex.
    
2. Use a priority queue to store edges to unvisited vertices.
    
3. Repeatedly pick the smallest edge and add it if it connects to a new vertex.
    
4. Stop when all vertices are in the tree.
    

**Time Complexity:** O(Elog⁡V) with a binary heap

```python
import heapq
from collections import defaultdict

def prim(n, edges):
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((w, v))
        graph[v].append((w, u))

    visited = set()
    mst = []
    pq = [(0, 0, -1)]  # (weight, current, parent)

    while pq and len(visited) < n:
        w, u, parent = heapq.heappop(pq)
        if u in visited:
            continue
        visited.add(u)
        if parent != -1:
            mst.append((parent, u, w))
        for weight, v in graph[u]:
            if v not in visited:
                heapq.heappush(pq, (weight, v, u))
    return mst
```

---

## 3\. Kruskal vs Prim

| Feature | Kruskal | Prim |
| --- | --- | --- |
| Approach | Edge-based | Vertex-based |
| Data Structure | Union–Find | Min-Heap + Adjacency List |
| Best for | Sparse graphs | Dense graphs |
| Complexity | O(Elog⁡E) | O(Elog⁡V) |
| Start Point | Not needed | Needs a starting vertex |

---

## ✨ Conclusion

Both Kruskal’s and Prim’s algorithms solve the MST problem efficiently:

* **Kruskal** is often better for sparse graphs.
    
* **Prim** works well for dense graphs with an efficient priority queue.
    

## 🔗 References

* [Kruskal's algorithm in 2 minutes](https://www.youtube.com/watch?v=71UQH7Pr9kU)
    
* [Prim's algorithm in 2 minutes](https://www.youtube.com/watch?v=cplfcGZmX7I)