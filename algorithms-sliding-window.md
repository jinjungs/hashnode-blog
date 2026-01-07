---
title: "A Beginner's Guide to the Sliding Window"
seoTitle: "Sliding Window: Beginner's Guide Explained"
seoDescription: "Learn the efficient sliding window technique to solve array or string problems by reducing time complexity through strategic window adjustments"
datePublished: Mon Jul 07 2025 16:00:16 GMT+0000 (Coordinated Universal Time)
cuid: cmctaf2fy000n02ktcpewfdam
slug: algorithms-sliding-window
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751779618185/6afabc51-e807-41f5-b680-6c5e93b770d4.png
tags: algorithms, algorithm, coding-interview, sliding-window, two-pointers, coding-test

---

Sliding Window is one of the most efficient and elegant techniques used in solving problems involving arrays or strings. Especially useful when you're dealing with subarrays or substrings, it allows you to reduce time complexity by avoiding unnecessary recalculations.

---

## 1\. What is Sliding Window?

Sliding Window is a technique that uses two pointers to create a "window" that slides over the data to examine a subset at a time.

* It usually involves a **left** and **right** pointer.
    
* The window expands or shrinks based on problem constraints.
    
* It helps maintain useful information (like sum, frequency, or set of characters) in the current window efficiently.
    

---

## 2\. When to Use Sliding Window

Use Sliding Window when:

* You're working with **contiguous** subarrays or substrings.
    
* You need to find the **maximum/minimum/average** over a range.
    
* The problem mentions a **window size** or asks for the **longest/shortest substring/subarray** meeting certain conditions.
    

---

## 3\. Types of Sliding Window

### 3.1. Fixed-size Window

You slide a window of constant size `k` across the array.

**Example: Maximum sum of subarray of size k**

```python
def max_sum_subarray(nums, k):
    window_sum = sum(nums[:k])
    max_sum = window_sum
    
    for i in range(k, len(nums)):
        window_sum += nums[i] - nums[i - k]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

**Input:** nums = \[2, 1, 5, 1, 3, 2\], k = 3  
**Output:** 9 (subarray \[5,1,3\])

### 3.2. Variable-size Window

The window expands and shrinks based on conditions until it satisfies constraints.

**Example: Minimum size subarray sum &gt;= target**

```python
def min_subarray_len(target, nums):
    left = 0
    total = 0
    min_len = float('inf')

    for right in range(len(nums)):
        total += nums[right]
        while total >= target:
            min_len = min(min_len, right - left + 1)
            total -= nums[left]
            left += 1

    return min_len if min_len != float('inf') else 0
```

**Input:** nums = \[2,3,1,2,4,3\], target = 7  
**Output:** 2 (subarray \[4,3\])

---

## 4\. Sliding Window vs Two Pointers

| Feature | Sliding Window | Two Pointers |
| --- | --- | --- |
| **Focus** | Maintains a range (window) and updates its internal state | Compares values at two positions to find a relationship |
| **Common Use Cases** | Contiguous subarrays/substrings, frequency counting | Sorted arrays, pairs that meet a condition (e.g., two sum) |
| **State Management** | Actively tracks info inside the window (e.g. sum, frequency) | Usually does not store window state, only uses positions |
| **Window Behavior** | Window expands/shrinks based on constraints | Pointers move independently depending on values |
| **Examples** | Longest substring without repeating characters, min-size subarray sum | Two-sum in sorted array, merging arrays |

💡 In fact, Sliding Window is a specialized form of Two Pointers — it's used when you're dealing with **contiguous elements** and often requires **tracking internal state**.

---

## 5\. 💡 Tips

* Use a **deque** if you need to maintain min/max within the window.
    
* For character frequency problems, consider using a **hashmap or array**.
    
* Always ask: Can I keep track of the current window state instead of recalculating it every time?
    

---

## ✨ Summary

Sliding Window is an essential tool in a developer's toolbox for solving many medium-level interview problems efficiently. Once you understand how to maintain and adjust a window, it becomes a go-to pattern for problems involving subarrays or substrings.

---

## 🔗 References

* [Sliding Window in 7 minutes | LeetCode Pattern](https://youtu.be/y2d0VHdvfdc?si=Rw68HjF1Mbe_mNlt)
    
* [Sliding Window Technique](https://youtu.be/dOonV4byDEg?si=g8r4F2kuL8J64YcJ)