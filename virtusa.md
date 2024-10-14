Virtusa Full Stack Java Developer Interview Questions
Virtusa Interview Experience | Full Stack Developer | 

Encapsulation, 
Abstraction, 
Virtual Functions, 
Void Pointers, 
Copy Constructors, 
Access Specifiers, 
Polymorphism, 
Database Normalization, 
ACID Properties, 
Data Warehousing, 
Types of Keys in Relational Model, 
Views in SQL, Threads in OS, 
Paging vs Segmentation.
---
Virtusa's interview process for Full Stack Java Developers includes a mix of 
coding questions, 
core concepts, and 
practical problems. 
Here are some of the key topics and questions 
that candidates might encounter:
---
Encapsulation: 
The bundling of data and methods within a class in Java.

Abstraction: 
The property by which only the essential details are displayed to the user.

Virtual Functions: 
Functions that can be overridden in subclasses.

Void Pointers: 
Pointers that have no associated data type and can hold any type.

Copy Constructors: 
Functions that initialize an object using another object of the same class.

Access Specifiers: 
Keywords used to control the accessibility of classes, methods, and fields.

Polymorphism: 
The ability of an object to take on different forms or types.

Database Normalization: 
The process of organizing data to minimize redundancy and anomalies.

ACID Properties: 
A set of properties that guarantee database transactions are processed reliably.

Data Warehousing: 
The storage of large amounts of data for statistical analysis.

Types of Keys in Relational Model: 
Including Candidate, Super, Primary, Alternate, and Foreign keys.

Views in SQL: 
Virtual tables created by selecting fields from one or more tables.

Threads in OS: 
Lightweight processes within a process that can be multithreaded.

Paging vs Segmentation: 
Differences in memory management strategies.

---
Explain Marker Interface. 
Explain OOPS concept alone with real time example. 
Explain about Constructor Chaining. 
Explain about static keyword. 
Why Strings are immutable. 
---
**Marker Interface**

A marker interface is an interface that doesn't declare any methods or constants. It simply serves as a tag or label to indicate that a class implementing it has certain characteristics or capabilities. The Java compiler uses marker interfaces to perform specific actions or provide additional information about classes.

**Examples of marker interfaces in Java:**

* **Serializable:** Classes implementing this interface can be serialized to a stream of bytes and deserialized back.
* **Cloneable:** Classes implementing this interface can be cloned using the `clone()` method.
* **Remote:** Classes implementing this interface can be used in remote method invocation (RMI).

**Object-Oriented Programming (OOP) Concepts**

OOP is a programming paradigm based on the concept of objects, which represent data and the operations that can be performed on that data. The four fundamental principles of OOP are:

1. **Encapsulation:** Bundling data and methods within an object to protect data integrity and control access.
2. **Inheritance:** Creating new classes (derived classes) based on existing classes (base classes), inheriting their properties and methods.
3. **Polymorphism:** The ability of objects of different types to be treated as if they were of the same type. It allows for flexible and extensible code.
4. **Abstraction:** Focusing on the essential features of an object while hiding unnecessary details.

**Example:**

Consider a real-world example of a car. A car can be represented as an object with properties like color, make, model, and methods like start(), stop(), accelerate(), etc.

**Constructor Chaining**

Constructor chaining is the process of calling one constructor from another within the same class. It's achieved using the `this()` keyword. Constructor chaining is useful for:

* **Initializing common fields:** Setting default values or ensuring certain conditions are met before object creation.
* **Providing different constructors:** Offering multiple ways to create objects with varying parameters.

**Example:**

```java
public class Person {
    private String name;
    private int age;

    public Person(String name) {
        this(name, 0); // Call the constructor with age parameter
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

**Static Keyword**

The `static` keyword in Java is used to create class-level variables and methods. These elements belong to the class itself rather than individual objects.

* **Static variables:** Shared by all instances of the class and can be accessed directly using the class name.
* **Static methods:** Can be called without creating an object of the class and can access only static members.

**Example:**

```java
public class MathUtils {
    public static final double PI = 3.14159;

    public static int factorial(int n) {
        // ... implementation
    }
}
```

**Why Strings are Immutable**

Strings in Java are immutable, meaning their values cannot be changed after creation. This has several advantages:

* **Security:** Prevents unauthorized modification of sensitive string data.
* **Caching:** Allows for efficient string comparison and caching.
* **Thread safety:** Ensures that strings are safe to use in multi-threaded environments.
* **Predictability:** Makes string behavior more consistent and easier to reason about.

**To create a new string with a different value, you must create a new string object.**





