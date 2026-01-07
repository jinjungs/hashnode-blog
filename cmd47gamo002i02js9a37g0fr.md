---
title: "A Beginner's Guide to the Union-Find"
seoTitle: "Union-Find Basics for Beginners"
seoDescription: "Learn how Union-Find efficiently manages disjoint sets, connects elements, and optimizes operations like find and union in this beginner's guide"
datePublished: Tue Jul 15 2025 07:22:42 GMT+0000 (Coordinated Universal Time)
cuid: cmd47gamo002i02js9a37g0fr
slug: coding-test-union-find
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754693554366/a543cbcc-5753-4359-b9a3-1369cd1b3c8e.png
tags: algorithms, tree, coding-test, union-find

---

Union-Find, also known as **Disjoint Set Union (DSU)**, is a data structure that efficiently keeps track of a set of elements partitioned into disjoint (non-overlapping) subsets. It's particularly useful in problems involving **connectivity**, such as determining whether two elements are in the same group or detecting cycles in a graph.

---

## ✨ Summary

* **Purpose**: Track and manage disjoint sets
    
* **Core operations**:
    
    * `find(x)`: Find the representative (root) of the set containing `x`
        
    * `union(x, y)`: Merge the sets that contain `x` and `y`
        
* **Optimizations**: Path compression and union by rank/size
    
* **Time Complexity**: Nearly O(1) per operation with optimizations
    

---

## 🧱 Basic Implementation

```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])  # Path compression
    return parent[x]

def union(x, y):
    rootX = find(x)
    rootY = find(y)
    if rootX != rootY:
        parent[rootY] = rootX  # Union
```

Initialization:

```python
parent = [i for i in range(n)]
```

Each element is its own parent initially.

---

## ⚡ Optimizations

### Path Compression

* During `find(x)`, we recursively set the parent of each node to its root.
    
* This flattens the tree, making future operations faster.
    

### Union by Rank or Size (Optional)

* Attach the smaller tree to the root of the larger tree.
    
* Prevents tree from becoming tall.
    

```python
rank = [1] * n

def union(x, y):
    rootX = find(x)
    rootY = find(y)
    if rootX != rootY:
        if rank[rootX] < rank[rootY]:
            parent[rootX] = rootY
        else:
            parent[rootY] = rootX
            if rank[rootX] == rank[rootY]:
                rank[rootX] += 1
```

---

## 🧠 Use Cases

| Problem Type | Example Problems |
| --- | --- |
| Connectivity / Components | Number of Provinces (LC 547) |
| Cycle Detection | Redundant Connection (LC 684) |
| Grouping / Merging | Accounts Merge (LC 721) |
| String/Custom Grouping | Sentence Similarity II (LC 737) |
| Weighted Union-Find | Evaluate Division (LC 399, advanced) |

---

## 📌 Example: Number of Provinces (LC 547)

Given an adjacency matrix `isConnected`, count how many connected components (provinces) exist.

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        parent = [i for i in range(n)]

        def find(x):
            if x != parent[x]:
                parent[x] = find(parent[x])  # Path compression
            return parent[x]

        def union(x, y):
            rootX = find(x)
            rootY = find(y)
            if rootX != rootY:
                parent[rootY] = rootX

        for i in range(n):
            for j in range(i + 1, n):
                if isConnected[i][j]:
                    union(i, j)

        return len(set(find(i) for i in range(n)))
```

---

## 💡 Tip

* The problem involves **grouping**, **connected components**, or **merging sets**.
    
* You need to answer: "Are these two elements in the same group?"
    

---

## ✨ Conclusion

Union-Find is a must-know data structure for any coding interview. With its powerful optimizations and simple API (`find` and `union`), it can solve many seemingly complex problems involving relationships, grouping, and connectivity.

## 🔗 References

* [LeetCode 547 - Number of Provinces](https://leetcode.com/problems/number-of-provinces)
    
* [LeetCode 684 - Redundant Connection](https://leetcode.com/problems/redundant-connection)
    
* [LeetCode 721 - Accounts Merge](https://leetcode.com/problems/accounts-merge)