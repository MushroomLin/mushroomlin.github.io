---
title:  "Leetcode BiWeekly Contest 9"
date:   2019-09-22 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [How Many Apples Can You Put into the Basket][Problem1] 
   
    Sort the array and greedily choose the minimum apple until we can't. The time complexity is `O(N)`.
    
    ```python
        def maxNumberOfApples(self, arr):
            """
            :type arr: List[int]
            :rtype: int
            """
            arr.sort()
            i = 0
            cur = 0
            if sum(arr) <= 5000:
                return len(arr)
            while i < len(arr) and cur <= 5000:
                cur += arr[i]
                i += 1
            return i - 1
    ```
    
     
    
2. [Minimum Knight Moves][Problem2]

   We can solve this problem with BFS.  we start from a queue with only (0,0) in it. Each round we pop every cell from queue (since they are at the same distance from (0,0)). If any of the cell matches the goal, we return the result, else we append their 8 neighbors to the queue if the cell has not been visited.  Noticed since the board is symmetric in 4 quadrants, we can do pruning which limits all cells append to queue follows -5<=xx<=302 and -5<=yy<=302. The time complexity is `O(N)`.

   ```python
       def minKnightMoves(self, x, y):
           """
           :type x: int
           :type y: int
           :rtype: int
           """
           ansx = abs(x)
           ansy = abs(y)
           visited = set()
           visited.add((0,0))
           q = [(0,0)]
           ans = 0
           while q:
               s = len(q)
               for i in range(s):
                   top = q.pop(0)
                   if top[0] == ansx and top[1] == ansy:
                       return ans
                   x, y = top[0], top[1]
                   for (xx, yy) in [(x+1,y+2), (x+1, y-2), (x-1, y+2), (x-1, y-2), (x+2, y+1), (x+2, y-1), (x-2, y+1), (x-2, y-1)]:
                       if -5 <= xx <=302 and -5 <= yy <= 302 and (xx, yy) not in visited:
                           visited.add((xx, yy))
                           q.append((xx, yy))
               ans += 1
           return -1
   ```
   
    We can have another solution using memory and recursion. The idea is from @[824728350](https://leetcode.com/824728350)'s post. We consider 4 quadrants are symmetric, so we can only consider one quadrants. There are 3 special cases: (0, 0) needs 0 steps, (0, 1) and (1, 0) needs 3 steps ((0, 0) -> (1, 2) -> (-1, 1) -> (1, 0)), others can be derived from these solutions with dynamic formula: 
   $$
   dp[x, y] = min(dp[abs(x-1), abs(y-2)], dp[abs(x-2), abs(y-1)])+1
   $$
   We can use memory (or cache) to remember some intermediate result to avoid repeat calculations. 
   
   ```python
       def minKnightMoves(self, x, y):
           """
           :type x: int
           :type y: int
           :rtype: int
           """
           memo = {(0, 0): 0, (1, 0): 3, (0, 1): 3}
           def helper(x, y):
               if (x, y) in memo: return memo[(x, y)]
               res = min(helper(abs(x-1), abs(y-2)),helper(abs(x-2), abs(y-1))) + 1
               memo[(x, y)] = res
               return res
           return helper(abs(x), abs(y))
   ```
   
   
   
3. [Find Smallest Common Element in All Rows][Problem3]

   We loop the number from first row. For each number, we binary search if the number is in the row. If yes, we search in next row. If not, we break and choose next number in first row. We repeat this procedure until we find a number in first row that appears in every row, then it is the solution. If none of number in first row appears in every row, we just return -1. 

   The time complexity is `O(NMlogM)`
   
   ```python
       def smallestCommonElement(self, mat):
           """
           :type mat: List[List[int]]
           :rtype: int
           """
           def bs(arr, target):
               l = 0
               r = len(arr)-1
               while l <= r:
                   m = (r-l)/2 + l
                   if arr[m] == target:
                       return True
                   elif arr[m] < target:
                       l = m + 1
                   else:
                       r = m - 1
               return False
           if not mat:
               return -1
           for i in mat[0]:
               f = 0
               for j in range(1, len(mat)):
                   if not bs(mat[j], i):
                       f = 1
                       break
               if not f:
                   return i
           return -1
   ```
   
     
   
4. [Minimum Time to Build Blocks][Problem4]

   We can use a PriorityQueue (or heapq in Python) to solve this problem. Firstly, we insert convert blocks array into a heapq which always keep the increasing order when we insert/pop value. For every round, we pop the first two elements and push back the larger one plus the time of split. This is the idea of Huffman's Algorithm to building a tree with lowest height. Each split we combine two subtree into one and until we get only one tree. We solve this with heapq and the time complexity is `O(NlogN)`.

   ``` python
       def minBuildTime(self, blocks, split):
           """
           :type blocks: List[int]
           :type split: int
           :rtype: int
           """
           heapq.heapify(blocks)
           while len(blocks) >= 2:
               a = heapq.heappop(blocks)
               b = heapq.heappop(blocks)
               heapq.heappush(blocks, max(a,b) + split)
           return heapq.heappop(blocks)
   ```
   
   

[Problem1]: https://leetcode.com/contest/biweekly-contest-9/problems/how-many-apples-can-you-put-into-the-basket/
[Problem2]: https://leetcode.com/contest/biweekly-contest-9/problems/minimum-knight-moves/
[Problem3]: https://leetcode.com/contest/biweekly-contest-9/problems/find-smallest-common-element-in-all-rows/
[Problem4]: https://leetcode.com/contest/biweekly-contest-9/problems/minimum-time-to-build-blocks/

