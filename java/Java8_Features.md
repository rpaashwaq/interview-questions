 ```markdown
what are all features of java 8 did uou used ?
```

1. Functional Interface ( include default & static method)
2. Lambda Expression
3. Stream
4. CompletqbleFuture
5. Java DateTime API

 ```markdown
what is functional interface
```

Interface that contains only one abstract method.
It can have any number of default and static methods.
e.g., Runnable, Callable, Comparator

 ```markdown
what are functional interfaces of java 8?
```

1. Function
2. Predicate
3. Consumer
4. Supplier

 ```markdown
Can you write one functional interface ?
 ```

   ```java
   import java.text.SimpleDateFormat;
   import java.util.Date;
   import java.util.Random;

   @FunctionalInterface
   public interface UPIPayment {

      // By default interface methods are public and abstract. FI have only one abstract method.
      String doPayment(String source, String destination); 

       default double getScratchCard(){
          return new Random().nextDouble();
       }

       static String datePatterns(String patterns){
          SimpleDateFormat sdf = new SimpleDateFormat(patterns);
          return sdf.format(new Date()); 
       }
   }
   ```

 ```markdown
Can we extend functional interface from another functional interface ?
```
  Yes we can extend, but it will not be functional interface after extending with another FI.

 ```markdown
what is Lambda expression?
```

it express instances of functional interfaces, 
in other words it provides a clear and concise way 
to represent method of a functional using a expression.



