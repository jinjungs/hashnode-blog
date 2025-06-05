---
title: "[Python Basics] print() Formatting — A Quick Guide to Make Your Outputs Shine!"
seoTitle: "Python Print Formatting Guide"
seoDescription: "Learn about Python string formatting methods, including percent formatting, format() method, and f-strings, for cleaner, modern outputs"
datePublished: Tue Jun 03 2025 18:27:59 GMT+0000 (Coordinated Universal Time)
cuid: cmbguq2cb000a09jxaye1c7mf
slug: python-basics-print-formatting
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/hPm_MK0Os7k/upload/e33ecdfa6a5c09a4311a9b301245c6b2.jpeg
tags: python, python3, libraries, python-libraries, python-basics, pythontips, printf

---

# **1\. Percent (%) Formatting — The Old C-Style**

* This is the classic way of formatting strings, inspired by the C language.
    
    ```python
    name = "EJ"
    age = 28
    print("My name is %s and I am %d years old." % (name, age))
    ```
    
* **Common format codes**
    
    * %s: string
        
    * %d: integer
        
    * %f: float
        
* You can also control decimal places:
    
    ```python
    pi = 3.14159
    print("PI is %.2f" % pi)  # PI is 3.14
    ```
    

> ⚠️ Although it’s still supported, this method is considered outdated.

---

# **2\. format() Method — More Powerful & Flexible**

* Introduced in Python 2.7+, this approach offers more control and readability.
    
    ```python
    print("My name is {} and I am {} years old.".format(name, age))
    ```
    
* Use placeholders with indexes or variable names:
    
    ```python
    print("Name: {0}, Age: {1}".format(name, age))
    print("Name: {name}, Age: {age}".format(name=name, age=age))
    ```
    
* Format floats easily:
    
    ```python
    print("PI is {:.3f}".format(pi))  # PI is 3.142
    ```
    

---

# **3\. f-Strings — The Modern, Cleanest Way (Python 3.6+)**

* If you're using Python 3.6 or later, **this is the go-to method**. It’s concise, readable, and supports expressions directly inside the string.
    
    ```python
    print(f"My name is {name} and I am {age} years old.")
    ```
    
* Supports inline expressions:
    
    ```python
    print(f"Next year, I’ll be {age + 1} years old.")
    ```
    
* Formatting numbers:
    
    ```python
    print(f"PI is {pi:.2f}")  # PI is 3.14
    ```
    
* Text alignment:
    
    ```python
    print(f"|{'left':<10}|{'center':^10}|{'right':>10}|")
    # Output: |left      |  center  |     right|
    ```
    

---

# 4\. Conclusion

| Method | Pros | Cons |
| --- | --- | --- |
| `%` format | Simple, familiar in old code | Less readable, outdated |
| `.format()` | Powerful, supports variables | Verbose, slightly clunky |
| `f-String` | Clean, modern, readable | Requires Python 3.6+ |

While all three formatting methods are still usable, **f-strings are by far the most Pythonic and preferred way** to format your output today. If you're working in Python 3.6 or higher, make f-strings your default choice!

💬 *Have a favorite formatting trick or want to share how you use* `print()` in real projects? Drop a comment below!