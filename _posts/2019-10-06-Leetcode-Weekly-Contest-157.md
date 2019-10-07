---
title:  "Leetcode Weekly Contest 157"
date:   2019-10-06 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Play with Chips][Problem1] 
   
    Since moving chips with 2 units cost 0, all chips on even index can be moved to position 0 with no cost and all chips on odd index can be move to position 1 with no cost. So we compare the number of chips on even index and number of chips on odd index and return the smaller one. The time complexity is `O(N)`
    
    ```python
        def minCostToMoveChips(self, chips):
            """
            :type chips: List[int]
            :rtype: int
            """
            odd = 0
            even = 0
            for i in chips:
                if i%2==0:
                    even+=1
                else:
                    odd+=1
            return min(even, odd)
    ```
    
     
    
2. [Longest Arithmetic Subsequence of Given Difference][Problem2]

   We use a map to remember current length of longest arithmetic subsequence end with key k. Then we loop the array, if arr[i] - difference in the map, we simply set value of current arr[i] in map to be value of  last plus one and update answer. Otherwise if the key arr[i] not in map, we set the value to be 1. The time complexity is `O(N)`.

   ```python
       def longestSubsequence(self, arr, difference):
           """
           :type arr: List[int]
           :type difference: int
           :rtype: int
           """
           dic = {}
           ans = 1
           cur = 1
           for i in range(len(arr)):
               last = arr[i] - difference
               if last in dic:
                   dic[arr[i]] = dic[last] + 1
                   ans = max(ans, dic[arr[i]])
               elif arr[i] not in dic:
                   dic[arr[i]] = 1
           return ans
   ```
   
   
   
3. [Path with Maximum Gold][Problem3]

   This problem is a simple Depth First Search, nothing special. The time complexity is `O(N^2*M^2)`

   ```python
       def getMaximumGold(self, grid):
           """
           :type grid: List[List[int]]
           :rtype: int
           """
           r = len(grid)
           c = len(grid[0])
           visited = [[False for _ in range(c)] for _ in range(r)]
           self.ans = 0
           def dfs(i, j, cur):
               if 0<=i<r and 0<=j<c and not visited[i][j] and grid[i][j]!=0:
                   visited[i][j] = True
                   cur += grid[i][j]
                   self.ans = max(self.ans, cur)
                   for (ii, jj) in [(i-1, j), (i+1, j), (i, j-1), (i, j+1)]:
                       dfs(ii, jj, cur)
                   visited[i][j] = False
           for i in range(r):
               for j in range(c):
                   dfs(i, j, 0)
           return self.ans
   ```
   
     
   
4. [Count Vowels Permutation][Problem4]

   Let a, e, i, o, u be the number of strings of current length that ends with 'a', 'e', 'i', 'o', 'u'. Then we loop n-1 times and in each round we simply apply all the rules mentioned and update a, e, i, o, u accordingly. Final answer is sum of a, e, i, o, u.

   The time complexity is `O(N)` 
   
   ``` python
       def countVowelPermutation(self, n):
           """
           :type n: int
           :rtype: int
           """
           a = e = i = o = u = 1
           while n > 1:
               na = ne = ni = no = nu = 0
               ne += a
               na += e
               ni += e
               nu += o
               ni += o
               na += u
               na += i
               ne += i
               no += i
               nu += i
               n -= 1
               a, e, i, o, u = na, ne, ni, no, nu
           return sum([a,e,i,o,u])%(10**9+7)
   ```
   
   

[Problem1]: https://leetcode.com/contest/weekly-contest-157/problems/play-with-chips/
[Problem2]: https://leetcode.com/contest/weekly-contest-157/problems/longest-arithmetic-subsequence-of-given-difference/
[Problem3]: https://leetcode.com/contest/weekly-contest-157/problems/path-with-maximum-gold/
[Problem4]: https://leetcode.com/contest/weekly-contest-157/problems/count-vowels-permutation/

