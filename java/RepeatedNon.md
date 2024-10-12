First Repeated and Non Repeated Character in a String Using Java8

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
