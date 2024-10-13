In java, Find a matrix element 19 from this given

01,02,03,09
10,11,12,13
17,18,19,21
25,27,28,29

```java
public class MatrixSearch {
    public static void main(String[] args) {
        int[][] matrix = {
                {1, 2, 3, 9},
                {10, 11, 12, 13},
                {17, 18, 19, 21},
                {25, 27, 28, 29}
        };

        int target = 19;

        if (searchMatrix(matrix, target)) {
            System.out.println("Element " + target + " found in the matrix.");
        } else {
            System.out.println("Element " + target + " not found in the matrix.");
        }
    }

    public static boolean searchMatrix(int[][] matrix, int target) {
        int rows = matrix.length;
        int cols = matrix[0].length;

        int i = 0;
        int j = cols - 1;

        while (i < rows && j >= 0) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] > target) {
                j--;
            } else {
                i++;
            }
        }

        return false;
    }
}
```

**Explanation:**

1. **Create the matrix:** Define the 2D array `matrix` containing the given elements.
2. **Set the target element:** Assign the target value (19) to the variable `target`.
3. **Call the `searchMatrix` function:** Pass the `matrix` and `target` to the `searchMatrix` function, which will return `true` if the element is found and `false` otherwise.
4. **Print the result:** Print a message indicating whether the element was found or not.

**The `searchMatrix` function:**

- Takes the matrix and target element as input.
- Initializes two pointers: `i` to the first row and `j` to the last column.
- While `i` is less than the number of rows and `j` is greater than or equal to 0:
  - If the current element at `matrix[i][j]` is equal to the target, return `true` (element found).
  - If the current element is greater than the target, move to the left column by decrementing `j`.
  - Otherwise, move to the next row by incrementing `i`.
- If the loop completes without finding the element, return `false` (element not found).

This approach efficiently searches the matrix by starting from the top-right corner and moving either left or down based on the comparison with the target element. It leverages the sorted nature of the matrix to optimize the search.
