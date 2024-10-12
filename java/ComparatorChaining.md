Explain about Comparator chaining?

**Comparator chaining** in Java refers to the process of combining multiple `Comparator` objects to perform **multi-level sorting** of a collection. This is useful when sorting by one criterion isn’t enough, and you need to add secondary (or tertiary, etc.) sorting criteria in case two elements are considered equal based on the first criterion.

Java 8 introduced convenient methods for `Comparator` chaining, particularly the `thenComparing()` method, which allows you to chain multiple comparators together.

### How Comparator Chaining Works

1. **Primary Comparator**: Compares elements based on the first criterion.
2. **Secondary Comparator**: If two elements are considered equal by the primary comparator, this comparator is used.
3. **Tertiary Comparator** (optional): If the first two comparators determine that the elements are still equal, you can add additional comparators.

### Example: Comparator Chaining in Action

Suppose we have a `Person` class with three attributes: `name`, `age`, and `salary`. We want to sort a list of `Person` objects first by `age`, and then, if two persons have the same age, by `salary`.

```java
import java.util.*;
import java.util.Comparator;

class Person {
    String name;
    int age;
    double salary;

    public Person(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return name + " (" + age + " years old, $" + salary + " salary)";
    }
}

public class ComparatorChainingExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
                new Person("John", 25, 3000),
                new Person("Alice", 25, 2500),
                new Person("Bob", 22, 4000),
                new Person("David", 25, 3000),
                new Person("Carol", 22, 3000)
        );

        // Comparator chaining: first by age, then by salary, then by name
        Comparator<Person> personComparator = Comparator.comparing(Person::getAge)
                                                        .thenComparing(Person::getSalary)
                                                        .thenComparing(Person::getName);

        Collections.sort(people, personComparator);

        // Output the sorted list
        people.forEach(System.out::println);
    }
}
```

### Breaking Down the Comparator Chaining:

1. **Primary Comparator**: `Comparator.comparing(Person::getAge)` sorts by age.
2. **Secondary Comparator**: `.thenComparing(Person::getSalary)` is used if two persons have the same age, sorting by salary.
3. **Tertiary Comparator**: `.thenComparing(Person::getName)` is used if two persons have the same age and salary, sorting by name as a final tie-breaker.

### Output:

```
Bob (22 years old, $4000.0 salary)
Carol (22 years old, $3000.0 salary)
Alice (25 years old, $2500.0 salary)
David (25 years old, $3000.0 salary)
John (25 years old, $3000.0 salary)
```

### Example of Reversing a Comparator

You can also reverse the sorting order for any comparator using `Comparator.reversed()`:

```java
Comparator<Person> personComparator = Comparator.comparing(Person::getAge)
                                                .reversed()            // Sort by age in descending order
                                                .thenComparing(Person::getSalary)  // Sort by salary (ascending)
                                                .thenComparing(Person::getName);   // Sort by name (ascending)
```

This will sort the list by descending age, then ascending salary, then ascending name.

### Key Methods for Comparator Chaining

1. **`thenComparing(Comparator)`**:

   - Chains another `Comparator` to be used when the previous comparator considers elements equal.

2. **`thenComparing(Function keyExtractor)`**:

   - Takes a function that extracts a key and creates a comparator for that key. Similar to `Comparator.comparing()`.

3. **`thenComparing(Function keyExtractor, Comparator)`**:

   - Allows specifying both a key extractor and a custom comparator for sorting the key.

4. **`Comparator.reversed()`**:
   - Reverses the order of the comparator.

### Example: Custom Sorting

You can use a custom comparator in `thenComparing` if you need more control over how values are compared:

```java
Comparator<Person> personComparator = Comparator.comparing(Person::getAge)
                                                .thenComparing((p1, p2) -> Double.compare(p1.getSalary(), p2.getSalary()))
                                                .thenComparing(Person::getName);
```

This is equivalent to the earlier example, but here, we are explicitly specifying the comparison logic for the salary field using `Double.compare`.

### Summary

- **Comparator chaining** allows you to sort objects by multiple fields or criteria.
- The **`thenComparing()`** method is used to add secondary and tertiary comparators.
- You can combine comparators with ascending or descending order using methods like `Comparator.reversed()`.
- Comparator chaining is powerful for complex sorting scenarios where multiple levels of comparison are required.

Let me know if you'd like more examples or deeper exploration into any of the methods!
