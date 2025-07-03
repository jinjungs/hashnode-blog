---
title: "A Beginner's Guide to the Two Pointers"
seoTitle: "Mastering Two Pointers: A Beginner's Guide"
seoDescription: "Learn the Two Pointers technique for efficient problem-solving in sorted arrays, sliding windows, and more with practical examples and tips"
datePublished: Thu Jul 03 2025 23:24:06 GMT+0000 (Coordinated Universal Time)
cuid: cmco0ig0j000002lbfogi4fdl
slug: a-beginners-guide-to-the-two-pointers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751584835202/5f655c0d-2368-4e94-8eb7-f7d2478fc065.png
tags: python, coding, two-pointers, coding-test

---

## 📌 1. What is Two Pointers?

The **Two Pointers** technique uses two variables (usually indices) to traverse a list or string from different directions (e.g., start and end).  
By controlling the movement of these pointers, you can solve problems **more efficiently** than brute force.

### Typical Patterns:

* **Opposite Direction (start ↔ end)**: Used for sorted arrays, like finding pairs with a certain sum.
    
* **Same Direction (left → right)**: Used for sliding window problems, subarrays, or skipping duplicates.
    

---

## 💡 2. When to Use Two Pointers?

Use it when:

* You need to find a pair or group of elements satisfying a condition (e.g. target sum, duplicates).
    
* You want to **avoid O(n²)** brute force.
    
* The array is **sorted** (very common scenario).
    
* You're working with **contiguous subarrays or substrings**.
    

---

## 🧠 3. Example Problems & Patterns

### 3.1. Pair with Target Sum (Two Sum - sorted input)

```python
def two_sum(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        total = arr[left] + arr[right]
        if total == target:
            return [left, right]
        elif total < target:
            left += 1
        else:
            right -= 1
    return []
```

**Input:**  
`arr = [1, 2, 4, 7, 11, 15]`, `target = 15`  
**Output:**  
`[1, 5]` → 2 + 15 = 15

**Time Complexity:** O(n)

---

### 3.2. Remove Duplicates in-place (Same Direction)

```python
def remove_duplicates(nums):
    if not nums:
        return 0
    slow = 0
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    return slow + 1
```

**Input:**  
`[0,0,1,1,1,2,2,3,3,4]`  
**Output:**  
`[0,1,2,3,4]` (in-place)  
**Return:** `5` (number of unique elements)

---

### 3.3. Valid Palindrome (Opposite Direction)

```python
def is_palindrome(s):
    s = ''.join(c.lower() for c in s if c.isalnum())
    left, right = 0, len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True
```

**Input:**  
`"A man, a plan, a canal: Panama"`  
**Output:**  
`True`

---

## 💡 Tip

* Sort the array first if the problem doesn’t guarantee sorted input.
    
* `left` and `right` naming is intuitive for opposite directions; use `slow` and `fast` for same-direction logic.
    
* Combine with **binary search** or **sliding window** when needed.
    
* Works best on problems involving **ordered structures** or **linear traversal**.
    

---

## ✨ Summary

The **Two Pointers** technique is a powerful and efficient way to solve problems involving arrays or strings, especially when searching for a condition or relationship between elements. Instead of using nested loops, you use **two indexes (pointers)** to iterate through data and reduce time complexity.

## 🔗 References

* [Two Pointers in 7 minutes | LeetCode Pattern](https://www.youtube.com/watch?v=QzZ7nmouLTI)
    
* [Fast and Slow Pointers in 6 Minutes | LeetCode Pattern](https://www.youtube.com/watch?v=b139yf7Ik-E)
    
* [Leetcode - Two Pointers](https://leetcode.com/problem-list/two-pointers/)