---
layout: post
title: Today No Algorithm
date: 2020-12-21
categories: Leetcode
comments: true
---

# Today No Algorithm

## [910. Smallest Range II](https://leetcode.com/problems/smallest-range-ii/)

__Intuition__:

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