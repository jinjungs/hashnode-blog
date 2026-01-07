---
title: "A Beginner's Guide to Binary Search Algorithm"
seoTitle: "Binary Search Algorithm: A Beginner's Guide"
seoDescription: "Learn binary search basics, time complexity, variations, and implementations for efficient coding and interviews"
datePublished: Thu Aug 21 2025 06:04:37 GMT+0000 (Coordinated Universal Time)
cuid: cmekzydyu000d02l4676jhfqf
slug: algorithms-binary-search
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755756212224/6c4b1a2f-8d5e-4dba-940b-53a19f888c24.png
tags: algorithms, coding-interview, binary-search, binary-search-algorithm, coding-test

---

## 1\. Introduction to Binary Search

Binary Search is one of the most fundamental algorithms in computer science. It is a highly efficient method to find the position of a target value within a sorted array. Instead of scanning the entire array linearly, binary search repeatedly divides the search space in half, which makes it run in logarithmic time complexity.

---

## 2\. How Binary Search Works

### 2.1 Key Idea

* Start with two pointers: left at the beginning and right at the end of the array.
    
* Repeatedly calculate the midpoint between left and right.
    
* Compare the midpoint value with the target:
    
    * If equal → target found.
        
    * If target &lt; mid → search the left half.
        
    * If target &gt; mid → search the right half.
        
* Continue until the range becomes empty.
    

### 2.2 Time and Space Complexity

* Time Complexity: O(log n)
    
* Space Complexity: O(1) (iterative) or O(log n) (recursive, due to call stack)
    

### 2.3 Overflow Issue in Midpoint Calculation

In most programming languages (like C++, Java, etc.), directly using (left + right) / 2 can cause integer overflow if left and right are large values.

Python handles big integers automatically, so overflow is not an issue. However, for interview preparation or cross-language practice, it is better to use a safer formula:

```java
// Java Example (safe midpoint calculation)
int mid = left + (right - left) / 2;
```

This approach prevents overflow by subtracting before adding.

---

## 3\. Implementation in Python

### 3.1 Iterative Version

```python
# Iterative Binary Search
# Returns index of target if found, otherwise -1
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Example
nums = [1, 3, 5, 7, 9, 11]
target = 7
print(binary_search(nums, target))  # Output: 3
```

### 3.2 Recursive Version

```python
# Recursive Binary Search
def binary_search_recursive(nums, target, left, right):
    if left > right:
        return -1
    mid = (left + right) // 2
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        return binary_search_recursive(nums, target, mid + 1, right)
    else:
        return binary_search_recursive(nums, target, left, mid - 1)

# Example
nums = [1, 3, 5, 7, 9, 11]
print(binary_search_recursive(nums, 5, 0, len(nums) - 1))  # Output: 2
```

---

## 4\. Variations of Binary Search

### 4.1 Lower Bound (First Occurrence)

Find the first index where the target appears.

```python
def lower_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left

# Example
nums = [1, 2, 2, 2, 3]
print(lower_bound(nums, 2))  # Output: 1
```

### 4.2 Upper Bound (Last Occurrence)

Find the first index where the value is greater than the target.

```python
def upper_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] <= target:
            left = mid + 1
        else:
            right = mid
    return left

# Example
nums = [1, 2, 2, 2, 3]
print(upper_bound(nums, 2))  # Output: 4
```

### 4.3 Search in Rotated Sorted Array

A common interview variation is searching in a rotated sorted array.

```python
def search_rotated(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        # Left half sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1

# Example
nums = [4,5,6,7,0,1,2]
print(search_rotated(nums, 0))  # Output: 4
```

---

## 5\. Common Pitfalls

* Forgetting to handle infinite loop when left and right don't move properly.
    
* Overflow risk in some languages (use `mid = left + (right - left) // 2`).
    
* Misunderstanding between lower\_bound and upper\_bound implementations.
    

---

## ✨ Conclusion

Binary Search is a powerful algorithm that serves as the foundation for many advanced techniques in computer science. Mastering not only the standard form but also its variations (like lower/upper bound and rotated array search) is crucial for coding interviews and efficient problem solving.

---

## 🔗 References

* [Binary Search - LeetCode Explore](https://leetcode.com/explore/learn/card/binary-search/)
    
* [MIT OpenCourseWare - Algorithms](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/)