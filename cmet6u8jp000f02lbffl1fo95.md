---
title: "Static Fields in Inner Classes? Yes, You Can (Since Java 16)"
seoTitle: "Static Fields in Inner Classes? Yes, You Can (Since Java 16)"
seoDescription: "Explore the updated Java 16 feature that allows static fields in non-static member and local classes, improving efficiency and simplifying code logic"
datePublished: Tue Aug 26 2025 23:39:30 GMT+0000 (Coordinated Universal Time)
cuid: cmet6u8jp000f02lbffl1fo95
slug: java-static-fields-in-inner-classes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756251380996/98f28b08-b975-4ed8-a66f-10d19d5e41c8.png
tags: java, static, nested-class, inner-classes, java16, static-field, member-class, local-class

---

## 1\. What Are Nested Classes?

A nested class is a class declared within another class. They are categorized into two types based on where they are declared. A nested class declared as a member of another class is called a **member class**, and a nested class declared within a method is called a **local class**.

| Category by Declaration | Sub-category | Declaration Location | Description |
| --- | --- | --- | --- |
| **Member Class** | Instance Member Class | class A {  
class B {...}  
} | A nested class `B` that can only be used after creating an object of `A`. |
| **Member Class** | Static Member Class | class A {  
static class B {...}  
} | A nested class `B` that can be accessed directly through class `A`. |
| **Local Class** | \- | class A {  
void method {  
class B {...}  
}  
} | A nested class `B` that can only be used during the execution of `method()`. |

---

## 2\. Can't Declare Static Fields in Non-Static Member Classes?

An instance member class (often called an inner class) is a member class declared without the `static` keyword. Traditionally, these inner classes could only declare instance fields and methods; static fields and methods were not allowed. The only exception was for `static final` fields that were compile-time constants.

But is that still true? Let's find out.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756250778182/cfe04358-67c8-4739-a2b2-2a2ced0fbaf6.png align="center")

We get a compile-time error, but the message suggests that this is now permitted in Java 16 and later. Let's press `Cmd + ;` to open the project settings and update the versions for the Project, Modules, and SDKs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756250721027/d4d88b9d-de87-424c-b394-9af062608bf7.png align="center")

Now, let's go back to the source code.

As you can see, the compile-time errors have disappea

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756250789763/15754ffc-bf39-4ad5-9d22-bb82dee61760.png align="center")

red for both the non-static member class and the local class!

After compiling, we can confirm that separate class files are generated for each nested class.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756250802657/ea8f9148-804f-42d3-bfb7-a3a39a1db046.png align="center")

Let's also write some test code to verify that it works as expected.

**Test Code**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756250807857/7c8bfbba-39b0-4219-9321-785657bc945c.png align="center")

**Result**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756250814991/cbf5cc65-d8b7-4df9-8f55-92483382a25d.png align="center")

We can see that the values are printed correctly.

---

## 3\. Why Was This Restriction Lifted?

So, why did Java 16 start allowing static field declarations in non-static member classes and local classes?

The primary reason was to **improve language consistency, remove unnecessary restrictions, and enhance developer convenience.**

This change was introduced as part of **JEP 395: Records**. Records are a new kind of type declaration designed to express data aggregates concisely. It was anticipated that records would often be declared as inner classes. The existing restriction on declaring `static` members in inner classes would have limited the utility of records and made code unnecessarily complex.

Let's dive a bit deeper.

### 3.1. Relaxing the 'Purity' Principle

Historically, Java enforced a 'purity' principle where a non-static inner class was completely dependent on an instance of its enclosing class. The idea was that nothing in the inner class could exist without an outer instance. Consequently, `static` members, which can exist independently of any instance, were disallowed.

Over time, this restriction was seen as impractical and a source of confusion and inconvenience for developers. The exception for compile-time constants (`static final`) already showed that this 'purity' principle wasn't absolute. Therefore, lifting the restriction entirely was deemed a step toward making the language simpler and more consistent. 🧐

### 3.2. Improving Code Conciseness and Readability

Because of this limitation, developers had to resort to workarounds like converting an inner class to a `static` nested class or moving it to a top-level class just to declare a `static` member.

**Example: Code from the Past**

```Java
public class Outer {
    // We want a helper class with a static method or variable, but...
    // it was impossible because this is a non-static inner class.
    class InnerHelper {
        // static int count = 0; // Compile-time error (Java 15 and earlier)
        void doSomething() {
            // ...
        }
    }
}
```

In the case above, one had to change `InnerHelper` to `static class InnerHelper` or move it to a separate file. Since Java 16, this is no longer necessary, allowing for more natural code structure and improved readability.

### 3.3. Synergy with Records

The main goal of JEP 395 was the introduction of **Records**. Records are often used as data transfer objects (DTOs) declared within a specific class or method. Declaring them as inner or local classes feels very natural. If inner classes couldn't declare `static` members, it would have created constraints on declaring related constants or factory methods, thus limiting the effectiveness of the records feature.

In conclusion, this change can be seen as part of a broader effort to modernize the Java language by removing an old restriction that no longer had a strong justification.

---

## 4\. Points of Caution

While the ability to use `static` members in inner classes is a convenient improvement, there are definitely things to watch out for. These are areas where developers accustomed to the old rules might make mistakes.

### 4.1. Potential for Memory Leaks

This is the most critical point to be aware of. An instance of a non-static inner class holds an implicit reference (`this$0`) to its enclosing class instance.

```Java
public class Outer {
    class Inner {
        // This static list can hold onto instances of Outer.
        static List<Outer> outerInstances = new ArrayList<>();

        void cacheOuterInstance() {
            // Access the Outer instance via the implicit reference
            outerInstances.add(Outer.this); 
        }
    }
}
```

* **The Problem**: The `static` variable `outerInstances` in the `Inner` class will persist in memory for the lifetime of the application. If this static collection holds references to instances of the `Outer` class, those instances will not be garbage collected, leading to a **memory leak**.
    
* **Precaution**: You must be very careful to design your code so that static fields in an inner class do not hold references to instances of the outer class or the inner class itself.
    

### 4.2. `static` Members Cannot Access the Outer Instance

Because `static` members exist independently, they **cannot access instance members of the enclosing class**, which is a key feature of non-static inner classes.

```Java
public class Outer {
    private int outerField = 10;

    class Inner {
        static int staticInnerField = 20;

        static void staticInnerMethod() {
            // System.out.println(outerField); // COMPILE-TIME ERROR!
            // Cannot access an instance field of the Outer class.
            System.out.println("Can be called without an outer instance");
        }
        
        void instanceInnerMethod() {
            System.out.println(outerField); // This is fine!
        }
    }
}
```

* **Don't Be Misled**: It's easy to mistakenly assume, "Since it's in an inner class, it can access outer members." Static methods and static initializer blocks run in a context that is completely independent of any instance of the outer class.
    

### 4.3. Complexity with Serialization

Serialization of inner classes has always been a complex topic. The addition of `static` members adds more to consider.

* `static` fields are not included in the serialization process by default.
    
* If a non-`transient` `static` field affects the state of a serialized object, the object may have an unexpected state upon deserialization, which can be a source of bugs.
    

### 4.4. Code Confusion and Reduced Readability

While this feature is convenient, overuse can make your code's structure difficult to understand.

* **Confusion with** `static` Nested Classes: Traditionally, a class with `static` members was made a `static` nested class. Now that non-static inner classes can also have `static` members, the distinction between the roles of these two types of classes can become blurred.
    
* **Design Principles**: The roles and responsibilities of your classes can become unclear. If there isn't a strong design reason for a `static` variable to be inside a non-static inner class, it may be better to stick with the traditional approach of using a `static` nested class or a top-level class.
    

---

## ✨ Conclusion

Allowing `static` members in inner classes is a welcome change that removes an unnecessary restriction, but you must understand its characteristics to use it correctly.

> Is this `static` member truly independent of any instance of the outer class?

You should only use this feature when you can confidently answer "yes" to this question. If there is even a slight relationship, it's safer to use a `static` nested class or reconsider your design to avoid potential memory leaks and confusion.

This experience taught me the importance of critically evaluating information, even when it comes from a book. I learned that language specifications can evolve, and it's crucial to develop a habit of checking official documentation and testing things out with my own code. By deeply understanding why a restriction existed and why it was removed, I felt my grasp of the fundamental concepts grew even stronger. 😁

## 🔗 References

* [JEP 395: Records - OpenJDK](https://openjdk.org/jeps/395)
    
* [Java Language Specification, SE 21, §8.1.3 Inner Classes](https://www.google.com/search?q=https://docs.oracle.com/javase/specs/jls/se21/html/jls-8.html%23jls-8.1.3)
    
* [Allow static members to be declared in inner classes - JDK Bug System](https://bugs.openjdk.org/browse/JDK-8254321)
    
* [Local and Nested Static Declarations - Oracle Docs](https://docs.oracle.com/javase/specs/jls/se16/preview/specs/local-statics-jls.html)
    
* [Write C-Style Local Static Variables in Java 16](https://blog.jooq.org/write-c-style-local-static-variables-in-java-16)