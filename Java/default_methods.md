## Why default methods were introduced in Java 8 ?

The **purpose** behind the introduction of **default methods** in **Java 8** was to **enable the evolution of interfaces** in a backward-compatible way. Prior to Java 8, interfaces could only have **abstract methods** (method signatures without implementations), and this posed a major limitation as Java evolved.

---

### 🔍 **Why were default methods introduced?**

#### ✅ 1. **Backward Compatibility**

* Before Java 8, once an interface was published and widely used, it could not be changed (e.g., adding a method) without breaking all implementing classes.
* With default methods, you can **add new methods to interfaces** with a **default implementation**, so existing implementations don’t break.

**Example:**

```java
public interface MyInterface {
    void existingMethod();

    default void newMethod() {
        System.out.println("New method with default implementation");
    }
}
```

Now, existing classes implementing `MyInterface` don’t need to change.

---

#### ✅ 2. **Support for Functional Programming (Lambdas)**

* Java 8 introduced **lambdas** and **functional interfaces**.
* To enhance APIs like **Streams** and **Collections**, interfaces like `java.util.List` and `java.util.Map` needed new methods like `forEach`, `replaceAll`, etc.
* Default methods allowed these additions **without breaking** millions of lines of existing code.

---

#### ✅ 3. **Avoid Creating Utility Classes**

* Previously, developers often used **static utility classes** (like `Collections`, `Arrays`) to add functionality.
* Default methods allow these utility-style methods to be attached directly to interfaces, making APIs cleaner and more object-oriented.

---

### ⚠️ Potential Issue It Solved

Imagine wanting to add a method to an interface used by thousands of classes:

```java
interface Vehicle {
    void start();
}
```

Adding a method `void stop();` would break all implementations.

With default methods:

```java
default void stop() {
    System.out.println("Vehicle stopped");
}
```

No breakage, seamless evolution.

---

### 🚫 Why Not Just Use Abstract Classes?

* Java supports **multiple interface inheritance**, but **only single class inheritance**.
* Default methods allow sharing code across multiple types without the diamond problem (though conflicts can arise, and Java provides resolution rules).

---

### Summary

| **Need**                         | **How Default Methods Help**                 |
| -------------------------------- | -------------------------------------------- |
| Add methods to interfaces        | Without breaking existing code               |
| Support functional programming   | Enhance interfaces like `Iterable`, `Stream` |
| Reduce boilerplate               | Avoid utility/helper classes                 |
| Multiple inheritance of behavior | Without using abstract classes               |

---

Let me know if you'd like a practical code example showing default method usage and conflict resolution.
