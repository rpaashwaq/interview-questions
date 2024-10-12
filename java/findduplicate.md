input=[1,1,2,2,3,3,4,4,8,8] By iterating only once, find the non duplicate number in java 8

```markdown
`<pre>``<code>`
import java.util.Arrays;
import java.util.stream.IntStream;

public class NonDuplicateFinder {

public static int findNonDuplicate(int[] input) {

return IntStream.of(input)

.boxed()

.reduce((a, b) -> a ^ b)

.orElse(0);

}

public static void main(String[] args) {
        
int[] input = {1, 1, 2, 2, 3, 3, 4, 4, 8, 8, 5}; // Added 5 as a non-duplicate number
        
int nonDuplicate = findNonDuplicate(input);

        System.out.println("Non-duplicate number: " + nonDuplicate);

    }


}
`</pre>``</code>`

Explanation:

Iterate once using IntStream: The IntStream.of(input) creates a stream of integers from the input array.

Box integers: boxed() converts the IntStream to a Stream<Integer> for using the reduce method.

Apply bitwise XOR: reduce((a, b) -> a ^ b) iterates over the stream and applies the bitwise XOR operation (^) to each pair of elements. This operation has the property that the XOR of a number with itself is 0, and the XOR of a number with 0 is the number itself.

Therefore, the non-duplicate number will remain after all duplicate pairs cancel each other out.
Handle empty input: orElse(0) returns 0 if the stream is empty.

Key points:

This solution leverages the properties of the bitwise XOR operation to efficiently find the non-duplicate number in a single pass.
It's concise and readable due to the use of Java 8 streams and functional programming concepts.
The orElse(0) handles the case of an empty input array.
