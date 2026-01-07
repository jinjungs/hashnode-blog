---
title: "Key Sorting Algorithms Every Coder Should Know - 2"
seoTitle: "Essential Sorting Algorithms for Coders"
seoDescription: "Learn Quick, Merge, Heap, and Counting Sort, their time/space complexities, and stability"
datePublished: Wed Jun 25 2025 06:18:31 GMT+0000 (Coordinated Universal Time)
cuid: cmcbkcpnn000c02ju4yedf274
slug: algorithms-sorting-algorithms-2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750829549032/f3b5b6f5-fdb9-4ed6-a4a9-daebe7f7f614.png
tags: algorithms, coding, sorting, quick-sort, merge-sort, sorting-algorithms, heap-sort, counting-sort

---

## 🌀 1. Quick Sort

> Divide-and-conquer: <mark>pick a&nbsp;</mark> **<mark>pivot</mark>**, partition the array into elements ≤ pivot (left) and &gt; pivot (right), then <mark>recursively sort the two halves.</mark> Average-case efficiency comes from balanced partitions.

### 1.1. Time Complexity

* **Average / Best case:** O(n log n)
    
* **Worst case:** O(n²) (already-sorted or all equal elements with naïve pivot)
    

### 1.2. Space Complexity

* **O(log n)** extra (call stack) if in-place partitioning is used.
    

### 1.3. Code

```python
def quick_sort(arr):
    if len(arr) <= 1:                   # base case
        return arr
    pivot = arr[0]                      # simple pivot choice
    tail  = arr[1:]

    left  = [x for x in tail if x <= pivot]
    right = [x for x in tail if x >  pivot]

    return quick_sort(left) + [pivot] + quick_sort(right)
```

---

## 🪄 2. Merge Sort

> <mark>Recursively divide the array in half</mark>, sort each half, then **merge** the two sorted halves back together in linear time.

### 2.1. Time Complexity

* **All cases:** O(n log n)
    

### 2.2. Space Complexity

* **O(n)** extra for the temporary merge buffer.
    

### 2.3. Code

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid   = len(arr) // 2
    left  = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    merged = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i]);  i += 1
        else:
            merged.append(right[j]); j += 1
    merged.extend(left[i:]); merged.extend(right[j:])
    return merged
```

---

## 🏧 3. Heap Sort

> <mark>Build a&nbsp;</mark> **<mark>max-heap</mark>** from the input array, then repeatedly swap the root (largest element) with the last unsorted element and heap-reduce the remaining array.

### 3.1. Time Complexity

* **All cases:** O(n log n)
    

### 3.2. Space Complexity

* **O(1)** extra if performed in place (array-backed heap).
    

### 3.3. Code

```python
import heapq

def heap_sort(arr):
    heapq.heapify(arr)              # Min-heap by default
    sorted_arr = [heapq.heappop(arr) for _ in range(len(arr))]
    return sorted_arr
```

---

## 📊 4. Counting Sort

> For a small integer range, count the occurrences of each value, then iterate through the counts to rebuild the sorted array. Works only for discrete, non-negative keys of limited range.

### 4.1. Time Complexity

* **O(n + k)** where *k* is the value range (max − min + 1).
    

### 4.2. Space Complexity

* **O(n + k)** for the count array and (optionally) the output array.
    

### 4.3. Code

```python
def counting_sort(arr):
    if not arr:
        return []
    max_val = max(arr)
    count = [0] * (max_val + 1)

    for num in arr:
        count[num] += 1

    sorted_arr = []
    for i in range(len(count)):
        sorted_arr.extend([i] * count[i])

    return sorted_arr
```

---

## ✨ Summary

| Algorithm | Time Complexity (Avg) | Time Complexity (Worst) | Space Complexity | Stable? |
| --- | --- | --- | --- | --- |
| Quick Sort | O(n log n) | O(n²) | O(log n) | No |
| Merge Sort | O(n log n) | O(n log n) | O(n) | Yes |
| Heap Sort | O(n log n) | O(n log n) | O(1) | No |
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | Yes |

## 🔗 References

* [Quick sort in 4 minutes](https://youtu.be/Hoixgm4-P4M?si=OsoGcFoAjGIttEC4)
    
* [Merge sort in 3 minutes](https://youtu.be/4VqmGXwpLqc?si=DJV6QU2ss7tqYw3r)
    
* [Heap sort in 4 minutes](https://youtu.be/2DmK_H7IdTo?si=YP971P9PDJLvSkcw)
    
* [Learn Counting Sort Algorithm in LESS THAN 6 MINUTES!](https://youtu.be/OKd534EWcdk?si=-ZAiq4o4qJCuOVkP)