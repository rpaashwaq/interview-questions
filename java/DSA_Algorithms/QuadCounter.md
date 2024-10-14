

Given an array arr[] and an integer target, you need to find and return the count of quadruplets such that the index of each element of the quadruplet is unique and the sum of the elements is equal to target.

Examples:

Input: arr[] = [1, 5, 3, 1, 2, 10], target = 20
Output: 1
Explanation: Only quadruplet satisfying the condition is arr[1] + arr[2] + arr[4] + arr[5] = 5 + 3 + 2 + 10 = 20. Hence, the answer is 1.
Input: arr[] = [4, 5, 3, 1, 2, 4], target = 13
Output: 3
Explanation: Three quadruplets with sum 13 are:
arr[0] + arr[2] + arr[4] + arr[5] = 4 + 3 + 2 + 4 = 13
arr[0] + arr[1] + arr[2] + arr[3] = 4 + 5 + 3 + 1 = 13
arr[1] + arr[2] + arr[3] + arr[5] = 5 + 3 + 1 + 4 = 13
Input: arr[] = [1, 1, 1, 1, 1], target = 4
Output: 5
Explanation: The five quadruplets that sum up to 4 are:
arr[0] + arr[1] + arr[2] + arr[3] = 4
arr[0] + arr[1] + arr[3] + arr[4] = 4
arr[0] + arr[1] + arr[2] + arr[4] = 4
arr[0] + arr[2] + arr[3] + arr[4] = 4
arr[1] + arr[2] + arr[3] + arr[4] = 4
Expected Time Complexity: O(n*n)
Expected Space Complexity: O(n)

Constraints:
1 <= arr.length <= 103
1 <=arr[i]<= 105
1 <=target<= 105

