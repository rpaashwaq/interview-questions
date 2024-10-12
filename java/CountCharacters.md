Count characters

String s = "aaabbccfddccb";
a-3,b-2,c-2,f-1,d-2,c-2,b

import java.util.HashMap;
import java.util.Map;

public class CharacterCount {
public static void countCharacters(String s) {
Map<Character, Integer> charCountMap = new HashMap<>();

        for (char c : s.toCharArray()) {
            charCountMap.put(c, charCountMap.getOrDefault(c, 0) + 1);
        }

        for (Map.Entry<Character, Integer> entry : charCountMap.entrySet()) {
            System.out.println(entry.getKey() + "-" + entry.getValue());
        }
    }

    public static void main(String[] args) {
        String s = "aaabbccfddccb";
        countCharacters(s);
    }

}

This code first creates a HashMap to store character counts. It then iterates through each character in the input string and updates the count in the map. Finally, it prints the character counts in the desired format.

---

Count characters in java8

String s = "aaabbccfddccb";
a-3,b-2,c-2,f-1,d-2,c-2,b

import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class CharacterCount {
public static void main(String[] args) {
String s = "aaabbccfddccb";

        Map<Character, Long> characterCount = s.chars()
                .mapToObj(c -> (char) c)
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        characterCount.forEach((key, value) -> System.out.println(key + "-" + value));
    }

}

Explanation:

Convert to stream: s.chars() converts the string s to a stream of character codes.

Map to characters: mapToObj(c -> (char) c) maps each character code back to a character object.

Group by character and count: collect(Collectors.groupingBy(Function.identity(), Collectors.counting())) groups the characters by their identity (using Function.identity()) and counts the occurrences of each character using Collectors.counting().

This results in a Map where the keys are characters and the values are their counts.

Print the results: characterCount.forEach((key, value) -> System.out.println(key + "-" + value)) iterates over the Map and prints each character-count pair in the desired format.

This code provides a concise and efficient way to count character occurrences in a string using Java 8 streams and functional programming.
