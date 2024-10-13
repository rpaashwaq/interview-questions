```markdown
Majority Element (Greedy Algorithm)
```

Problem Description
 
Given an array of size N, find the majority element. The majority element is the element that appears more than floor(N/2) times.
You may assume that the array is non-empty and the majority element always exist in the array.


Problem Constraints
1 <= |A| <= 106
1 <= Ai <= 109


Input Format
The first argument is an integer array A.


Output Format
Return the majority element.


Example Input
A = [2, 1, 2]


Example Output
2


Example Explanation
2 occurs 2 times which is greater than 3/2.

Code in Java:
```java
import java.util.HashMap;
import java.util.Map;

public class MajorityElement {
    public int majorityElement(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> countMap = new HashMap<>();

        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
            if (countMap.get(num) > n / 2) {
                return num;
            }
        }

        return -1; // Majority element not found
    }
}
```
Explanation:

```markdown
Imagine you have a group of friends, and you want to choose a movie to watch. You ask everyone their favorite movie, and the movie that most people choose is the "majority" movie.

Here's a simple way to find the majority movie:

1. **Count the votes:** Ask each friend their favorite movie and keep track of how many votes each movie gets.
2. **Find the winner:** The movie with the most votes is the majority movie.

In the example you gave, there are 3 friends and their favorite movies are:

* Friend 1: Movie A
* Friend 2: Movie B
* Friend 3: Movie A

If we count the votes, we see that Movie A has 2 votes and Movie B has 1 vote. Therefore, Movie A is the majority movie because it has more than half the votes.

```
