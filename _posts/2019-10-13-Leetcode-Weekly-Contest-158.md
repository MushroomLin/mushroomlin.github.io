---
title:  "Leetcode Weekly Contest 158"
date:   2019-10-13 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Split a String in Balanced Strings][Problem1] 
   
    We can just use greedy search. Whenever there is left count equals right count, we clear both count and add 1 to final answer. The time complexity is `O(N)`
    
    ```python
    	def balancedStringSplit(self, s):
            """
            :type s: str
            :rtype: int
            """
            l, r, cnt = 0, 0, 0
            for i in s:
                if i == "L":
                    l += 1
                else:
                    r += 1
                if l == r:
                    cnt += 1
                    l = 0
                    r = 0
            return cnt
    ```
    
     
    
2. [Queens That Can Attack the King][Problem2]

   We can search from king and reach queens on all eight directions and stop when a queen is found. Notice we can use 2 loops with (-1, 0, 1) to search on 8 directions.

   ```python
       def queensAttacktheKing(self, queens, king):
           """
           :type queens: List[List[int]]
           :type king: List[int]
           :rtype: List[List[int]]
           """
           result = []
           visited = [[False for _ in range(8)] for _ in range(8)]
           for q in queens:
               visited[q[0]][q[1]] = True
           dir = [-1, 0, 1]
           for dx in dir:
               for dy in dir:
                   if dx == 0 and dy == 0:
                       continue
                   x, y = king[0], king[1]
                   while (x+dx >=0 and x+dx <8 and y+dy >= 0 and y+dy <8):
                       x += dx
                       y += dy
                       if visited[x][y]:
                           result.append([x, y])
                           break
           return result
   ```
   
   
   
3. [Dice Roll Simulation][Problem3]

   The idea is from @lee215. We solve this problem with dynamic programming. Let dp\[i\]\[k\] means after several rolls, the number of sequence that ends with k times i.

   ```python
       def dieSimulator(self, n, rollMax):
           mod = 10**9 + 7
           dp = [[0, 1] + [0] * 15 for i in xrange(6)]
           for _ in xrange(n - 1):
               dp2 = [[0] * 17 for i in xrange(6)]
               for i in xrange(6):
                   for k in xrange(1, rollMax[i] + 1):
                       for j in xrange(6):
                           if i == j:
                               if k < rollMax[i]:
                                   dp2[j][k + 1] += dp[i][k] % mod
                           else:
                               dp2[j][1] += dp[i][k] % mod
               dp = dp2
           return sum(sum(row) for row in dp) % mod
   ```
   
     
   
4. [Maximum Equal Frequency][Problem4]

   I did not figure this problem out while contest. The idea is from @tylertay. There are only two cases in this problem. Firstly we count the frequency of the elements and the number of elements with that frequency. We need to update the result only when one of below cases happen:

   Case 1:
   frequency * (number of elements with that frequency) == length AND `i` != nums.length - 1
   E.g. `[1,1,2,2,3]`
   When the iteration is at index 3, the count will be equal to the length. It should update the result with (length + 1) as it should take an extra element in order to fulfil the condition.
   
   Case 2:
   frequency * (number of elements with that frequency) == length - 1
   E.g. `[1,1,1,2,2,3]`
   When the iteration is at index 4, the count will be equal to (length - 1). It should update the result with length as it fulfil the condition.
   
   The time complexity is `O(N)` 
   
   ``` python
   def maxEqualFreq(self, nums):
   	counts, freq = collections.Counter(), collections.Counter()
   	res = 0
   	for i, num in enumerate(nums):
   		# update counts
   		counts[num] += 1
   		# update counts with that frequency
   		freq[counts[num]] += 1
   
   		count = freq[counts[num]] * counts[num]
   		if count == i + 1 and i != len(nums) - 1: # case 1
   			res = max(res, i + 2)
   		elif count == i: # case 2
   			res = max(res, i + 1)
   	return res
   ```
   
   

[Problem1]: https://leetcode.com/contest/weekly-contest-158/problems/split-a-string-in-balanced-strings/
[Problem2]: https://leetcode.com/contest/weekly-contest-158/problems/queens-that-can-attack-the-king/
[Problem3]: https://leetcode.com/contest/weekly-contest-158/problems/dice-roll-simulation/
[Problem4]: https://leetcode.com/contest/weekly-contest-158/problems/maximum-equal-frequency/

