# Dynamic Programming
Dynamic programming is a problem-solving technique used in computer science to efficiently solve problems by breaking them down into simpler subproblems. It is particularly useful for optimization problems where the solution can be obtained by solving overlapping subproblems. The key idea behind dynamic programming is to store the results of subproblems so that they can be reused whenever needed, thereby avoiding redundant computations. This leads to significant improvements in runtime complexity compared to naive approaches that solve the same subproblems repeatedly.

The main motivation for using dynamic programming lies in its ability to solve problems with optimal substructure and overlapping subproblems more efficiently than other approaches. Optimal substructure means that the optimal solution to a problem can be constructed from the optimal solutions of its subproblems. Overlapping subproblems occur when the same subproblems are encountered multiple times during the computation. By storing the solutions to these subproblems in a table or cache, dynamic programming eliminates redundant calculations, resulting in a significant reduction in time complexity. Dynamic programming is particularly well-suited for problems like shortest path finding, sequence alignment, and knapsack problems, among others. Its elegance lies in transforming complex problems into simpler, more manageable subproblems, making it an indispensable tool in algorithm design and optimization.

## Fibonnaci
In mathematics, the Fibonacci numbers, commonly denoted Fn, form a sequence the
Fibonacci sequence, in which each number is the sum of the two preceding ones. The sequence
commonly starts from 0 and 1.

### Recursive Approach

```java
/**
 * Time Complexity: O(2^n)
 * Aux. Space Complexity: O(1) OR O(n) if you count the function call stack
 */
public static int fib(final int n) {
    if (n <= 1) {
        return 1;
    }
    return fib(n - 1) + fib(n - 2);
}
```

### Dynamic Programming Approach
```java
/**
 * Time Complexity: O(n)
 * Aux. Space Complexity: O(1)
 */
public static int fibDP(final int n) {
    /* Declare an array to store Fibonacci numbers. */
    int f[] = new int[n + 2]; 
    int i;

    /* 0th and 1st number of the series are 0 and 1*/
    f[0] = 0;
    f[1] = 1;

    for (i = 2; i <= n; i++) {
        /* Add the previous 2 numbers in the series
            and store it */
        f[i] = f[i - 1] + f[i - 2];
    }

    return f[n];
}
```
**Time Complexity:** The recursive approach has an exponential time complexity of ``O(2^N)``, while the dynamic programming approach has a linear time complexity of ``O(N)``. This makes the dynamic programming approach much more efficient, particularly for large values of N.

**Performance:** The dynamic programming approach performs much better in terms of runtime performance, especially for large values of N, due to its superior time complexity.

## Longest Common Subsequence
Given two strings, find the length of their longest common subsequence (LCS). A subsequence is a sequence that appears in the same relative order but not necessarily contiguous.

Dynamic programming can efficiently solve the LCS problem by using a bottom-up approach to build a table that stores the lengths of the longest common subsequences for all prefixes of the input strings.

1. Create a 2D array L of size (m+1) x (n+1), where m and n are the lengths of the input strings stringOne and stringTwo respectively. Initialize all elements of L to 0.
2. Iterate over each character in text1 and stringTwo.
3. If the characters are equal, increment the value in L corresponding to the current positions in both strings by 1, and take the diagonal element (i.e., L[i-1][j-1]) and add 1.
4. If the characters are not equal, take the maximum of the element above (i.e., L[i-1][j]) and the element to the left (i.e., L[i][j-1]).
5. Continue this process until the entire 2D array L is filled
6. The value in the bottom-right corner of L will be the length of the LCS.
7. Optionally, backtrack through the filled table to reconstruct the LCS itself.


### Dynamic Porgramming Approach
```java
/**
 * Time Complexity O(m*n)
 * m = len(stringOne)
 * n = len(stringTwo)
 * Aux. Space Complexity O(m*n)
 */
public static int LCS(final String stringOne, final String stringTwo) {
    final int[][] LCStuff = new int[stringOne.length() + 1][stringTwo.length() + 1];
    int result = 0;
    for (int i = 0; i <= stringOne.length(); i++) {
        for (int j = 0; j <= stringTwo.length(); j++) {
            if (i == 0 || j == 0)
                LCStuff[i][j] = 0;
            else if (stringOne.charAt(i - 1) == stringTwo.charAt(j - 1)) {
                LCStuff[i][j] = LCStuff[i - 1][j - 1] + 1;
                result = Integer.max(result, LCStuff[i][j]);
            } else
                LCStuff[i][j] = 0;
        }
    }
    return result;
}
```

This dynamic programming approach has a time complexity of  ``O(mn)``, where m and n are the lengths of the input strings text1 and text2. The space complexity is also ``O(mn)`` due to the 2D array used to store the lengths of the LCS for all prefixes of the input strings. However, it can be optimized to ``O(min(m, n))`` space complexity by using only two rows of the 2D array at a time.

## Knapsack Problem
Given a set of items, each with a weight and a value, and a knapsack with a maximum weight capacity, determine the maximum value that can be obtained by selecting a subset of the items to fit into the knapsack without exceeding its capacity.

### Brute-force Approach
The brute-force approach to the knapsack problem involves considering all possible subsets of items and calculating the total value of each subset. For each subset, we check if the total weight exceeds the knapsack capacity. If the weight is within the capacity, we compare the total value of the subset with the maximum value obtained so far and update it if necessary.

1. Generate all possible subsets of items using a recursive approach or a bitmasking technique.
2. For each subset, calculate the total weight and total value.
3. If the total weight is within the knapsack capacity, update the maximum value obtained so far if the total value of the current subset is greater.
4. Continue this process for all subsets.
5. The maximum value obtained after considering all subsets is the solution to the knapsack problem.

The brute-force approach has an exponential time complexity of ``O(2^N)``, where N is the number of items. This is because we are considering all possible subsets, leading to a combinatorial explosion in the number of subsets to be considered. As a result, this approach is impractical for large instances of the knapsack problem.

```java
public static int knapsack(final int W, final int[] w, final int[] vals) {
    return knapsack(W, w, vals, vals.length);
}

/**
 * Time Complexity O(2^n)
 * Aux. Space Complexity O(1)
 */
public static int knapsack(final int W, final int[] w, final int[] vals, final int n) {
    if (n == 0 || W == 0) {
        return 0;
    }

    if (w[n - 1] > W) {
        return knapsack(W, w, vals, n - 1);
    } else {
        return Math.max(vals[n - 1] + knapsack(W - w[n - 1], w, vals, n - 1), knapsack(W, w, vals, n - 1));
    }
}
```

### Dynamic Programming Approach
The dynamic programming approach to the knapsack problem efficiently solves it by breaking it down into simpler subproblems and storing the results of these subproblems to avoid redundant calculations.

```java
public static int knapsackDP(final int W, final int[] w, final int[] vals, final int n) {
        int[][] K = new int[2][W + 1];

        // We know we are always using the the current row or
        // the previous row of the array/vector . Thereby we can
        // improve it further by using a 2D array but with only
        // 2 rows i%2 will be giving the index inside the bounds
        // of 2d array K
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= W; j++) {
                if (i == 0 || j == 0) {
                    K[i % 2][j] = 0;
                } else if (w[i - 1] <= j) {
                    K[i % 2][j] = Math.max(
                            vals[i - 1] + K[(i - 1) % 2][j - w[i - 1]], K[(i - 1) % 2][j]);
                } else {
                    K[i % 2][j] = K[(i - 1) % 2][j];
                }
            }
        }
        return K[n % 2][W];
    }
}
```

We start by initializing a 2D array K with dimensions [2][W + 1]. This array is used to store the maximum values that can be obtained for different subproblems of the knapsack. Since we only need to refer to the current row and the previous row of the array during computation, we optimize memory usage by using a 2D array with only 2 rows.

We then iterate through each item represented by the variable i and each possible knapsack capacity represented by the variable j. The outer loop runs from 0 to n, where n is the total number of items.

Inside the nested loop, we handle different cases based on whether the current item can be included in the knapsack without exceeding its capacity:

1. If i is 0 (indicating the absence of any items) or j is 0 (indicating a knapsack with zero capacity), we set `K[i % 2][j]`` to 0 because there are no items to consider or no capacity available, resulting in zero value.
2. If the weight of the current item (``w[i - 1]``) is less than or equal to the current knapsack capacity (j), we consider including the item in the knapsack. In this case, we calculate the maximum value that can be obtained by either including or excluding the current item and store it in K[i % 2][j]. The maximum value is computed using the formula: ``Math.max(vals[i - 1] + K[(i - 1) % 2][j - w[i - 1]], K[(i - 1) % 2][j]),`` where ``vals[i - 1]`` represents the value of the current item.
3. If the weight of the current item exceeds the knapsack capacity, we cannot include it in the knapsack. In this case, we simply inherit the value from the previous row of the array by setting ``K[i % 2][j]`` to ``K[(i - 1) % 2][j]``.

After completing the iteration over all items and capacities, the maximum value that can be obtained for the given knapsack capacity is stored in ``K[n % 2][W]``, where n is the total number of items. Finally, we return this maximum value as the solution to the knapsack problem.
