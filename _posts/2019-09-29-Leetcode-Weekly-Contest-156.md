---
title:  "Leetcode BiWeekly Contest 9"
date:   2019-09-22 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Unique Number of Occurrences][Problem1] 
   
    Use a map to remember the number of occurrences of each number in the array. Then we use set to remove duplicate to see if all number of occurrences are unique. The time complexity is `O(N)`
    
    ```python
        def uniqueOccurrences(self, arr):
            """
            :type arr: List[int]
            :rtype: bool
            """
            dic = {}
            for i in arr:
                if i not in dic:
                    dic[i] = 0
                dic[i] += 1
            return len(dic.values()) == len(set(dic.values()))
    ```
    
     
    
2. [Get Equal Substrings Within Budget][Problem2]

   First we convert s and t into an array of absolute value difference for letters in each index. If every difference in the array is larger than maxCost, we return 0. Otherwise we can convert this problem into finding the max length of a subarray so that the sum of the subarray is smaller or equal to maxCost. We can use two pointer solve this problem. Every time we move right pointer, we sum current right value to total. If total is larger than maxCost, while sum larger than maxCost, we minus current left pointer value and increase left pointer by 1. Then we update the answer, answer is the max of answer and right-left+1. The time complexity is `O(N)` and space complexity is `O(N)`.

   ```python
       def equalSubstring(self, s, t, maxCost):
           """
           :type s: str
           :type t: str
           :type maxCost: int
           :rtype: int
           """
           arr = []
           n = len(s)
           # Convert s and t into an array of absolute value difference for letters in each index
           for i in range(n):
               arr.append(abs(ord(s[i])-ord(t[i])))
           if all([i > maxCost for i in arr]):
               return 0
           l = 0
           cur = 0
           ans = 0
           for r in range(n):
               cur += arr[r]
               if cur > maxCost:
                   while l < r and cur > maxCost:
                       cur -= arr[l]
                       l += 1
               ans = max(r-l+1, ans)
           return ans
   ```
   
   
   
3. [Remove All Adjacent Duplicates in String II][Problem3]

   We can use stack to solve this problem. We use (letter, count) pair to remember state of the stack. We always watch the top of the stack and count the number of occurrences of current letter. If the count equal to k, we just pop the top pair. The time complexity is `O(N)`

   ```python
       def removeDuplicates(self, s, k):
           """
           :type s: str
           :type k: int
           :rtype: str
           """
           stack = [['#', 0]]
           for c in s:
               if stack[-1][0] == c:
                   stack[-1][1] += 1
                   if stack[-1][1] == k:
                       stack.pop()
               else:
                   stack.append([c, 1])
           return ''.join(c * k for c, k in stack)
   ```
   
     
   
4. [Minimum Moves to Reach Target with Rotations][Problem4]

   We can use BFS to solve this problem. We maintain a queue, which each element in the queue represents the place that snake can arrive. We use level represents current steps. When we pop out a element represents the end, we output current steps as answer. The element consists of 4 values: ti: tail row, tj: tail column, hi: head row, hj: head column. Details are in the code:

   ``` python
       def minimumMoves(self, grid):
           """
           :type grid: List[List[int]]
           :rtype: int
           """
           n = len(grid)
           # (tail_row, tail_col, head_row, head_col)
           q = [(0,0,0,1)]
           visited = set()
           visited.add((0,0,0,1))
           # if start or end has 1, we can never arrive
           if grid[0][0] == 1 or grid[0][1] == 1:
               return -1
           if grid[n-1][n-1] == -1 or grid[n-1][n-2] == -1:
               return -1
           level = 0
           while q:
               size = len(q)
               # Pop all elements from current queue
               for _ in range(size):
                   cur = q.pop(0)
                   (ti, tj, hi, hj) = cur
                   # If current element is the end, we return current level
                   if ti == n-1 and tj == n-2 and hi == n-1 and hj == n-1 or ti == n-1 and tj == n-1 and hi == n-1 and hj == n-2:
                       return level
                   # Else we append any valid state to the queue
                   for (tii, tjj, hii, hjj) in [(ti+1, tj, hi+1, hj), (ti, tj+1, hi, hj+1)]:
                       # Move down and right
                       if 0<=tii<n and 0<=tjj<n and 0<=hii<n and 0<=hjj<n and grid[tii][tjj] == 0 and grid[hii][hjj] == 0 and (tii, tjj,hii, hjj) not in visited:
                               visited.add((tii, tjj, hii, hjj))
                               q.append((tii, tjj, hii, hjj))
                   # If horizontal, we can rotate clockwise
                   if ti == hi:
                       if ti+1<n and grid[ti+1][tj] == 0 and grid[hi+1][hj] == 0 and (ti, tj, hi+1, hj-1) not in visited:
                           visited.add((ti, tj, hi+1, hj-1))
                           q.append((ti, tj, hi+1, hj-1))
                   # Else it is vertical, we can rotate counterclockwise
                   else:
                       if tj+1<n and grid[ti][tj+1] == 0 and grid[hi][hj+1] == 0 and (ti, tj, hi-1, hj+1) not in visited:
                           visited.add((ti, tj, hi-1, hj+1))
                           q.append((ti, tj, hi-1, hj+1))
               level += 1
           return -1
   ```
   
   

[Problem1]: https://leetcode.com/contest/weekly-contest-156/problems/unique-number-of-occurrences/
[Problem2]: https://leetcode.com/contest/weekly-contest-156/problems/get-equal-substrings-within-budget/
[Problem3]: https://leetcode.com/contest/weekly-contest-156/problems/remove-all-adjacent-duplicates-in-string-ii/
[Problem4]: https://leetcode.com/contest/weekly-contest-156/problems/minimum-moves-to-reach-target-with-rotations/

