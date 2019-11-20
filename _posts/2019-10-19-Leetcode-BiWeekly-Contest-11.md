---
title:  "Leetcode BiWeekly Contest 11"
date:   2019-10-19 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Missing Number In Arithmetic Progression][Problem1] 
   
    Just search if there is any neighbor value difference is not equal to the difference of others. The time complexity is `O(N)`.
    
    ```python
        def missingNumber(self, arr):
            """
            :type arr: List[int]
            :rtype: int
            """
            diff = (arr[-1] - arr[0])/(len(arr))
            for i in range(1, len(arr)):
                if arr[i] - arr[i-1] != diff:
                    return arr[i] - diff
            return arr[0]
    ```
    
     
    
2. [Meeting Scheduler][Problem2]

   Firstly we sort all slots with start time. Then we find the common time slots of two people and add them into an array. Then we iteratively find if the common slots is longer than duration and return the first slots that is greater than duration. The time complexity is `O(NlogN)` since we use sort.

   ```python
       def minAvailableDuration(self, slots1, slots2, duration):
           """
           :type slots1: List[List[int]]
           :type slots2: List[List[int]]
           :type duration: int
           :rtype: List[int]
           """
           slots1 = sorted(slots1, key = lambda x: x[0])
           slots2 = sorted(slots2, key = lambda x: x[0])
           common = []
           i = 0
           j = 0
           while i < len(slots1) and j < len(slots2):
               cur = [max(slots1[i][0], slots2[j][0]), min(slots1[i][1], slots2[j][1])]
               common.append(cur)
               if slots1[i][1] == cur[1]:
                   i += 1
               if slots2[j][1] == cur[1]:
                   j += 1
           for c in common:
               if c[1]-c[0] >= duration:
                   return [c[0], c[0]+duration]
           return []
   ```
   
   
   
3. [Toss Strange Coins][Problem3]

   We use memorization with dynamic programming to solve this problem. helper(i, n) means the probability that we toss i times and have n heads in result. Then we can easily get the state equations:

   **helper(i, n) = helper(i+1, n)\*(1-prob[i])+helper(i+1, n+1)\*prob[i]**
   
   ```python
   from functools import lru_cache
   class Solution(object):
       def probabilityOfHeads(self, prob, target):
           """
           :type prob: List[float]
           :type target: int
           :rtype: float
           """
           prob.sort()
           @lru_cache(None)
           def helper(i, n):
               if n == target:
                   k = 1
                   for v in prob[i:]:
                       k *= (1-v)
                   return k
               if i == len(prob):
                   return 0
               return helper(i+1, n)*(1-prob[i])+helper(i+1, n+1)*prob[i]
           return helper(0,0)
   ```
   
     
   
4. [Divide Chocolate][Problem4]

   We can use binary search to solve this problem. Firstly we randomly guess a value between 0 and sum(a) and using test function to test if this answer can be achieved. We continue searching this answer until we have left equals right. Then we get the final answer.

   The time complexity is `O(NlogN)` 
   
   ``` python
       def maximizeSweetness(self, a, K):
           """
           :type sweetness: List[int]
           :type K: int
           :rtype: int
           """
           l, r = 0, sum(a)
           def test(v):
               cur, cnt = 0, 0
               for i in a:
                   cur += i
                   if cur >= v:
                       cur = 0
                       cnt += 1
               return cnt >= K+1
           while l < r :
               m = (r-l)/2 + l
               if l == m:
                   break
               if test(m):
                   l = m
               else:
                   r = m - 1
           return r if test(r) else l
   ```
   
   

[Problem1]: https://leetcode.com/contest/biweekly-contest-11/problems/missing-number-in-arithmetic-progression/
[Problem2]: https://leetcode.com/contest/biweekly-contest-11/problems/meeting-scheduler/
[Problem3]: https://leetcode.com/contest/biweekly-contest-11/problems/toss-strange-coins/
[Problem4]: https://leetcode.com/contest/biweekly-contest-11/problems/divide-chocolate/

