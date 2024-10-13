Bean creation of parent child with different scope of bean.. parent is having singleton and child is prototype and whenever I calling parent .. how many child instances gets created

When you have a parent bean with **singleton** scope and a child bean with **prototype** scope in Spring, the behavior depends on how the child bean is injected or accessed. By default, when a singleton-scoped bean holds a reference to a prototype-scoped bean, the prototype bean is created **only once**, at the time the singleton bean is instantiated. This means the same instance of the prototype bean is reused every time the singleton bean is used.

However, if you want a new instance of the child (prototype) bean every time the parent (singleton) is accessed, you need to handle the prototype creation explicitly. Here’s a breakdown:

### Scenario 1: Default behavior (without intervention)

If the child (prototype) bean is simply injected into the parent (singleton) using something like `@Autowired`, then the child will be created **once**, when the parent bean is instantiated. That same child instance will be reused every time you call the parent.

```java
@Component
@Scope("singleton")
public class Parent {
    @Autowired
    private Child child; // prototype-scoped bean
}

@Component
@Scope("prototype")
public class Child {
    public Child() {
        System.out.println("Child instance created");
    }
}
```

In this case, only **one instance of the child** is created, because the child is injected during the parent's initialization, and the same instance is reused.

### Scenario 2: Explicit new instances of prototype bean

To ensure that a new instance of the child (prototype) bean is created every time the parent (singleton) is called, you need to use one of the following methods:

#### 1. **Using `ObjectFactory` or `Provider` (recommended)**

Spring provides mechanisms like `ObjectFactory` or `javax.inject.Provider` to get new instances of a prototype-scoped bean each time.

```java
@Component
@Scope("singleton")
public class Parent {
    @Autowired
    private ObjectFactory<Child> childFactory;

    public void doSomething() {
        Child child = childFactory.getObject();
        System.out.println("Using child: " + child);
    }
}
```

Now, every time `doSomething()` is called, a **new instance** of the `Child` bean is created.

#### 2. **Using `@Lookup` method injection**

You can use Spring's `@Lookup` annotation to instruct Spring to return a new instance of a prototype-scoped bean each time the method is called.

```java
@Component
@Scope("singleton")
public class Parent {
    public void doSomething() {
        Child child = createChild();
        System.out.println("Using child: " + child);
    }

    @Lookup
    protected Child createChild() {
        // Spring will override this method to return a new instance of the prototype-scoped bean
        return null;
    }
}
```

Each time `createChild()` is called, Spring will inject a new instance of the `Child` bean.

### Summary

- **By default**, a **singleton-scoped parent** will use the same **single instance** of a **prototype-scoped child**, created when the parent is initialized.
- To ensure a new child instance is created every time you call the parent, you need to use techniques like `ObjectFactory`, `Provider`, or `@Lookup`.
