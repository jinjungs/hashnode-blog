---
title: "Introduction to Stacks and Queues: From Basics to Advanced"
seoTitle: "Stacks vs Queues: A Comprehensive Guide"
seoDescription: "Explore the basics and advanced techniques of stacks and queues, including their use cases and variations like the monotonic queue"
datePublished: Mon Aug 18 2025 21:04:41 GMT+0000 (Coordinated Universal Time)
cuid: cmehlsc7o000002js2izefehu
slug: coding-test-stack-queue
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755551032776/ffbd8bac-205b-4355-896d-8947829952cc.png
tags: algorithms, queue, stack, monotonic-stack

---

## 1\. Introduction

Stacks and queues are two of the most fundamental data structures in computer science. They serve as building blocks for more complex algorithms and are frequently used in coding interviews. In this post, we’ll explore how they work, their use cases, and some advanced techniques like the **Monotonic Queue**.

---

## 2\. Stack

### 2.1. What is a Stack?

A **stack** is a data structure that follows the **LIFO (Last In, First Out)** principle. The most recently added element is the first one to be removed.

* **Push**: Add an element to the top of the stack
    
* **Pop**: Remove the element from the top of the stack
    
* **Peek/Top**: View the element at the top without removing it
    

### 2.2. Common Use Cases

* Function call stack (tracking function execution)
    
* Undo/redo operations
    
* Balanced parentheses or syntax checking
    
* Depth-first search (DFS)
    

### 2.3. Stack Example in Python

```python
stack = []
stack.append(1)   # Push
stack.append(2)
stack.append(3)
print(stack.pop())  # Pop → 3
print(stack[-1])    # Peek → 2
```

### 2.4. Advanced Stack Techniques

* **Monotonic Stack**: Maintains elements in either increasing or decreasing order. Useful for problems like *Next Greater Element* or *Largest Rectangle in Histogram*.
    

Example:

```python
def next_greater_elements(nums):
    res = [-1] * len(nums)
    stack = []  # will store indices
    for i, num in enumerate(nums):
        while stack and nums[stack[-1]] < num:
            res[stack.pop()] = num
        stack.append(i)
    return res
```

---

## 3\. Queue

### 3.1. What is a Queue?

A **queue** is a data structure that follows the **FIFO (First In, First Out)** principle. The first element added is the first one removed.

* **Enqueue**: Add an element to the back
    
* **Dequeue**: Remove an element from the front
    

### 3.2. Common Use Cases

* Task scheduling
    
* BFS (Breadth-first search)
    
* Caching (e.g., LRU Cache implementation)
    
* Rate limiting in systems
    

### 3.3. Queue Example in Python

```python
from collections import deque

queue = deque()
queue.append(1)    # Enqueue
queue.append(2)
queue.append(3)
print(queue.popleft())  # Dequeue → 1
print(queue[0])         # Peek → 2
```

### 3.4. Monotonic Queue

A **Monotonic Queue** is a powerful data structure that maintains elements in sorted order while supporting efficient insertion and deletion. It is commonly used for **sliding window maximum/minimum problems**.

Example:

```python
from collections import deque

def sliding_window_max(nums, k):
    dq = deque()
    res = []
    for i, num in enumerate(nums):
        # Remove smaller values from the back
        while dq and nums[dq[-1]] < num:
            dq.pop()
        dq.append(i)

        # Remove indices out of the current window
        if dq[0] <= i - k:
            dq.popleft()

        # Store result when window is valid
        if i >= k - 1:
            res.append(nums[dq[0]])
    return res
```

Time Complexity: **O(n)** (each element is added/removed at most once)

### 3.5. Other Queue Techniques

* **Priority Queue (Heap)**: For efficiently retrieving min/max elements.
    
* **Deque (Double-Ended Queue)**: Supports fast operations on both ends, often used in sliding window problems.
    

---

## 4\. Essential Techniques with Stack & Queue

* **Two Stacks for Queue Implementation**: Simulate a queue using two stacks (useful in interview questions).
    
* **Queue with Stacks** and **Stack with Queues**: Classic exercises to test data structure knowledge.
    
* **BFS + Queue** and **DFS + Stack**: Core building blocks for graph/tree problems.
    

---

## ✨ Conclusion

Stacks and queues may look simple, but mastering their variations and techniques (like monotonic structures, two-stack simulation, and sliding windows) is crucial for solving complex algorithm problems efficiently. They often appear in real-world applications and are a common theme in coding interviews.

---

## 🔗 References

* [LeetCode Explore: Queue & Stack](https://leetcode.com/explore/learn/card/queue-stack/)
    
* [Sliding Window Maximum - LeetCode](https://leetcode.com/problems/sliding-window-maximum/)
    
* [Trapping Rain Water - LeetCode](https://leetcode.com/problems/trapping-rain-water/)