---
title: "A Beginner's Guide to Linked List"
seoTitle: "Understanding Linked Lists: A Beginner's Guide"
seoDescription: "Learn the basics of linked lists, their types, operations, advantages, and applications, including cycle detection and LRU cache"
datePublished: Thu Aug 21 2025 21:22:01 GMT+0000 (Coordinated Universal Time)
cuid: cmelwq63v000002lbgm3n3w2v
slug: algorithms-linked-list
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755811235330/5f268548-999d-40a8-95e1-ac37fbcf3bac.png
tags: algorithms, data-structures, linked-list, linkedlists, coding-test

---

## 1\. Introduction to Linked Lists

A Linked List is a linear data structure where elements, called nodes, are connected using pointers. Unlike arrays, linked lists do not require contiguous memory, making insertion and deletion operations efficient.

Each node typically contains:

* **Data**: The value stored in the node.
    
* **Pointer (Next)**: A reference to the next node in the sequence.
    

---

## 2\. Types of Linked Lists

### 2.1 Singly Linked List

Each node points only to the next node.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        curr = self.head
        while curr.next:
            curr = curr.next
        curr.next = new_node

    def display(self):
        curr = self.head
        while curr:
            print(curr.data, end=" -> ")
            curr = curr.next
        print("None")

# Example
ll = LinkedList()
ll.append(1)
ll.append(2)
ll.append(3)
ll.display()  # Output: 1 -> 2 -> 3 -> None
```

### 2.2 Doubly Linked List

Each node has two pointers: one pointing to the next node and another pointing to the previous node.

```python
class DNode:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None
```

This allows traversal in both directions.

### 2.3 Circular Linked List

The last node points back to the head, forming a circle.

---

## 3\. Operations on Linked Lists

### 3.1 Insertion

* At the beginning
    
* At the end
    
* After a specific node
    

```python
def insert_at_beginning(self, data):
    new_node = Node(data)
    new_node.next = self.head
    self.head = new_node
```

### 3.2 Deletion

* Delete first node
    
* Delete by value
    

```python
def delete_by_value(self, key):
    curr = self.head
    if curr and curr.data == key:
        self.head = curr.next
        return
    prev = None
    while curr and curr.data != key:
        prev = curr
        curr = curr.next
    if curr:
        prev.next = curr.next
```

### 3.3 Searching

```python
def search(self, key):
    curr = self.head
    while curr:
        if curr.data == key:
            return True
        curr = curr.next
    return False
```

---

## 4\. Dummy Node Usage Example

In some problems, especially when modifying the list (like swapping pairs), a **dummy node** helps simplify edge cases.

Example: [LeetCode 24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

```python
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        prev = dummy

        while head and head.next:
            first = head
            second = head.next

            # switch
            prev.next = second
            first.next = second.next
            second.next = first

            # move pointer
            prev = first
            head = first.next

        return dummy.next
```

---

## 5\. Advanced Topics

### 5.1 Cycle Detection (Floyd’s Tortoise and Hare)

Cycle detection in a linked list is done using **fast and slow pointers**.

```python
def hasCycle(head: Optional[ListNode]) -> bool:
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### 5.2 LRU Cache Implementation

An **LRU (Least Recently Used) Cache** evicts the least recently used element when capacity is full. It is commonly implemented using a **HashMap + Doubly Linked List**.

* **HashMap**: Provides O(1) lookup for nodes.
    
* **Doubly Linked List**: Maintains usage order (most recent at head, least recent at tail).
    

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)
        return self.cache[key]

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
```

---

## 6\. Advantages and Disadvantages

### Advantages

* Dynamic memory allocation (no fixed size).
    
* Efficient insertions and deletions (O(1) when pointer is known).
    

### Disadvantages

* Random access is not allowed (O(n) traversal).
    
* Extra memory for storing pointers.
    
* Cache-unfriendly compared to arrays.
    

---

## 7\. Applications of Linked Lists

* Stacks and Queues
    
* Polynomial Representation
    
* Hash Chaining
    
* Dynamic Memory Allocation
    
* LRU Cache
    

---

## 8\. Common Pitfalls

* Forgetting to update pointers during insertions or deletions.
    
* Memory leaks due to lost references.
    
* Infinite loops when dealing with circular linked lists.
    

---

## 7\. Advanced Linked List Patterns

### 7.1 Swap Nodes in Pairs (Dummy Node Pattern) — LeetCode 24

Using a dummy (sentinel) node simplifies head manipulations and reduces edge cases.

```python
# LeetCode 24. Swap Nodes in Pairs
# Dummy (sentinel) node pattern to simplify pointer rewiring
from typing import Optional

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0, head)   # sentinel that points to head
        prev = dummy                # "prev" trails the pair being swapped

        while head and head.next:   # there is a pair to swap
            first = head
            second = head.next

            # rewire: prev -> second -> first -> rest
            prev.next = second
            first.next = second.next
            second.next = first

            # advance pointers for the next pair
            prev = first
            head = first.next

        return dummy.next
```

### 7.2 Cycle Detection (Floyd’s Tortoise & Hare) — LeetCode 141/142

Two pointers (slow + fast) move at different speeds. If a cycle exists, they will meet.

```python
# Detect if a cycle exists
from typing import Optional

def hasCycle(head: Optional[ListNode]) -> bool:
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next          # move 1 step
        fast = fast.next.next     # move 2 steps
        if slow is fast:
            return True
    return False
```

To find the entry node of the cycle (LeetCode 142): after a meeting, reset one pointer to head; move both 1 step until they meet again.

```python
# Return node where the cycle begins; None if no cycle
from typing import Optional

def detectCycle(head: Optional[ListNode]) -> Optional[ListNode]:
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            # Found a meeting point; find entry
            ptr1, ptr2 = head, slow
            while ptr1 is not ptr2:
                ptr1 = ptr1.next
                ptr2 = ptr2.next
            return ptr1
    return None
```

### 7.3 LRU Cache (What it is and a Simple Implementation) — LeetCode 146

What it is: An LRU (Least Recently Used) Cache evicts the item that was used the longest time ago when capacity is full. Operations must be O(1) for get and put.

How it works: Combine

* a hash map (key → node) for O(1) access, and
    
* a doubly linked list to maintain recency order (most-recent at head, least-recent at tail).
    

#### 7.3.1 Quick solution using collections.OrderedDict

```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.od = OrderedDict()

    def get(self, key: int) -> int:
        if key not in self.od:
            return -1
        self.od.move_to_end(key, last=False)  # mark as most-recent
        return self.od[key]

    def put(self, key: int, value: int) -> None:
        if key in self.od:
            self.od.move_to_end(key, last=False)
        self.od[key] = value
        if len(self.od) > self.cap:
            self.od.popitem(last=True)  # remove least-recent (tail)
```

#### 7.3.2 Full design with a custom doubly linked list

```python
class Node2:
    def __init__(self, k=0, v=0):
        self.k, self.v = k, v
        self.prev = None
        self.next = None

class LRUCacheDLL:
    def __init__(self, capacity: int):
        self.cap = capacity
        self.map = {}
        # sentinel head/tail to avoid edge cases
        self.head, self.tail = Node2(), Node2()
        self.head.next = self.tail
        self.tail.prev = self.head

    # helper: remove a node
    def _remove(self, node: Node2):
        p, n = node.prev, node.next
        p.next = n
        n.prev = p

    # helper: insert right after head (mark most-recent)
    def _add_front(self, node: Node2):
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def get(self, key: int) -> int:
        if key not in self.map:
            return -1
        node = self.map[key]
        self._remove(node)
        self._add_front(node)
        return node.v

    def put(self, key: int, value: int) -> None:
        if key in self.map:
            node = self.map[key]
            node.v = value
            self._remove(node)
            self._add_front(node)
        else:
            if len(self.map) == self.cap:
                # evict least-recent (node before tail)
                lru = self.tail.prev
                self._remove(lru)
                self.map.pop(lru.k)
            node = Node2(key, value)
            self.map[key] = node
            self._add_front(node)
```

---

## ✨ Conclusion

Linked Lists provide flexibility and efficiency for dynamic data storage. While arrays are better for random access, linked lists excel at insertion and deletion. Advanced problems like **cycle detection** and **LRU cache design** are common in interviews, making them essential topics for preparation.

---

## 🔗 References

* [GeeksforGeeks - Linked List](https://www.geeksforgeeks.org/data-structures/linked-list/)
    
* [MIT OpenCourseWare - Linked Lists](https://ocw.mit.edu/)
    
* [LeetCode 24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)
    
* [LeetCode 141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
    
* [LeetCode 142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
    
* [LeetCode 146. LRU Cache](https://leetcode.com/problems/lru-cache/)