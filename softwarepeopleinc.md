# dev.it

<div class="details-wrapper_rendered details-wrapper">
  <details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">


      The `super` keyword in Java is a reference used to access members of the **immediate parent class**. 
      I would use it in three critical scenarios:
    
        ***
        
    1. Invoking the Parent Constructor (Mandatory Use)
        
        I use `super(...)` as the **first statement** in a subclass constructor to execute a specific constructor 
        of the superclass. This is essential for **correctly initializing the inherited state** 
        before the subclass initializes its own.
        
        * **Example:** `class Dog extends Animal { public Dog(String breed) { super("Canine"); /* ... */ } }`
        
    2. Accessing Parent Methods (Overriding/Extension)
        
        I use `super.methodName()` to call the **overridden method** in the parent class. This allows me to extend or 
        decorate the inherited logic rather than entirely replacing it.
        
        * **Architectural Use:** In Spring development, if I override a framework method, I often call `super.method()` 
        to ensure base functionality is preserved while adding custom business logic.
        
    3. Accessing Parent Fields (Shadowing)
        
        I use `super.fieldName` to explicitly reference an instance variable defined in the parent class when the subclass 
        has defined a variable with the **same name**, thus **hiding** the parent's field. 
        This ensures I access the intended scope.

</div>
</details>

<details>
    <summary>What are default values assigned to variables and instances in java?</summary>
    <div class="preserve-format">

      Java assigns default values to fields (instance variables) and static variables if they aren't explicitly initialized. 
      Local variables, however, are not assigned defaults and must be explicitly initialized before use.

  <img width="819" height="453" alt="image" src="https://github.com/user-attachments/assets/e4aa8e82-c897-4d3a-801e-73fae57f8572" />

    Local Variables (No Default)
      Local variables (variables declared inside a method, constructor, or block) are not assigned a default value by the Java compiler.
      If you attempt to use a local variable before explicitly initializing it, the Java compiler will throw a compile-time error.
      This is a safety mechanism to prevent programming errors resulting from using uninitialized values.
      
</div>
</details>


<details>
    <summary>Static methods vs variables vs classes?</summary>
    <div class="preserve-format">
        
        That's an important comparison in Java object-oriented design. 
        Here is a concise breakdown of static methods, static variables, 
        and static (nested) classes from an architectural perspective:
  <img width="803" height="498" alt="image" src="https://github.com/user-attachments/assets/f7f1899e-b61b-4106-a558-6f240bcf7b6e" />
              
    Comparison Details
    
    1. Static Variables (Class Variables)
      
      Usage: Declared using static type variableName;.
    
      Behavior: Only one copy of the variable exists, regardless of how many objects are created. 
      All objects share the same memory location for this variable.
      
      Best For: Counters, global constants (public static final), or shared resource pools.
      
    2. Static Methods (Class Methods)
      
      Usage: Declared using public static returnType methodName().
      
      Constraint: Since they don't belong to an object, they cannot access non-static (instance) variables or methods directly. 
      They must operate only on static data or passed-in parameters.
      
      Best For: Factory methods (getInstance()), utility methods (Math.sqrt()), or entry points (main()).
      
    3. Static Nested Classes
      
      Context: Only applies to nested classes (classes defined inside another class).
      
      Behavior: A static nested class is just like a regular top-level class, but its name is scoped within its outer class. 
      Crucially, it does not hold an implicit reference to the instance of the outer class.
      
      Best For: Building helper classes (e.g., a custom Comparator or a Builder pattern implementation) that are logically 
      tied to the outer class but don't need its internal object data.
   <img width="791" height="187" alt="image" src="https://github.com/user-attachments/assets/3a8e325d-a9d2-41fa-a6e9-a350ad1c2f4b" />
      
</div>
</details>


<details>
    <summary>Importance of reflection java?</summary>
    <div class="preserve-format">

      The importance of Reflection in Java, as a senior architect, lies in its ability to enable dynamic runtime 
      operations critical for building extensible and flexible frameworks, testing tools, and system integration components.
   <img width="798" height="535" alt="image" src="https://github.com/user-attachments/assets/ef5f789b-2c7e-4885-821c-698b07df21c8" />
               
    Architectural Trade-offs
        While powerful, reflection comes with significant trade-offs that limit its use to framework code:
        
        Performance Overhead: 
        
        Reflection is slower than direct method calls because the JVM must perform security checks 
        and method lookup at runtime. We avoid it in high-throughput business logic.
        
        Security Risk: 
        
        It can bypass normal access control (e.g., accessing private fields), necessitating 
        strict security manager policies in restricted environments.
        
        Code Fragility: 
        
        It breaks abstraction and encapsulation. If a class member is renamed, code using reflection to 
        access it will fail silently at runtime, not at compile time.
</div>
</details>

<details>
    <summary>Is it mandatory for a catch block to be followed after a try block?</summary>
    <div class="preserve-format">

      No, it's not mandatory for a try block to be immediately followed by a catch block.
      
      A try block must be followed by either:
            One or more catch blocks (to handle specific exceptions).
            A finally block (to execute cleanup code regardless of whether an exception occurred).
            Both a catch block (or blocks) and a finally block.
            The compiler enforces that a try block cannot exist on its own.
   <img width="799" height="318" alt="image" src="https://github.com/user-attachments/assets/c037183e-4068-4217-907a-7a39289471a5" />

</div>
</details>



<details>
    <summary>Composition vs Aggregation diff?</summary>
    <div class="preserve-format">

      The difference between Composition and Aggregation lies in the strength of the relationship and the lifetime dependency 
      between the component class and the container class. 
      Both represent "Has-A" relationships in Object-Oriented Programming (OOP).
   <img width="810" height="470" alt="image" src="https://github.com/user-attachments/assets/c4d59fd5-abab-4e27-97a9-a3103f43562a" />

    Summary:
      As an architect, I use:
        Composition when I need to enforce a tight lifecycle dependency and model objects that are fundamentally a part of a whole 
        (e.g., a Logger instance that is created and destroyed with its Service).
      
      Aggregation when modeling shared or optional resources and looser connections where the constituent 
      object may outlive the container (e.g., a Team aggregates a list of Employee objects).
</div>
</details>


<details>
    <summary>Can you explain why synchronization is necessary with the help of an example?</summary>
    <div class="preserve-format">

    Synchronization is necessary to prevent data corruption and ensure data consistency in a multi-threaded environment 
    where multiple threads access a shared mutable resource concurrently. 
    It enforces that only one thread can execute a specific block of code (the critical section) at a time.
    
    The Necessity of Synchronization: Race Conditions
    Synchronization prevents a common problem known as a Race Condition.
    
    A race condition occurs when the correctness of a computation depends on the unpredictable sequence 
    or timing of operations in multiple threads.
    
    Example: A Simple Counter 🔢
    Consider a banking application with a shared balance variable and an increment method 
    that two different threads (Thread A and Thread B) call simultaneously.
    
    The Shared Mutable Resource:

    public class SharedCounter {
    private int balance = 0; // Shared mutable resource

    // UNSAFE: No synchronization
    public void increment() {
        // This single line compiles down to three CPU instructions:
        // 1. READ: Read 'balance' from memory (e.g., 100)
        // 2. MODIFY: Increment value (e.g., 100 + 1 = 101)
        // 3. WRITE: Write new value back to memory (e.g., 101)
        balance = balance + 1; 
    }
}
</div>
</details>



<details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">

</div>
</details>



<details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">

</div>
</details>


<details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">

</div>
</details>


<details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">

</div>
</details>


<details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">

</div>
</details>


<details>
    <summary>When you would use the super keyword?</summary>
    <div class="preserve-format">

</div>
</details>



    
<style>
.details-arrow {
    left: 16px !important;
    top: 42% !important;
    transition: revert;
    transform: rotate(317deg);
}
.details-wrapper details {
    border: 1px dotted var(--e360-base-light-gray, #ccc) !important;
    transition: border 0.3s ease;
    padding: 8px;
    margin-bottom: 12px;
    border-radius: 4px;
    background: #f9f9f9;
}
.details-wrapper details:hover {
    border: 1px solid var(--e360-base-light-gray, #aaa) !important;
}
.details-wrapper_rendered summary {
    left: 19px;
    position: relative;
    font-weight: 600;
    cursor: pointer;
    padding: 4px 0;
}
.details-wrapper details[open]+.details-arrow {
    transform: rotate(45deg);
}
details summary {
    outline: none;
}
details[open] summary {
    color: #3178c6;
}

/* Preserve whitespace and line breaks for answer paragraphs */
.preserve-format {
    white-space: pre-wrap;
    font-family: inherit;
    font-size: 1rem;
}
</style>


  </details>
</div>
