---
title:  "Leetcode Biweekly Contest 7"
date:   2019-09-07 15:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Count Substrings with Only One Distinct Letter][Problem1] 
   
    From the start, we count the length of substrings that only contains one letter. For each such substring of length k, we can parse it into (k+1)*k/2 goal substrings.  The time complexity is `O(n)`.
    
    ```python
        def countLetters(self, s):
            """
            :type S: str
            :rtype: int
            """
            if not s:
                return 0
            cur = s[0]
            count = 0
            ans = 0
            for i in range(len(s)):
                if s[i] != cur:
                    ans += (count+1)*count/2
                    count = 0
                    cur = s[i]
                count += 1
            ans += (count+1)*count/2
            return ans
    ```
    
     
    
2. [Before and After Puzzle][Problem2]

   We can solve this problem by directly loop the phrases to get all pairs and compare their first and last word. If they match, we add the combined string answer to the set to remove duplicate. Finally we sort the answer. The time complexity is `O(kn^2)` where k is the length of the word and n is number of phrases.

   ```python
       def beforeAndAfterPuzzles(self, phrases):
           """
           :type phrases: List[str]
           :rtype: List[str]
           """
           phrases = [i.split() for i in phrases]
           phrases.sort()
           n = len(phrases)
           ans = []
           for i in range(n):
               for j in range(i+1, n):
                   w1 = phrases[i]
                   w2 = phrases[j]
                   if w1[-1] == w2[0]:
                       ans.append(" ".join(w1+w2[1:]))
                   if w1[0] == w2[-1]:
                       ans.append(" ".join(w2+w1[1:]))
           return sorted(list(set(ans)))
   ```
   
    
   
3. [Shortest Distance to Target Color][Problem3]

   Firstly for each color, we build an array to remember each occurrence index in colors. Then for each query, we binary search the index in target color array and get the position that the index should appear if we want insert this index into the array. If the index appears in the array, we add 0 to answer array, else we calculate the min difference between the index and its left and right index and add to answer array.

   ```python
       def shortestDistanceColor(self, colors, queries):
           """
           :type colors: List[int]
           :type queries: List[List[int]]
           :rtype: List[int]
           """
           # Build 3 arrays to remember each occurence index
           dic = {}
           for i, v in enumerate(colors):
               if v not in dic:
                   dic[v] = []
               dic[v].append(i)
           ans = []
           for q in queries:
               index = q[0]
               color = q[1]
               if color not in dic:
                   ans.append(-1)
                   continue
               # Do binary search
               arr = dic[color]
               l = 0
               r = len(arr)-1
               flag = 0
               while l <= r:
                   m = (r-l)/2+l
                   if arr[m] == index:
                       ans.append(0)
                       flag = 1
                       break
                   elif arr[m] < index:
                       l = m + 1
                   else:
                       r = m - 1
               # Calculate index difference of the index with left and right index
               if not flag:
                   if l == 0:
                       ans.append(abs(arr[l]-index))
                   elif l == len(arr):
                       ans.append(abs(arr[l-1]-index))
                   else:
                       ans.append(min(abs(arr[l]-index), abs(arr[l-1]-index)))
           return ans
   ```

     

4. [Maximum Number of Ones][Problem4]

   This answer is originally from `@veraci  ` 's idea.  Firstly we define an K\*K size array. The R\*C matrix is just a repeat of K\*K array. For each position in R*C matrix, we get corresponding position in K\*K array with formula `code = (r%K) * K + c%K` and add 1 to the position. After this step, we get an array that means if we choose to add a one in this position, how many ones in total can we put in the whole R\*C matrix. Then we sort the count array and sum up top `maxOnes` position. The time complexity of this algorithm is `max(O(RC), O(k^2logk^2))`. The time complexity is `O(K^2)`.

   ``` python
       def maximumNumberOfOnes(self, C, R, K, maxOnes):
           """
           :type C: int
           :type R: int
           :type K: int
           :type maxOnes: int
           :rtype: int
           """
           # every K*K square has at most maxOnes ones
           count = [[0] * (K*K)]
           for r in xrange(R):
               for c in xrange(C):
                   code = (r%K) * K + c%K
                   count[code] += 1
           count.sort()
           ans = 0
           for _ in xrange(maxOnes):
               ans += count.pop()
           return ans
   ```
   
   

[Problem1]: https://leetcode.com/contest/biweekly-contest-8/problems/count-substrings-with-only-one-distinct-letter/
[Problem2]: https://leetcode.com/contest/biweekly-contest-8/problems/before-and-after-puzzle/
[Problem3]: https://leetcode.com/contest/biweekly-contest-8/problems/shortest-distance-to-target-color/
[Problem4]: https://leetcode.com/contest/biweekly-contest-8/problems/maximum-number-of-ones/
