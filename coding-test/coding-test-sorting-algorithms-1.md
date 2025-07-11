---
title: "Key Sorting Algorithms Every Coder Should Know - 1"
seoTitle: "Essential Sorting Algorithms for Coders"
seoDescription: "Learn Bubble, Selection, and Insertion Sort algorithms. Understand their key ideas, complexities, and practical applications for efficient coding"
datePublished: Wed Jun 18 2025 07:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cm6i1m9xr000008jo5z56g0x3
slug: coding-test-sorting-algorithms-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750311554314/7165d9e2-65f5-42cd-9083-44594792a5c8.png
tags: algorithms, coding, insertion-sort, sorting, bubble-sort, selection-sort, coding-test

---

## 🫧 1. Bubble Sort

> Compare adjacent pairs of elements in an array and swap them if they are in the wrong order, <mark>gradually moving larger elements to the end of the array.</mark>

### 1.1. Time Complexity

* **Average / Worst case**: O(n^2)
    
* **Best case**: O(n) (already sorted)
    

### 1.2. Space Complexity

* **O(1)**
    

### 1.3. Code

```python
def bubble_sort(l: List[int]) -> List[int]:
    n = len(l)
    for i in range(n-1):
        for j in range(n-i-1):
            if l[j] > l[j+1]:
                l[j], l[j+1] = l[j+1], l[j]
```

---

## 📍2. Selection Sort

> <mark>Find the minimum element from the unsorted portion</mark> of the array and swap it with the first unsorted element.

### 2.1. Time Complexity

* **All cases**: O(n^2)
    

### 2.2. Space Complexity

* **O(1)**
    

### 2.3. Code

```python
def selection_sort(l: List[int]) -> List[int]:
    n = len(l)
    for i in range(n):
        min_index = i
        for j in range(i+1, n):
            if l[min_index] > l[j]:
                min_index = j
        if m != i: # prevent unnecessary swap
            l[i], l[m] = l[m], l[i]
```

---

## 🪛 3. Insertion Sort

> Take each element from the unsorted portion and <mark>insert it into its correct position within the sorted portion</mark> of the array.

### 3.1. Time Complexity

* **Worst case**: O(n^2)
    
* Performs well for small or nearly sorted list.
    

### 3.2. Space Complexity

* **O(1)**
    

### 3.3. Code

```python
def insertion_sort(l: List[int]) -> List[int]:
    n = len(l)
    for i in range(1, n):
        j = i
        while l[j-1] > l[j] and j > 0:
            l[j-1], l[j] = l[j], l[j-1]
            j -= 1
```

---

## ✨ Summary

| Algorithm | Time Complexity (Worst) | Time Complexity (Best) | Space Complexity | Stable? |
| --- | --- | --- | --- | --- |
| Bubble Sort | O(n²) | O(n) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n²) | O(n) | O(1) | Yes |

## **🔗** References

* [Sorting Algorithms Explained in 10 min](https://www.youtube.com/watch?v=Bor_CRWEIXo&t=44s)
    
* [Comparison among Bubble Sort, Selection Sort and Insertion Sort](https://www.geeksforgeeks.org/comparison-among-bubble-sort-selection-sort-and-insertion-sort/)