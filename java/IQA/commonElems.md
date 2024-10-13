Given two arrays of strings we need to find common elements in java8


```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class CommonElements {
    public static List<String> findCommonElements(List<String> list1, List<String> list2) {
        return list1.stream()
                .filter(list2::contains)
                .collect(Collectors.toList());
    }

    public static void main(String[] args) {
        List<String> list1 = Arrays.asList("apple", "banana", "cherry", "date");
        List<String> list2 = Arrays.asList("cherry", "date", "elderberry", "fig");

        List<String> commonElements = findCommonElements(list1, list2);
        System.out.println("Common elements: " + commonElements);
    }
}

```

This code is identical to the previous response, except that it uses String objects instead of Integer objects. The logic remains the same: it filters the stream of elements from the first list to only include those that are also present in the second list, and then collects the filtered elements into a new list.
 * https://github.com/rockysahoo/java-program-for-interview-maven


___
Given two arrays we need to find common elements.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class CommonElements {
    public static List<Integer> findCommonElements(List<Integer> list1, List<Integer> list2) {
        return list1.stream()
                .filter(list2::contains)
                .collect(Collectors.toList());
    }

    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1, 2, 3, 4, 5);
        List<Integer> list2 = Arrays.asList(3, 4, 5, 6, 7);

        List<Integer> commonElements = findCommonElements(list1, list2);
        System.out.println("Common elements: " + commonElements);
    }
}
```

This code uses Java 8 streams to efficiently find common elements between two lists:
 * Stream creation: The stream() method on list1 creates a stream of elements from the first list.
 * Filtering: The filter(list2::contains) method filters the stream to only include elements that are also present in list2. The ::contains method is a method reference that refers to the contains method of the List interface.
 * Collection: The collect(Collectors.toList()) method collects the filtered elements into a new list and returns it.
This approach is concise and efficient, leveraging the power of Java 8 streams for filtering and collection.
 * https://github.com/rockysahoo/java-program-for-interview-maven
 * https://github.com/spezdmtest/JavaCore
 * https://github.com/Brickredsysjava/Lambda_Assignments


-----
