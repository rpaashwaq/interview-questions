```markdown
Explain about the design principles
```

## Design Principles in Software Development

**Table of Contents**

## [1. **SOLID Principles**](#solid-principles)
- [SOLID Principles](#solid-principles)
  - [Single Responsibility Principle (SRP)](#srp)
  - [Open/Closed Principle (OCP)](#ocp)
  - [Liskov Substitution Principle (LSP)](#lsp)
  - [Interface Segregation Principle (ISP)](#isp)
  - [Dependency Inversion Principle (DIP)](#dip)

## [2. **DRY Principle (Don't Repeat Yourself)**](#dry)
- [KISS Principle (Keep It Simple, Stupid)](#kiss)
- [YAGNI (You Aren't Gonna Need It)](#yagni)
- [Composition Over Inheritance](#composition-over-inheritance)
- [Law of Demeter (LoD)](#lod)
- [Favor Composition Over Inheritance](#favor-composition)
- [Conclusion](#conclusion)

Design principles in software development refer to high-level guidelines that help developers create robust, maintainable, scalable, and reusable systems. Following these principles ensures better software design, which leads to cleaner code, easier debugging, and better handling of future changes.

Here are some key design principles:

### 1. **SOLID Principles**

The SOLID principles are a set of five design principles introduced by Robert C. Martin (Uncle Bob) that promote maintainability, flexibility, and extensibility in object-oriented programming.

#### S - **Single Responsibility Principle (SRP)**

A class should have **only one reason to change**, meaning it should have only one responsibility or job. If a class has multiple responsibilities, changes in one responsibility might affect or break the other responsibilities.

**Example**: If a `User` class handles both user data and file management, it violates SRP. You should separate these concerns into different classes:

```java
class User { /* Handles user data */ }
class UserFileManager { /* Handles file operations */ }
```

[link](#o---openclosed-principle-ocp)

#### O - **Open/Closed Principle (OCP)**

A class should be **open for extension but closed for modification**. This means that you should be able to extend a class's behavior without modifying the original class, usually by using inheritance, interfaces, or composition.

**Example**: Adding new functionality via subclasses or decorators without changing the core class.

```java
interface Shape {
    double area();
}

class Circle implements Shape {
    public double area() { return Math.PI * radius * radius; }
}

class Square implements Shape {
    public double area() { return side * side; }
}

// We can add new shapes without modifying existing ones.
```

[link](#l---liskov-substitution-principle-lsp)

#### L - **Liskov Substitution Principle (LSP)**

Subtypes must be substitutable for their base types without altering the correctness of the program. In other words, objects of a derived class should be able to replace objects of the base class without affecting the program's behavior.

**Example**: If `Square` is a subclass of `Rectangle`, it should behave like a `Rectangle` when used in the program, without breaking functionality.

[link](#i---interface-segregation-principle-isp)

#### I - **Interface Segregation Principle (ISP)**

Clients should not be forced to depend on interfaces they do not use. Instead of having one large interface, create smaller, more specific interfaces so that implementing classes only need to implement the methods they actually use.

**Example**: Instead of having a single `Machine` interface with too many unrelated methods:

```java
interface Machine {
    void print();
    void scan();
    void fax();
}
```

Break it into smaller interfaces:

```java
interface Printer {
    void print();
}
interface Scanner {
    void scan();
}
interface Fax {
    void fax();
}
```

This way, a `Printer` class only needs to implement the `Printer` interface, not the other unnecessary methods.

[link](#d---dependency-inversion-principle-dip)

#### D - **Dependency Inversion Principle (DIP)**

High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces). Similarly, abstractions should not depend on details; details should depend on abstractions.

**Example**: Instead of a class depending on concrete implementations:

```java
class OrderProcessor {
    private CreditCardPayment payment = new CreditCardPayment();
}
```

Depend on an abstraction:

```java
class OrderProcessor {
    private PaymentMethod payment;

    public OrderProcessor(PaymentMethod payment) {
        this.payment = payment;
    }
}
```

This way, you can easily swap out different payment methods.

---

### 2. **DRY Principle (Don't Repeat Yourself)**

The DRY principle encourages the reduction of code duplication. You should aim to **abstract repeated code** into functions, classes, or modules to make the code more maintainable and reduce the chances of introducing inconsistencies or bugs.

**Example**: Instead of repeating a piece of logic in multiple places, refactor it into a method and reuse it.

```java
// Violates DRY
calculateDiscount(price, customer);
applyTax(price, customer);
calculateDiscount(price, anotherCustomer);
applyTax(price, anotherCustomer);
```

Refactor:

```java
void processTransaction(double price, Customer customer) {
    calculateDiscount(price, customer);
    applyTax(price, customer);
}

processTransaction(price, customer);
processTransaction(price, anotherCustomer);
```

---

### 3. **KISS Principle (Keep It Simple, Stupid)**

The KISS principle emphasizes keeping code **simple and straightforward**. Avoid unnecessary complexity or overengineering. Simple code is easier to understand, maintain, and test.

**Example**: Avoid writing overly complicated solutions for simple problems.

```java
// Overly complicated code
for (int i = 0; i < list.size(); i++) {
    if (list.get(i) == null) {
        list.remove(i);
        i--;
    }
}

// Simpler approach using standard library
list.removeIf(Objects::isNull);
```

---

### 4. **YAGNI (You Aren’t Gonna Need It)**

The YAGNI principle advocates for not implementing features until you **actually need them**. Avoid building features or adding complexity to your code for speculative future use, as this often leads to wasted effort and more maintenance work.

**Example**: Don’t add extra parameters or methods in a class "just in case" they’ll be needed later. Only build what the current requirements demand.

---

### 5. **Composition Over Inheritance**

Prefer **composition** over inheritance. Composition is the practice of building classes by combining other classes (by including them as member variables) rather than inheriting behavior from a parent class.

- **Inheritance** often leads to tightly coupled classes that are hard to maintain or modify.
- **Composition** provides more flexibility and allows you to build systems that are easier to extend or modify.

**Example**: Instead of subclassing `Bird` to create `FlyingBird` and `SwimmingBird`, use composition to separate behaviors:

```java
class Bird {
    private FlyingBehavior flyingBehavior;
    private SwimmingBehavior swimmingBehavior;

    public void fly() {
        flyingBehavior.fly();
    }

    public void swim() {
        swimmingBehavior.swim();
    }
}
```

---

### 6. **Law of Demeter (LoD)**

The **Law of Demeter** (also known as the principle of least knowledge) states that an object should know as little as possible about the inner workings of other objects. In practice, this means that objects should avoid calling methods on objects that were returned by other methods (avoiding chaining multiple calls together).

**Example**: Instead of:

```java
order.getCustomer().getAddress().getCity();
```

Refactor to:

```java
order.getCustomerCity();
```

This way, `Order` is responsible for retrieving the city, and the caller doesn't need to know about the internal structure of `Customer` or `Address`.

---

### 7. **Favor Composition Over Inheritance**

This principle advises using **composition** (i.e., "has-a" relationships) rather than inheritance ("is-a" relationships) whenever possible. It results in more flexible designs, as composition allows for better modularity and maintainability.

For example, instead of using inheritance to give a class certain behaviors, you can use interfaces and delegate those behaviors to separate classes, which leads to more reusable and decoupled code.

---

### Conclusion

Adhering to these design principles results in code that is easier to understand, test, maintain, and extend. By following principles like **SOLID**, **DRY**, **KISS**, and **composition over inheritance**, developers can build systems that are more robust, flexible, and resilient to changes.
