`Collectors.groupingBy()` is one of the most commonly used collectors in the Java Streams API. It groups stream elements based on a classification function and returns a `Map`.

## 1. `Collectors.groupingBy()`

### Example: Group employees by department

```java
List<Employee> employees = List.of(
    new Employee("John", "IT"),
    new Employee("Alice", "HR"),
    new Employee("Bob", "IT")
);

Map<String, List<Employee>> result =
    employees.stream()
             .collect(Collectors.groupingBy(Employee::getDepartment));

System.out.println(result);
```

Output:

```java
{
  IT=[John, Bob],
  HR=[Alice]
}
```

### How it works

```java
groupingBy(classifier)
```

* `classifier` = function that determines the group key.
* Returns `Map<K, List<T>>`.

Equivalent to SQL:

```sql
SELECT department, *
FROM employee
GROUP BY department;
```

---

## 2. Variations of `groupingBy`

### a) Basic grouping

```java
Map<String, List<Employee>> map =
    employees.stream()
             .collect(Collectors.groupingBy(Employee::getDepartment));
```

---

### b) Group and count

```java
Map<String, Long> countByDept =
    employees.stream()
             .collect(Collectors.groupingBy(
                 Employee::getDepartment,
                 Collectors.counting()
             ));
```

Output:

```java
{IT=2, HR=1}
```

---

### c) Group and sum

```java
Map<String, Integer> salaryByDept =
    employees.stream()
             .collect(Collectors.groupingBy(
                 Employee::getDepartment,
                 Collectors.summingInt(Employee::getSalary)
             ));
```

Output:

```java
{IT=150000, HR=50000}
```

---

### d) Nested grouping

```java
Map<String, Map<String, List<Employee>>> result =
    employees.stream()
             .collect(Collectors.groupingBy(
                 Employee::getDepartment,
                 Collectors.groupingBy(Employee::getCity)
             ));
```

Output:

```java
{
  IT={
      Pune=[John],
      Mumbai=[Bob]
     }
}
```

---

## 3. Other Important `Collectors` Methods

The `Collectors` utility class contains many factory methods that create collectors.

### Collection Collectors

```java
toList()
toSet()
toCollection()
toMap()
toUnmodifiableList()
toUnmodifiableSet()
toUnmodifiableMap()
```

Example:

```java
List<String> names =
    employees.stream()
             .map(Employee::getName)
             .collect(Collectors.toList());
```

---

### Grouping / Partitioning

```java
groupingBy()
groupingByConcurrent()
partitioningBy()
```

Example:

```java
Map<Boolean, List<Employee>> result =
    employees.stream()
             .collect(Collectors.partitioningBy(
                 e -> e.getSalary() > 100000
             ));
```

Output:

```java
{
  true=[...],
  false=[...]
}
```

Unlike `groupingBy`, `partitioningBy` always creates exactly two groups (`true` and `false`).

---

### Counting / Statistics

```java
counting()
summarizingInt()
summarizingLong()
summarizingDouble()
averagingInt()
averagingLong()
averagingDouble()
summingInt()
summingLong()
summingDouble()
```

Example:

```java
IntSummaryStatistics stats =
    employees.stream()
             .collect(Collectors.summarizingInt(
                 Employee::getSalary
             ));
```

Contains:

```java
count
sum
average
min
max
```

---

### String Collectors

```java
joining()
joining(delimiter)
joining(delimiter, prefix, suffix)
```

Example:

```java
String names =
    employees.stream()
             .map(Employee::getName)
             .collect(Collectors.joining(", "));
```

Output:

```java
John, Alice, Bob
```

---

### Min / Max

```java
minBy()
maxBy()
```

Example:

```java
Optional<Employee> highestPaid =
    employees.stream()
             .collect(Collectors.maxBy(
                 Comparator.comparing(Employee::getSalary)
             ));
```

---

### Mapping / Transformation

```java
mapping()
flatMapping()
filtering()
collectingAndThen()
```

Example:

```java
Map<String, List<String>> namesByDept =
    employees.stream()
             .collect(Collectors.groupingBy(
                 Employee::getDepartment,
                 Collectors.mapping(
                     Employee::getName,
                     Collectors.toList()
                 )
             ));
```

Output:

```java
{
  IT=[John, Bob],
  HR=[Alice]
}
```

---

### Reducing

```java
reducing()
```

Example:

```java
int totalSalary =
    employees.stream()
             .collect(Collectors.reducing(
                 0,
                 Employee::getSalary,
                 Integer::sum
             ));
```

Though usually:

```java
employees.stream()
         .mapToInt(Employee::getSalary)
         .sum();
```

is preferred.

---

### Teeing (Java 12+)

Allows two collectors to run simultaneously.

```java
Collectors.teeing(
    Collectors.minBy(...),
    Collectors.maxBy(...),
    (min, max) -> ...
)
```

Example:

```java
record SalaryRange(int min, int max) {}

SalaryRange range =
    employees.stream()
             .collect(Collectors.teeing(
                 Collectors.minBy(Comparator.comparing(Employee::getSalary)),
                 Collectors.maxBy(Comparator.comparing(Employee::getSalary)),
                 (min, max) -> new SalaryRange(
                     min.get().getSalary(),
                     max.get().getSalary()
                 )
             ));
```

---

## Commonly Used Collectors in Real Projects

If you're preparing for interviews or working in Spring Boot applications, you'll use these most often:

| Collector             | Usage                                  |
| --------------------- | -------------------------------------- |
| `toList()`            | Collect stream to list                 |
| `toSet()`             | Collect stream to set                  |
| `toMap()`             | Convert stream to map                  |
| `groupingBy()`        | Group records                          |
| `partitioningBy()`    | Split into true/false groups           |
| `counting()`          | Count elements                         |
| `mapping()`           | Transform values during grouping       |
| `joining()`           | Concatenate strings                    |
| `summingInt()`        | Sum numeric values                     |
| `averagingInt()`      | Calculate average                      |
| `summarizingInt()`    | Get count, sum, avg, min, max together |
| `maxBy()` / `minBy()` | Find largest/smallest                  |
| `collectingAndThen()` | Post-process collected result          |

A useful way to think about `Collectors` is:

> A **Collector** defines **how stream elements should be accumulated into a final result**. `groupingBy`, `toList`, `joining`, `counting`, etc. are simply different strategies for collecting the elements of a stream.
