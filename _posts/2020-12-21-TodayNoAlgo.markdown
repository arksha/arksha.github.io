---
layout: post
title: Today No Algorithm
date: 2020-12-21
categories: Leetcode
comments: true
---

Records everyday's random problem

## [910. Smallest Range II](https://leetcode.com/problems/smallest-range-ii/)

**Intuition**
Imagine a pivot, if a number `x` smaller than it, then `x + K` to close to the pivot; if number `x` bigger than it, then `x - K` to close to the pivot.

So it would be nice to sort Array first, since we want minimum of max and min in the new Array.

Actually for `A[i] < A[j]` it does not matter `A[i] + K` , `A[j] - K` or `A[i] - K` , `A[j] + K` since `A[i]-K < A[i]+K < A[j]-K < A[j]+K`.

The biggest number will be `max = A[size - 1] - K` and smallest is `min = A[0] + K` Then we find if any number will change boarder above, maintain the most min among the difference max - min.


```
class Solution {
    public int smallestRangeII(int[] A, int K) {
        Arrays.sort(A);
        int left = A[0] + K, right = A[A.length - 1] - K;
        int res = A[A.length - 1] - A[0];
        for (int i = 0; i < A.length - 1; i++) {
            int maxNum = Math.max(right, A[i] + K);
            int minNum = Math.min(left, A[i + 1] - K);
            res = Math.min(res, maxNum - minNum);
        }
        
        return res;
    }
}
```

## [498. Diagonal Traverse](https://leetcode.com/problems/diagonal-traverse/)

**Intuition**
 - go through for each diagnal line and remember each ups and downs
 - reverse the diagnal line and put into results
 
 **Then**
 - simulate process, fill each number encounted in result
 - Since sum of `row + col` does not change, can use `(rol + col )% 2 == 0` to determine if is go up or down
 - Handle cases reach to border
    - when go up: if col reach to end row++, if row == 0, col++
    - when go down: if col == 0, row++, if row reach to end col++
 - remember edge case when matrix is empty

 ```
 class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return new int[0];
        }
        int i = 0, j = 0, m = matrix.length, n = matrix[0].length;
        
        int[] result = new int[m * n];
        for (int c = 0; c < result.length; c++) {
            result[c] = matrix[i][j];
           if ((i + j) % 2 == 0) { // up
               if (j == n - 1) {
                   i++;
               } else if (i == 0) {
                   j++;
               } else {
                   i--;
                   j++;
               }
           } else {
               if (i == m - 1) {
                   j++;
               } else if (j == 0) {
                   i++;
               } else {
                   i++;
                   j--;
               }
           }
        }
        return result;
    }
}
 ```

