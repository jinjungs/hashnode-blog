---
title: "Big-O You Actually Need to Know (Nothing More)"
datePublished: Mon Jun 16 2025 04:44:39 GMT+0000 (Coordinated Universal Time)
cuid: cmbym1c8d000402l9fmui1ex7
slug: coding-test-big-o
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750311035959/e73d239b-a73e-4aed-b1cd-d655711dda97.png
tags: time-complexity, big-o-notation, space-complexity, coding-test, logn

---

If you’re preparing for coding interviews, you’ve probably heard about Big-O notation more times than you can count. But let’s be honest—most of us don’t need to memorize every complexity out there. What you *do* need is a solid grasp of the core concepts that actually show up in interviews.

This post is a quick, no-fluff guide to Big-O—just the essentials. Whether you’re revising the night before your coding test or brushing up during a lunch break, this is for you.

## 📚 1. Big-O Notation Overview

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750048006778/e97841c1-e4ab-4615-9d7f-c7f0db09c3c6.png align="center")

---

## ⏱️ 2. Time Complexity Deep Dive

### 2.1. Rule of Thumb

* **100 million (10⁸) operations ≈ 1 second**
    
* If time limit = 1 sec, your solution should generally stay below that range.
    

### 2.2. Acceptable Complexity Based on N

| Input Size N | Acceptable Complexity | Est. Ops | Description |
| --- | --- | --- | --- |
| N ≤ 10 | O(N!), O(2^N) | &lt; 10⁶ | Brute force is acceptable |
| N ≤ 100 | O(N³) | ~10⁶ | Triple nested loops OK |
| N ≤ 1,000 | O(N² log N), O(N²) | ~10⁷ | Double loop with sort works |
| N ≤ 10,000 | O(N log N), O(N√N) | ~10⁷–10⁸ | Sorting, two pointers, segment tree |
| N ≤ 100,000 | O(N log N) | ~10⁷–10⁸ | Priority queue, hash maps |
| N ≤ 1,000,000 | O(N) | ~10⁶–10⁷ | Simple loops, hash, sliding window |

### 2.3. Algorithm Complexity Cheat Sheet

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750048066442/25d89f5c-2324-4475-a00f-65b541dbeaec.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750048073148/93354e70-522a-4dde-be45-725c0f731296.png align="center")

### 2.4. Practical Tips to Avoid TLE

* Use `sys.stdin.readline()` instead of `input()` in Python
    
* Avoid unnecessary nested loops
    
* Use `set` / `dict` for fast lookup
    
* Use `bisect` for binary search after sorting
    
* Apply memoization in DP
    
* Use sliding window to reduce redundant calculations
    

---

## 📉 3. log N Complexity

> Logarithmic time complexity can be confusing at first, but once you understand that it means ***<mark>cutting the problem size in half at each step</mark>***, it starts to make a lot more sense.

### 3.1. Examples

| Scenario | Time Complexity | Reason |
| --- | --- | --- |
| Binary Search (sorted array) | O(log N) | Halves search space |
| Heap operations (insert/delete) | O(log N) | Tree depth traversal |
| Balanced binary tree traversal | O(log N) | Depth from root to leaf |
| Segment/Fenwick Tree | O(log N) | Range update/query |
| Union-Find (with path compression) | O(α(N)) ≈ O(log N) | Almost constant time |

### 3.2. Estimating log₂(N)

* N = 16 → x = 4 → log₂16 = 4
    
* log₂(N) ≈ **number of times to divide N by 2 to reach 1**
    

> You can estimate log base 2 intuitively as:
> 
> * log₂(1,000) ≈ 10
>     
> * log₂(1,024) = 10
>     
> * log₂(1,000,000) ≈ 20
>     

---

## 💾 4. Space Complexity

Why do we care about space complexity? Many problems give memory limits (e.g. 80MB). You need to estimate usage based on data size and type.

### 4.1. Rough C++ Memory Usage Guide

```cpp
int a[20_000_000];   // ~80 MB
int a[2_000_000];    // ~8 MB
char a[20_000_000];  // ~20 MB
double a[20_000_000];// ~160 MB
```

So if a problem allows 80MB of memory, you can calculate whether your array fits based on this rule of thumb.

---

## ✨ Summary

* O(log N) appears in binary search, heaps, trees, divide-and-conquer
    
* log₂(N) = number of divisions by 2 until 1
    
* For N = 1,000,000, log₂(N) ≈ 20 is enough to remember
    

## 🔗 References

* [https://www.bigocheatsheet.com/](https://www.bigocheatsheet.com/)