# What is the Diamond Problem in Java?

The **diamond problem** is a classic issue in **multiple inheritance** (particularly in object-oriented programming), where a class inherits from two classes (or interfaces) that both inherit from a common superclass or interface. This creates ambiguity about which implementation should be used.

---

### 💎 **Why is it called the Diamond Problem?**

It’s called the **diamond problem** because of the shape formed in the inheritance hierarchy:

```
      A
     / \
    B   C
     \ /
      D
```

* `A` is the top-level class or interface.
* `B` and `C` both inherit from `A`.
* `D` inherits from both `B` and `C`.

The question is: **If `A` has a method, and `B` and `C` don’t override it, which version does `D` inherit?**

---

### 🔁 **In Java (with interfaces)**

Java **doesn't allow multiple inheritance of classes**, but it does allow multiple inheritance of **interfaces**, especially since Java 8 introduced **default methods**.

So the diamond problem can appear with **default methods in interfaces**.

---

### ⚠️ **Diamond Problem with Default Methods**

#### Example:

```java
interface A {
    default void greet() {
        System.out.println("Hello from A");
    }
}

interface B extends A {
    default void greet() {
        System.out.println("Hello from B");
    }
}

interface C extends A {
    default void greet() {
        System.out.println("Hello from C");
    }
}

class D implements B, C {
    // Compilation error unless greet() is overridden
}
```

This will cause a **compilation error** in class `D`, because Java doesn’t know whether to use `greet()` from `B` or `C`.

---

### ✅ **How to Resolve It**

Java forces you to **override the method** in class `D` and explicitly choose which default implementation to use.

```java
class D implements B, C {
    @Override
    public void greet() {
        // Choose one explicitly
        B.super.greet(); // or C.super.greet();
    }
}
```

---

### 🧠 Summary

| Concept             | Explanation                                                                                  |
| ------------------- | -------------------------------------------------------------------------------------------- |
| **Diamond Problem** | Ambiguity when a class inherits the same method from multiple sources                        |
| **Java’s Approach** | Allows multiple inheritance via interfaces, but requires conflicts to be resolved explicitly |
| **Resolution**      | Override and specify which interface’s method to use with `InterfaceName.super.method()`     |

---
