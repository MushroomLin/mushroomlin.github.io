---
title:  "Leetcode Weekly Contest 154"
date:   2019-09-15 00:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Maximum Number of Balloons][Problem1] 
   
    We use a map to count the number of occurrences of each letter in the text. The answer should be the minimum count of  'b', 'a', 'n' and half of 'l' and 'o'. The time complexity is `O(N)`.
    
    ```python
        def maxNumberOfBalloons(self, text):
            """
            :type text: str
            :rtype: int
            """
            dic = {}
            for i in text:
                if i not in dic:
                    dic[i] = 0
                dic[i] += 1
            for i in ['b', 'a', 'l', 'o', 'n']:
                if i not in dic:
                    return 0
            return min(dic['b'], dic['a'], dic['l']/2, dic['o']/2, dic['n'])
    ```
    
     
    
2. [Reverse Substrings Between Each Pair of Parentheses][Problem2]

   We can solve this problem with recursion. 

   Firstly we use count to remember current number of left "(". Every time we meet an "(", we increase the count by 1. 
   
   * If the "(" is the first one, we remember current index so later we can get the whole string inside the parentheses to do the recursion. 
   
   * If current character is ")", we decrease the count by 1. If after decreasing the count become zero, which means we are out of parentheses, we can then get the substring inside the parentheses by using current index and the first "(" index. We can then recursively call reverseParentheses() on the substring and append the reversed return string. 
   
   * If current count is 0, which means we are out of any parentheses currently, we can just append current character to the answer string. 
   * Finally we return the string we get. The time complexity is `O(N)`.
   
   ```python
       def reverseParentheses(self, s):
           """
           :type s: str
           :rtype: str
           """
           if not s:
               return s
           count = 0
           ans = ""
           l = -1
           for i in range(len(s)):
               if s[i] == "(":
                   count += 1
                   if count == 1: l = i
               elif s[i] == ")":
                   count -= 1
                   if count == 0:
                       ans += self.reverseParentheses(s[l+1:i])[::-1]
               elif count == 0:
                   ans += s[i]
           return ans
   ```
   
    
   
3. [K-Concatenation Maximum Sum][Problem3]

   This problem is a modified version of [Maximum Subarray][Maximum subarray] problem. Firstly, we define a function called maxSum(arr) which calculates the maximum subarray sum of the array. In this problem, we have only 3 cases: 

   * If k equals 0, we just return 0. 
   
   * If k equals 1, we can just use maxSum() to get the maximum subarray sum. 
   
   * If k greater than 1, there are two cases.
     * If the sum of the array is less than or equal to 0, we just need to run maxSum() on the array that repeat original array two times.
     * If the sum of the array is greater than 0, then we should append the subarray as long as possible. The answer should be the maxSum() of the array that repeat original array two times plus (k-2)*sum(array).
   
   The time complexity is `O(N)`
   
   ```python
       def kConcatenationMaxSum(self, arr, k):
           """
           :type arr: List[int]
           :type k: int
           :rtype: int
           """
           def maxSum(arr):
               ans = 0
               cur = 0
               for i in arr:
                   cur += i
                   ans = max(cur, ans)
                   if cur < 0:
                       cur = 0
               return ans
           if k == 0:
               return 0
           if k == 1:
               return maxSum(arr)
           total = sum(arr)
           if total < 0:
               return maxSum(arr+arr)
           else:
               return (maxSum(arr+arr)+(k-2)*total)%(10**9+7)
   ```
   
     
   
4. [Critical Connections in a Network][Problem4]

   This problem is a classical problem in graph theory called finding the strongly connected components of a directed graph. We can use [Tarjan's Algorithm][Tarjan's Algorithm] to solve this problem in linear time. 

   Firstly we loop all connections to get the map of out-edges for each edge. Then we define two arrays, one is called time, which remembers the time that the first time that the node i appears in our depth-first-search. The other called low, which remembers the lowest time of all nodes that connects to node i. 
   
   Next we run a DFS on all nodes and update the time array and low array. One edges is considered as an critical connections if and only we find an node v's neighbor i, that we have not visited i (This means in DFS we visit i after v) and after DFS low[i] is larger than time[v]. This means we do not have any "back edge" from i to v. So that the edge [v, i] must be a critical connection.
   
   ``` python
       def criticalConnections(self, n, connections):
           """
           :type n: int
           :type connections: List[List[int]]
           :rtype: List[List[int]]
           """
           dic = {}
           ans = []
           for c in connections:
               if c[0] not in dic:
                   dic[c[0]] = []
               if c[1] not in dic:
                   dic[c[1]] = []
               dic[c[0]].append(c[1])
               dic[c[1]].append(c[0])
           visited = set()
           time = [-1 for _ in range(n+1)]
           low = [-1 for _ in range(n+1)]
           self.t = 0
           def dfs(v, p):
               visited.add(v)
               time[v] = low[v] = self.t
               self.t += 1
               for i in dic[v]:
                   if i == p:
                       continue
                   if i in visited:
                       low[v] = min(low[v], time[i])
                   else:
                       dfs(i, v)
                       low[v] = min(low[v], low[i])
                       if low[i] > time[v]:
                           ans.append([v,i])
           for i in range(n):
               if i not in visited:
                   dfs(i, 0)
           return ans
   ```
   
   

[Problem1]: https://leetcode.com/contest/weekly-contest-154/problems/maximum-number-of-balloons/
[Problem2]: https://leetcode.com/contest/weekly-contest-154/problems/reverse-substrings-between-each-pair-of-parentheses/
[Problem3]: https://leetcode.com/contest/weekly-contest-154/problems/k-concatenation-maximum-sum/
[Problem4]: https://leetcode.com/contest/weekly-contest-154/problems/critical-connections-in-a-network/

[Tarjan's Algorithm]: https://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm
[Maximum subarray]: https://leetcode.com/problems/maximum-subarray/