```markdown
Gas station
https://www.interviewbit.com/coding-interview-questions/
```
Problem Description
``` 
 
Given two integer arrays A and B of size N. There are N gas stations along a circular route, where the amount of gas at station i is A[i].

You have a car with an unlimited gas tank and it costs B[i] of gas to travel from station i to its next station (i+1).
You begin the journey with an empty tank at one of the gas stations.

Return the minimum starting gas station's index if you can travel around the circuit once, otherwise return -1.

You can only travel in one direction. i to i+1, i+2, ... n-1, 0, 1, 2.. Completing the circuit means starting at i and ending up at i again.


Problem Constraints
1 <= |A| <= 5 * 105
|A| == |B|
0 <= Ai <= 5 * 103
0 <= Bi <= 5 * 103


Input Format
The first argument given is the integer array A. The second argument given is the integer array B.


Output Format
Return the minimum starting gas station's index if you can travel around the circuit once, otherwise return -1.


Example Input
A = [1, 2]
B = [2, 1]


Example Output
1


Example Explanation
If you start from index 0, you can fill in A[0] = 1 amount of gas.
Now your tank has 1 unit of gas. But you need B[0] = 2 gas to travel to station 1.

If you start from index 1, you can fill in A[1] = 2 amount of gas.
Now your tank has 2 units of gas. You need B[1] = 1 gas to get to station 0.
So, you travel to station 0 and still have 1 unit of gas left over.
You fill in A[0] = 1 unit of additional gas, making your current gas = 2.
It costs you B[0] = 2 to get to station 1, which you do and complete the circuit.
```
```markdown
Explainataion:

Imagine you're on a road trip with your family. You have a car that needs gas to run.
There are many gas stations along the way, but you don't know which one to start from to make it all the way around the trip.

Here's a simple way to think about it:

Check the gas: Look at how much gas is in each gas station.
Calculate the cost: Figure out how much gas it takes to get to the next station.
Subtract: See if you have enough gas to reach the next station.
Repeat: Keep checking, calculating, and subtracting for all the stations.
If you find a gas station where you have enough gas to reach the next station,
and you can keep going around the trip without running out, then that's the starting point you need!

```
Code in Java:
```java
import java.util.Arrays;

public class GasStation {
    public static int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        int totalGas = 0;
        int currentGas = 0;
        int start = 0;

        for (int i = 0; i < n; i++) {
            totalGas += gas[i] - cost[i];
            currentGas += gas[i] - cost[i];

            if (currentGas < 0) {
                start = i + 1;
                currentGas = 0;
            }
        }

        return totalGas >= 0 ? start : -1;
    }

    public static void main(String[] args) {
        int[] gas = {1, 2};
        int[] cost = {2, 1};
        int result = canCompleteCircuit(gas, cost);
        System.out.println(result);  // Output: 1
    }
}
```

Code in Java 8:
```java
import java.util.List;

public class GasStation {
    public int canCompleteCircuit(final List<Integer> A, final List<Integer> B) {
        int n = A.size();
        int totalGas = 0;
        int currentGas = 0;
        int start = 0;

        for (int i = 0; i < n; i++) {
            totalGas += A.get(i) - B.get(i);
            currentGas += A.get(i) - B.get(i);

            if (currentGas < 0) {
                start = i + 1;
                currentGas = 0;
            }
        }

        return totalGas >= 0 ? start : -1;
    }
}
```

Code in Python:
```python
def can_complete_circuit(A, B):
    """
    Determines the minimum starting gas station index to complete a circuit.

    Args:
        A: An integer array representing the amount of gas at each station.
        B: An integer array representing the cost to travel from one station to the next.

    Returns:
        The minimum starting gas station index, or -1 if it's impossible.
    """

    n = len(A)
    total_gas = 0
    current_gas = 0
    start_index = 0

    for i in range(n):
        total_gas += A[i] - B[i]
        current_gas += A[i] - B[i]

        if current_gas < 0:
            start_index = i + 1
            current_gas = 0

    if total_gas >= 0:
        return start_index
    else:
        return -1

# Example usage
A = [1, 2]
B = [2, 1]
result = can_complete_circuit(A, B)
print(result)  # Output: 1
```
