---
title:  "Leetcode BiWeekly Contest 10"
date:   2019-10-05 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Intersection of Three Sorted Arrays][Problem1] 
   
    We just convert three arrays into set and calculate the intersection of them and return the sorted result list. The time complexity is `O(N^2)` since we do intersection two times.
    
    ```python
        def arraysIntersection(self, arr1, arr2, arr3):
            """
            :type arr1: List[int]
            :type arr2: List[int]
            :type arr3: List[int]
            :rtype: List[int]
            """
            ans = set(arr1).intersection(set(arr2)).intersection(set(arr3))
            return sorted(list(ans))
    ```
    
     Since the original three arrays are sorted. We can have another `O(N)` time solution, which use pointers.
    
    ```python
        def arraysIntersection(self, arr1, arr2, arr3):
            """
            :type arr1: List[int]
            :type arr2: List[int]
            :type arr3: List[int]
            :rtype: List[int]
            """
            ans = []
            p1, p2, p3 = 0, 0, 0
            while p1 < len(arr1) and p2 < len(arr2) and p3 < len(arr3):
                minV = min(arr1[p1], arr2[p2], arr3[p3])
                # If minV are in all three arrays, we add it to the answer
                if minV == arr1[p1] == arr2[p2] == arr3[p3]:
                    ans.append(minV)
                # We moving the pointer if value in array equals minV
                if minV == arr1[p1]:
                    p1 += 1
                if minV == arr2[p2]:
                    p2 += 1
                if minV == arr3[p3]:
                    p3 += 1
            return ans
    ```
    
    
    
2. [Two Sum BSTs][Problem2]

   We use a set to remember all values in first tree. Then for a node with value v in second tree, we see if target-v in the set (which means in the first tree). If yes we return True, if not we continue find value in two subtree of current node. The time complexity is `O(N)` and space complexity is `O(N)`.

   ```python
       def twoSumBSTs(self, root1, root2, target):
           """
           :type root1: TreeNode
           :type root2: TreeNode
           :type target: int
           :rtype: bool
           """
           s = set()
           # Add all roo
           def dfs(root):
               if not root:
                   return
               s.add(root.val)
               dfs(root.left)
               dfs(root.right)
           dfs(root1)
           def find(root, target):
               if not root:
                   return False
               if target - root.val in s:
                   return True
               return find(root.left, target) or find(root.right, target)
           return find(root2, target)
   ```
   
   
   
3. [Stepping Numbers][Problem3]

   We can use Breadth First Search to solve this problem. In i-th round we maintain a queue which contains all valid stepping numbers in length i. In next round, we poll every number v from the queue and to see if v is between low and high. If yes, we append the value to answer. Then we append a last digit of v plus one or minus one to the end of v and add it to the new queue. The time complexity is `O(10*2^N)` where N is the length of high number.

   ```python
       def countSteppingNumbers(self, low, high):
           """
           :type low: int
           :type high: int
           :rtype: List[int]
           """
           ans = []
           cur = [0,1,2,3,4,5,6,7,8,9]
           si = -1
           while cur and (si < high):
               s = len(cur)
               temp = []
               for i in range(s):
                   top = cur.pop(0)
                   si = top
                   if low <= top <= high:
                       ans.append(top)
                   if si == 0:
                       continue
                   if si%10 != 0:
                       v = si*10+si%10-1
                       temp.append(v)
                   if si%10 != 9:
                       v = si*10+si%10+1
                       temp.append(v)
               cur = temp
           return ans
   ```
   
     
   
4. [Valid Palindrome III][Problem4]

   We can use dynamic programming to solve this problem. The idea is from the problem [Edit Distance](https://leetcode.com/problems/edit-distance). Let dp\[i\]\[j\] means the edit distance between first i letters substring and last j letters substring. Our goal is to calculate dp\[n\]\[n\] and to see if the value is smaller than 2k.

   The time and space complexity is `O(N^2)` 
   
   ``` python
       def isValidPalindrome(self, s, k):
           n = len(s)
           dp = [[0] * (n + 1) for _ in range(n + 1)] 
           for i in range(n + 1): 
               for j in range(n + 1): 
                   if not i or not j: 
                       dp[i][j] = i or j 
                   elif s[i - 1] == s[n - j]: 
                       dp[i][j] = dp[i - 1][j - 1] 
                   else: 
                       dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1])
           return dp[n][n] <= k * 2
   ```
   
   

[Problem1]: https://leetcode.com/contest/biweekly-contest-10/problems/intersection-of-three-sorted-arrays/
[Problem2]: https://leetcode.com/contest/biweekly-contest-10/problems/two-sum-bsts/
[Problem3]: https://leetcode.com/contest/biweekly-contest-10/problems/stepping-numbers/
[Problem4]: https://leetcode.com/contest/biweekly-contest-10/problems/valid-palindrome-iii/

