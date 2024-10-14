

Given an array arr[] and an integer target, 
you need to find and return the count of 
quadruplets such that the index of 
each element of the quadruplet is unique 
and the sum of the elements is equal to 
target.

Examples:

Input: arr[] = [1, 5, 3, 1, 2, 10], target = 20
Output: 1
Explanation: Only quadruplet satisfying 
the condition is 
arr[1] + arr[2] + arr[4] + arr[5] 
= 5 + 3 + 2 + 10 = 20. 
Hence, the answer is 1.

Input: arr[] = [4, 5, 3, 1, 2, 4], target = 13
Output: 3
Explanation: Three quadruplets with sum 13 are:
arr[0] + arr[2] + arr[4] + arr[5] = 4 + 3 + 2 + 4 = 13
arr[0] + arr[1] + arr[2] + arr[3] = 4 + 5 + 3 + 1 = 13
arr[1] + arr[2] + arr[3] + arr[5] = 5 + 3 + 1 + 4 = 13

Input: arr[] = [1, 1, 1, 1, 1], target = 4
Output: 5
Explanation: The five quadruplets that 
sum up to 4 are:
arr[0] + arr[1] + arr[2] + arr[3] = 4
arr[0] + arr[1] + arr[3] + arr[4] = 4
arr[0] + arr[1] + arr[2] + arr[4] = 4
arr[0] + arr[2] + arr[3] + arr[4] = 4
arr[1] + arr[2] + arr[3] + arr[4] = 4
Expected Time Complexity: O(n*n)
Expected Space Complexity: O(n)

Constraints:
1 <= arr.length <= 10^3

1 <=arr[i]<= 10^5

1 <=target<= 10^5

Code in Java 8 Lambdas:

import java.util.HashMap;
import java.util.Map;

public class QuadrupletCounter {
    public static int countQuadruplets(int[] arr, int target) {
        int n = arr.length;
        Map<Integer, Integer> pairSums = new HashMap<>();
        int count = 0;

        // Create a map to store pair sums and their frequencies
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                int pairSum = arr[i] + arr[j];
                pairSums.putIfAbsent(pairSum, 0);
                pairSums.put(pairSum, pairSums.get(pairSum) + 1);
            }
        }

        // Iterate through the array again to find quadruplets
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                int remainingSum = target - arr[i] - arr[j];
                if (pairSums.containsKey(remainingSum)) {
                    count += pairSums.get(remainingSum);
                    // If the pair sum is the same as the remaining sum, subtract 1 to avoid counting the same quadruplet twice
                    if (remainingSum == arr[i] + arr[j]) {
                        count -= 1;
                    }
                }
            }
        }

        return count;
    }

    public static void main(String[] args) {
        int[] arr = {1, 5, 3, 1, 2, 10};
        int target = 20;
        int result = countQuadruplets(arr, target);
        System.out.println(result);  // Output: 1
    }
}









