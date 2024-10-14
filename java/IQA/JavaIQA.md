
diff between class vs instance variables ?

## Class vs. Instance Variables in Java

| Feature | Class Variable | Instance Variable |
|---|---|---|
| Declaration | Within the class declaration, outside of any method. | Within a class, but inside a method or constructor. |
| Scope | Accessible throughout the class. | Accessible only within the instance where it's declared. |
| Memory Allocation | One copy per class, shared by all instances. | One copy per instance. |
| Modification | Can be modified directly using the class name. | Can only be modified through an instance. |
| Initialization | Default value (e.g., 0 for numbers, null for objects) if not explicitly initialized. | Default value (e.g., 0 for numbers, null for objects) if not explicitly initialized. |
| Example | `public static int classVariable = 10;` | `public int instanceVariable = 20;` |

what is terminal operator and non-terminal opertators in java 8

**Terminal and Non-Terminal Operators in Java 8 Streams**

Java 8 streams introduce a powerful mechanism for processing collections of data. They are composed of a sequence of operations, which can be categorized into two types: terminal and non-terminal.

**Non-Terminal Operators**

* **Intermediate operations** that transform the stream into another stream. They do not produce a final result and can be chained together.
* Examples: `map`, `filter`, `flatMap`, `sorted`, `distinct`, `limit`, `skip`.

**Terminal Operators**

* **Operations that produce a final result** and terminate the stream pipeline. They consume the entire stream and cannot be followed by any other operations.
* Examples: `forEach`, `collect`, `reduce`, `count`, `min`, `max`, `findFirst`, `findAny`.

**Key Differences**

| Feature | Non-Terminal Operators | Terminal Operators |
|---|---|---|
| Return Type | Always returns a `Stream` | Returns a value of the specified type |
| Side Effects | Can have side effects (e.g., modifying elements) | Often have side effects (e.g., printing results) |
| Termination | Do not terminate the stream | Terminate the stream |
| Chaining | Can be chained with other non-terminal operators | Cannot be followed by any other operations |

**Example:**

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Non-terminal operation: filtering even numbers
Stream<Integer> evenNumbers = numbers.stream().filter(n -> n % 2 == 0);

// Terminal operation: collecting even numbers into a list
List<Integer> evenNumbersList = evenNumbers.collect(Collectors.toList());
```

In this example, `filter` is a non-terminal operation that creates a new stream containing only even numbers. `collect` is a terminal operation that collects the even numbers into a list and terminates the stream.

**Additional Notes:**

* Non-terminal operators are lazy, meaning they are executed only when a terminal operation is applied.
* Terminal operators can be used to perform various actions, such as collecting results, finding specific elements, or performing calculations.

By understanding the difference between terminal and non-terminal operators, you can effectively use Java 8 streams to perform various data processing tasks in a concise and expressive way.

