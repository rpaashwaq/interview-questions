First Repeated and Non Repeated Character in a String Using Java 8


Finding the First Repeated and Non-Repeated Characters in a String Using Java 8

Understanding the Problem:
 * Repeated Character: The first character that appears more than once in the string.

 * Non-Repeated Character: The first character that appears exactly once in the string.

Java 8 Solution:

We can leverage Java 8's Stream API and Collectors to efficiently solve this problem.

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
import java.util.stream.Collectors;

public class CharacterFrequency {
    public static void findFirstRepeatedAndNonRepeated(String str) {
        Map<Character, Long> frequencyMap = Arrays.stream(str.toCharArray())
                .collect(Collectors.groupingBy(c -> c, Collectors.counting()));

        Character firstRepeated = frequencyMap.entrySet().stream()
                .filter(entry -> entry.getValue() > 1)
                .map(Map.Entry::getKey)
                .findFirst()
                .orElse(null);

        Character firstNonRepeated = frequencyMap.entrySet().stream()
                .filter(entry -> entry.getValue() == 1)
                .map(Map.Entry::getKey)
                .findFirst()
                .orElse(null);

        System.out.println("First Repeated Character: " + firstRepeated);
        System.out.println("First Non-Repeated Character: " + firstNonRepeated);
    }

    public static void main(String[] args) {
        String str = "hello world";
        findFirstRepeatedAndNonRepeated(str);
    }
}
```

Explanation:

 * Frequency Map:
   * Convert the string to a character array and create a stream.
   * Group the characters by their value and count their occurrences using Collectors.groupingBy and Collectors.counting.
 * First Repeated Character:
   * Filter the frequency map entries for those with a count greater than 1.
   * Map the entries to their keys (characters).
   * Find the first such character using findFirst().
   * If no repeated character is found, return null.
 * First Non-Repeated Character:
   * Filter the frequency map entries for those with a count of 1.
   * Map the entries to their keys (characters).
   * Find the first such character using findFirst().
   * If no non-repeated character is found, return null.

This approach provides a concise and efficient solution to the problem, utilizing Java 8's functional programming features.
 * https://github.com/DenPixel/Sandbox


----

Using HashMap

```java
import java.util.HashMap;
import java.util.Map;

public class CharacterFrequency {
    public static char firstRepeatedCharacter(String str) {
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : str.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
            if (frequencyMap.get(c) == 2) {
                return c;
            }
        }
        return '\0';
    }

    public static char firstNonRepeatedCharacter(String str) {
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : str.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
        }
        for (char c : str.toCharArray()) {
            if (frequencyMap.get(c) == 1) {
                return c;
            }
        }
        return '\0';
    }

    public static void main(String[] args) {
        String str = "hello world";
        char firstRepeated = firstRepeatedCharacter(str);
        char firstNonRepeated = firstNonRepeatedCharacter(str);

        System.out.println("First repeated character: " + firstRepeated);
        System.out.println("First non-repeated character: " + firstNonRepeated);
    }
}
```

This code uses a HashMap to store the frequency of each character in the string. To find the first repeated character, we iterate through the string and check if the frequency of the current character is 2. If it is, we return that character. To find the first non-repeated character, we iterate through the string again and check if the frequency of the current character is 1. If it is, we return that character.
 * https://github.com/ericbor/algoexpert
