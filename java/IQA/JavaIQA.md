
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
