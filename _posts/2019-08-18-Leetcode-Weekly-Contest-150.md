---
title:  "Leetcode Weekly Contest 150"
date:   2019-08-18 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Find Words That Can Be Formed by Characters][Problem1] 
    
    For this problem, we can simply use a hashmap to count the number of each character appears in chars. For each word in words, we check if the number of each character appears in word in less than or equal to the number in original chars. Let n be the number of word in words and m be the length of each word. The time complexity is `O(mn)` and space complexity is `O(26) = O(1)`.
    
    ```python
          def countCharacters(self, words, chars):
              """
              :type words: List[str]
              :type chars: str
              :rtype: int
              """
              # Use chars_map to count the number of each char appears in chars
              chars_map = {}
              for c in chars:
                  if c not in chars_map:
                      chars_map[c] = 0
                  chars_map[c] += 1
              ans = 0
              # For each word in words, we compare the number of each character 
              for word in words:
                  word_map = {}
                  for c in word:
                      if c not in word_map:
                          word_map[c] = 0
                      word_map[c] += 1
                      flag = 0
                      # early stop
                      if c not in chars_map or word_map[c] > chars_map[c]:
                          flag = 1
                          break
                  if not flag:
                      ans += len(word)
              return ans
    ```
    
    

2. [Maximum Level Sum of a Binary Tree][Problem2]

   We can use recursion to traverse the binary tree by level and use an hashmap to remember sum of each level. We only need to traverse each tree node once, so the time complexity is `O(n)`. The space complexity is `O(level)`= `O(lgn)` .

   ```python
       def maxLevelSum(self, root):
           """
           :type root: TreeNode
           :rtype: int
           """
           # Use a hashmap to remember the sum of each level
           dic = {}
           # Define the helper function to traverse the tree recursively
           def helper(root, level):
               if not root:
                   return
               if level not in dic:
                   dic[level] = 0
               dic[level] += root.val
               helper(root.left, level+1)
               helper(root.right, level+1)
           helper(root, 1)
           v = max(dic.values())
           ans = float('inf')
           for k in dic.keys():
               if dic[k] == v:
                   ans = min(ans, k)
           return ans
   ```

3. [As Far From Land as Possible][Problem3]

   This is an simple BFS problem. Firstly we add all the land to an queue and set current level to be 0. Then we run breadth first search iteratively. In each round, we pop all the cells from the queue and add the neighbor cells to the queue if cell has not been visited. Then we increase the level by 1. Finally when the queue is empty, which means all the cells are visited, we can got the level-1 to be maximum distance of the water cell from any land cell since it is last one to be visited.

   The time complexity is `O(n)` since we at most visited each cell once. The space complexity is also `O(n)`.

   ```python
       def maxDistance(self, grid):
           """
           :type grid: List[List[int]]
           :rtype: int
           """
           r = len(grid)
           c = len(grid[0])
           q = []
           visited = set()
           # Add all land cell to the queue
           for i in range(r):
               for j in range(c):
                   if grid[i][j] == 1:
                       q.append((i, j))
                       visited.add((i,j))
           level = 0
           # BFS by level of all cells
           while q:
               size = len(q)
               for _ in range(size):
                   i, j = q.pop(0)
                   for (ii, jj) in [(i+1, j), (i-1, j), (i, j+1), (i, j-1)]:
                       if ii<0 or ii>=r or jj<0 or jj>=c or (ii, jj) in visited:
                           continue
                       q.append((ii, jj))
                       visited.add((ii, jj))
               level += 1
           return level-1 if level-1 >0 else -1 
   ```

4. [Last Substring in Lexicographical Order][Problem4]

   According to the problem description, it is easy to get that the answer should be an suffix of original string s. Because if not we can always add all chars after the substring to it to get a larger substring. Then we can brute force all suffix and compare to get a largest substring.

   ```python
       def lastSubstring(self, s):
           """
           :type s: str
           :rtype: str
           """
           ans = ""
           for i in range(len(s)):
               ans = max(s[i:], ans)
           return ans
   ```

   The time complexity of above answer is `O(n^2)`. When using Python3 we can luckily get accepted. Can we use any algorithm to optimize the time complexity? 

   Consider above code, in each round we compare current suffix of `s` with the current maximum substring. However, in fact we do not need to compare the two whole string. Instead, we only need to compare the part that current suffix string minus current maximum substring. We choose the smaller length of the part and current maximum, and compare char by char. If this part is larger than or equal to current maximum substring, then this suffix must be the new maximum.

   For example, we have string s to be `bbab`:

   Round 1 we have first suffix to be `b`, and the maximum should also be `b`.

   Round 2 we compare second suffix `ab` with current maximum `b`, we only need to compare `a` with `b`. Since `a`<`b`, we have maximums still be `b`.

   Round 3 we compare third suffix `bab` with current maximum `b`, we only need to compare `ba` with `b`. Since the min length of the two is 1, we only compare first char. Since `b `= `b`, we have current maximum to be `bab`.

   Round 4 we compare last suffix `bbab` with current maximum `bab`, we only need to compare `b` with `bab`. Since the min length of the two is 1, we only compare first char. Since `b`=`b`, we have current maximum to be `bbab`.

   ```python
       def lastSubstring(self, s):
           """
           :type s: str
           :rtype: str
           """
           n = len(s)
           max_j = n - 1
           for j in range(n - 2, -1, -1):
               # if current char is smaller than maximum, continue to next suffix
               if s[j] < s[max_j]:
                   continue
               # if current char is larger than maximum, replace the index of maximum
               elif s[j] > s[max_j]:
                   max_j = j
               else:
                   k = 0
                   # Compare the larger part char by char
                   # Only need to compare the diffence part.
                   while k < min(max_j - j, n - max_j):
                       if s[j + k] > s[max_j + k]:
                           max_j = j
                           break
                       if s[j + k] < s[max_j + k]:
                           break
                       k += 1
                   else:
                       max_j = j
   
           return s[max_j:]
   ```

   

[Problem1]: https://leetcode.com/contest/weekly-contest-150/problems/find-words-that-can-be-formed-by-characters/
[Problem2]: https://leetcode.com/contest/weekly-contest-150/problems/maximum-level-sum-of-a-binary-tree/
[Problem3]: https://leetcode.com/contest/weekly-contest-150/problems/as-far-from-land-as-possible/
[Problem4]: https://leetcode.com/contest/weekly-contest-150/problems/last-substring-in-lexicographical-order/

