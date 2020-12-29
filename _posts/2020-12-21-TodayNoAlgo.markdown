---
layout: post
title: Today No Algorithm
date: 2020-12-21
categories: Leetcode
comments: true
---

Records everyday's random problem

## 910.Smallest Range II - [Link](https://leetcode.com/problems/smallest-range-ii/) 

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

## 498. Diagonal Traverse - [Link](https://leetcode.com/problems/diagonal-traverse/)

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

## 91. Decode Ways - [Link](https://leetcode.com/problems/decode-ways/)

**Solution**

- DP, for each position i how many ways to decode can be solved with subproblems
- `dp[0]` means length 0 only has 1 way to decode
- check i - 1 is between 0 and 9, check i - 2 is between 10 and 26 and update decode numbers

 ```
 class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        // for length i has dp[i] ways to decode
        //need to check 2pos prev so need n + 1
        int[] dp = new int[n + 1]; 
        dp[0] = 1; // length 0 has 1 way to decode
        dp[1] = s.charAt(0) == '0' ? 0: 1; // length 1 has 1 way  to decode
        
        for (int i = 2;i < n + 1; i++) {
            Integer prev = Integer.valueOf(s.substring(i - 1, i));
            Integer prevtwo = Integer.valueOf(s.substring(i - 2, i));
            if (prev > 0 && prev <= 9) {
                dp[i] += dp[i - 1];
            }
            if (prevtwo >= 10 && prevtwo <= 26) {
                dp[i] += dp[i - 2];
            }
        }
        return dp[n];
    }
}
 ```
 **Extend**
62. Unique Paths
70. Climbing Stairs
509. Fibonacci Number
639. Decode Ways II

## 1345. Jump Game IV - [Link](https://leetcode.com/problems/jump-game-iv/)

**Solution**
- construct a graph use adj list
- put i - 1, i + 1 and j where arr[i] == arr[j] into neighbor list
- bfs count steps to target which is last element in array arr[n - 1]
- can use visit to exclude visited place or remove the visted point in map

```
class Solution {
    public int minJumps(int[] arr) {
        Map<Integer, List<Integer>> map = new HashMap<>(); // map index: neighbor list
        int n = arr.length;
        
        int result = 0;
        if (n == 1) return result;
        for (int i = n-1; i >= 0; i--) {
            map.computeIfAbsent(arr[i], x -> new ArrayList<Integer>()).add(i);
        }
        
        Queue<Integer> queue = new LinkedList<>();
        queue.add(0);
        
        while (!queue.isEmpty()) {
            result++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int current = queue.poll();
                if (current + 1 < n && map.containsKey(arr[current + 1])) {
                    if (current + 1 == n - 1) {
                        return result;
                    }
                    queue.offer(current + 1);
                }
                if (current - 1 >= 0 && map.containsKey(arr[current - 1])) {
                    queue.offer(current - 1);
                }
                if (map.containsKey(arr[current])){
                    for (int next: map.get(arr[current])) {
                        if (next != current) {
                            if (next == n - 1) {
                                return result;
                            }
                            queue.offer(next);
                        }
                    }
                    map.remove(arr[current]);
                }
            }
        }
        return result;
    }
}
```

## 754. Reach a Number - [Link](https://leetcode.com/problems/reach-a-number/)

**Solution**
- completely a math problem
- or can imagin find a sum that `sum >= target`
    - go find sum needs to go right and plus each step
    - At some point we have  `sum - target = number`, means already passed the target and need to go back. 
    - if `number` is even then steps is the answer. eg `sum - target = 4` means sum needs to minus 4 to equal to sum, we need to make right `+2` steps to go left `-2` steps

```
class Solution {
    public int reachNumber(int target) {
        // positive target and negative are the same
        // imagine go left is -, right is +
        // find the sum that equal or bigger than target only using +, then need go left to substract from sum
        // if choose go left, needs minus sum with *2, eg, start at pos 2, if go right, will be 4; if go left, should be 0, compare position with go right is (4-0) = 2*2. Like back and forth
        // if the sum - target is even, then return step
        // return step - 1 since everytime add 1 by step at first
        
        int sum = 0, step = 0;
        int posTarget = Math.abs(target);
        while(sum < posTarget || (sum - posTarget) % 2 != 0) {
            sum += step;
            step++;
        }
        return step - 1;
    }
}
```

