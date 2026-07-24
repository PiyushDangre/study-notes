## Comparable vs Comparator




In Java, both **`Comparable`** and **`Comparator`** are interfaces used to sort collections or arrays of custom objects. However, they serve different purposes and are used in different scenarios.

---

## 1. `Comparable` Interface

The `Comparable` interface is used to define the **natural ordering** of an object. A class implements `Comparable` to give its instances a default way to be sorted (e.g., sorting `Student` objects by ID or `Employee` objects by name).

* **Package:** `java.lang`
* **Single Abstract Method:** `int compareTo(T o)`
* **Usage:** Modifies the class itself.

### How `compareTo()` Works:

* Returns a **negative integer** if `this` object is smaller than the specified object.
* Returns **zero** if `this` object is equal to the specified object.
* Returns a **positive integer** if `this` object is greater than the specified object.

### Code Example:

```java
import java.util.*;

class Student implements Comparable<Student> {
    private int id;
    private String name;

    public Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() { return id; }
    public String getName() { return name; }

    // Natural ordering: Sort by ID in ascending order
    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.id, other.id);
    }

    @Override
    public String toString() {
        return id + ": " + name;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();
        list.add(new Student(103, "Alice"));
        list.add(new Student(101, "Bob"));
        list.add(new Student(102, "Charlie"));

        // Uses Student's compareTo method automatically
        Collections.sort(list);

        System.out.println(list); // [101: Bob, 102: Charlie, 103: Alice]
    }
}

```

---

## 2. `Comparator` Interface

The `Comparator` interface is used to define **multiple or custom sorting sequences**. It allows you to sort objects based on different attributes without modifying the original class source code (e.g., sorting `Student` by name instead of ID, or in descending order).

* **Package:** `java.util`
* **Single Abstract Method:** `int compare(T o1, T o2)`
* **Usage:** Implemented in a separate class or passed as a Lambda/Method Reference.

### How `compare()` Works:

* Returns a **negative integer** if `o1` is less than `o2`.
* Returns **zero** if `o1` is equal to `o2`.
* Returns a **positive integer** if `o1` is greater than `o2`.

### Code Example:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();
        list.add(new Student(103, "Charlie"));
        list.add(new Student(101, "Alice"));
        list.add(new Student(102, "Bob"));

        // Custom Comparator: Sort by Name (Alphabetical) using Lambda
        Comparator<Student> nameComparator = (s1, s2) -> s1.getName().compareTo(s2.getName());

        // Or using modern Comparator methods:
        // Comparator<Student> nameComparator = Comparator.comparing(Student::getName);

        list.sort(nameComparator);

        System.out.println(list); // [101: Alice, 102: Bob, 103: Charlie]
    }
}

```

---

## Differences Between `Comparable` and `Comparator`

| Feature | `Comparable` | `Comparator` |
| --- | --- | --- |
| **Package** | `java.lang` | `java.util` |
| **Abstract Method** | `compareTo(T o)` | `compare(T o1, T o2)` |
| **Purpose** | Defines **natural ordering** for a class. | Defines **custom/multiple orderings**. |
| **Class Modification** | Requires modifying the original class source code (`implements Comparable`). | Does **not** require modifying the target class. |
| **Number of Sorts** | Allows only **one** default sorting logic. | Allows **multiple** sorting logics (by ID, Name, Salary, etc.). |
| **Method Signature** | Takes **1 argument** (`this` is compared against `o`). | Takes **2 arguments** (`o1` is compared against `o2`). |
| **Execution** | `Collections.sort(list)` | `Collections.sort(list, comparator)` or `list.sort(comparator)` |

---

## Summary Guidelines

> * Use **`Comparable`** when you control the class source code and there is an obvious, universal way to order its instances (like sorting by numerical ID or primary key).
> * Use **`Comparator`** when you want multiple ways to sort objects, when working with third-party classes you can't edit, or when writing inline sorting logic via Lambdas.
> 
>
