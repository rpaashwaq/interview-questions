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

 ```markdown
 What's the advantages and disadvantages of Lambda Expression it?
 ```
Advantage:
1. Avoid writing anonymous implementation
2. It save a lot of code
3. The code is directly readable without interpretation

Disadvantage:
1. Hard to use without IDE
2. Complex to debug

```markdown
What is Stream API?
```
Stream API introduced in java 8 and it is used to process collections of objects with functional style of coding using lambda expression.

```markdown
What is Stream in Java 8 ?
```
A stream is a sequence of objects that  supports various methods which can be pipelined to produce the desired result.
Features:
1. A stream is NOT a data structure instead it takes input from the Collections, Arrays or I/O channels.
2. Streams don't change the original data structure, they only provide the result as per the pipelined methods.

```markdown
What is method reference in Java 8 ?
```
Method Reference is a shorthand notation of a lambda expression to call a method.
e.g.,
List<Integer> numList = Arrays.asList(23,4,6,8,9,1,2,10);
numList.stream().filter(i -> i>5).sorted().forEach(t -> System.out::println(t));
output: 6,8,9,10,23

```markdown
Spell few stream method you used in your project ?
```
filter, forEach, sorted, map, flatMap, reduce, groupingBy, count, collect

```markdown
when to use map & flatMap ?
```
```java

public class User { private String name; private String phone; private List<String> email; }

List<User> users = Stream.of(
                     new User("user01", "1234567890", Arrays.asList("abc@gmail.com", "def@gmail.com")),
                     new User("user02", "0987654321", Arrays.asList("pqr@gmail.com", "stu@gmail.com")))
                .collect(Collectors.toList());     

//Find all phone numbers from users list.
List<String> phoneNumbers = users.stream()
                            .map(User::getPhone)
                            .collect(Collectors.toList()); System.out.println(phoneNumbers);

//Find all email addresses from users list. We need to use flatMap as it is stream of streams.
List<String> emails = users.stream()
                      .flatMap(e -> e.getEmail().stream())
                      .collect(Collectors.toList()); System.out.println(emails);
```
                     
```markdown
Count characters in a String ? or
Write a Program using Stream to Find frequency of each character in a given String ?
```
```java
String input ="Basanta";

Map<String,Long> countMap = Arrays.stream( input.split("") )
                             .collect(
                                       groupingBy(
                                          Function.identity(), counting()
                                        )
                              );
```
output: {a=3, B=1, s=1, t=1, n=1}

```markdown
Stream vs Parallel Stream ?
```
![image](https://github.com/user-attachments/assets/e35f3746-043f-41d0-9117-d9bae7cf78e9)

// Stream uses single core cpu thread .. executes all iterations in main method in sequence

IntStream.rangeClosed(1,10).forEach(e -> System.out.println(Thread.currentThread().getName() + ":"+ e);

// Parallel Stream utilizes all core cpu threads .. executes all iterations using thread pool workers along with one main thread.
// Not executed in sequence 

IntStream.rangeClosed(1,10)
                .parallel()
                .forEach(e -> System.out.println(Thread.currentThread().getName() +":"+ e);

## Stream vs ParallelStream in Java 8

| Feature | Stream | ParallelStream |
|---|---|---|
| **Execution** | Sequential (single thread) | Concurrent (multiple threads) |
| **Performance** | Generally slower for small datasets | Faster for large datasets due to parallelization |
| **Overhead** | Lower overhead | Higher overhead due to thread creation and management |
| **Determinism** | Order of operations is guaranteed | Order of operations may not be guaranteed |
| **Use Case** | Suitable for most scenarios | Ideal for CPU-intensive tasks with large datasets |
| **Example** | `List<Integer> numbers = Arrays.asList(1, 2, 3);` | `List<Integer> numbers = Arrays.asList(1, 2, 3).parallelStream();` |
| **Key Method** | `stream()` | `parallelStream()` |

**Note:** While `ParallelStream` can often improve performance for large datasets, it's important to consider factors like task granularity, synchronization overhead, and potential race conditions. Always measure and profile your applications to determine the optimal approach.



```markdown
Assume you have list of employees in various departments.
Write a program to find highest paid employee from each department ?
```
```java
public class Employee { private long id; private String name; private String dept; private double salary; }

List<Employee> employees = Stream.of(
   new Employee(1, "Basanat", "DEV", 50000),
   new Employee(2, "Santosh", "DEV", 80000),
   new Employee(3, "Pratik", "QA", 60000),
   new Employee(4, "Dipak", "QA", 90000),
   new Employee(5, "Bikash", "DEVOPS", 40000))
.collect(Collectors.toList());

Map<String, List<Employee>> emap1 = employees.stream().collect(groupingBy(Employee::getDept); System.out.println(emap1);

//approach 1
Comparator<Employee> compareBySalary = Comparator.comparing(Employee::getSalary);
Map<Sting,Optional<Employee>> employeeMap = employees.stream().
                                             collect( groupingBy(Employee::getDept, Collectors.reducing(BinaryOperator.maxBy(compareBySalary))));
System.out.println(employeeMap);

//approach 2
Map<String, Employee> employeeMap1 = employees.stream().
                                      collect(groupingBy(
                                                Employee::getDept,
                                                CollectingAndThen(maxBy(Comparator.compaingDouble(Employee::getSalary)), Optional::get)
                                     ));

System.out.println(employeeMap1);
```

```markdown
What is CompletableFuture ?
```
Used in Asynchronous Programming in Java, meaning writing a non-blocking code by running a task on a seperate thread than 
the main application thread and notifying the main application thread and notify the main thread about its progress, completion of failure. 


```markdown
Why CompletableFuture why not Future ?
```
1. It can not be manually completed
2. Multiple Futures can not be chained together
3. You can not combine multiple futures together
4. no exception handling

```java
package org.java.lambda_expressions;

import java.util.concurrent.CompletableFuture;
import java.util.function.IntPredicate;
import java.util.stream.IntStream;

public class EvenOddPrinter {

    private static  Object object = new Object();
    private static IntPredicate evenCondition = number -> number % 2 == 0;
    private static IntPredicate oddCondition = number -> number % 2 != 0;

    public static void main(String[] args) throws InterruptedException {
        CompletableFuture.runAsync(() -> printNumber(oddCondition));
        CompletableFuture.runAsync(() -> printNumber(evenCondition));
        Thread.sleep(1000);
        System.out.println("Done printing");
    }

    public static void printNumber(IntPredicate condition) {
        IntStream.rangeClosed(1,10)
                .filter(condition)
                //.forEach(number -> execute(number));
                .forEachOrdered(EvenOddPrinter::execute);
    }
      public static void execute(int number) {
          synchronized (object) {
              try{
                  System.out.println(Thread.currentThread().getName()+":"+number);
                  object.notify();
                  object.wait();
              }catch (Exception e) {
                  e.printStackTrace();
              }
          }
      }
}

```
