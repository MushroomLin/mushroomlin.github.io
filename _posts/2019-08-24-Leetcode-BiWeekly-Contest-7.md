---
title:  "Leetcode Biweekly Contest 7"
date:   2019-08-24 15:00:00
categories: [leetcode, algorithm]
tags: [leetcode, algorithm, weekly contest]

---

1. [Single-Row Keyboard][Problem1] 
   
    Firstly, we use an hashmap to remember each character's index, so we can get any character's index in O(1) time. Next start from the index 0, we iteratively sum the index difference between current character and next character, then we update current character. The time complexity is `O(n)`.
    
    ```python
        def calculateTime(self, keyboard, word):
            """
            :type keyboard: str
            :type word: str
            :rtype: int
            """
            # Use an hashmap to remember each character's index of the keyboard
            dic = {}
            for i, v in enumerate(keyboard):
                dic[v] = i
            if len(word)<=1:
                return 0
            # Iteratively get the index difference as the moving steps and sum the steps
            ans = 0
            cur = keyboard[0]
            for i in range(len(word)):
                ans += abs(dic[cur] - dic[word[i]])
                cur = word[i]
            return ans
    ```
    
     
    
2. [Design File System][Problem2]

   This problem can be solved using a special data structure called [Trie][TrieWiki]. The time complexity is `O(n)` for both create and get operation where n is the word length.

   ```python
   class FileSystem(object):
   
       def __init__(self):
           # Init the Trie root as a dict
           self.root = {}
   
       def create(self, path, value):
           """
           :type path: str
           :type value: int
           :rtype: bool
           """
           # Parse the path string
           arr = path[1:].split('/')
           cur = self.root
           # Iteratively find the next path
           for i, v in enumerate(arr):
               # If the path is the fianl path
               if i == len(arr)-1:
                   if v not in cur:
                       # Add the path and value into Trie
                       cur[v] = {}
                       cur[v]['#'] = value
                       return True
                   else:
                       return False
               # If the middle path does not exist, return False
               elif v not in cur:
                   return False
               else:
                   # Go to next level
                   cur = cur[v]
   
       def get(self, path):
           """
           :type path: str
           :rtype: int
           """
           # Parse the path string
           arr = path[1:].split('/')
           cur = self.root
           for i, v in enumerate(arr):
               # If path does not exist
               if v not in cur:
                   return -1
               else:
                   # Go to next level
                   cur = cur[v]
           return cur["#"]
   ```

    

3. [Minimum Cost to Connect Sticks][Problem3]

   This problem can be solved with greedy approach. In each round, we pick the shortest two sticks, connect them and put the stick back to original sticks. We do loop until we only have one stick. We can do this with the help of Java's PriorityQueue data structure. We loop for n-1 times for n sticks and each round we need to find the minimum two stick (with O(1) time) and insert the connected stick back to queue (with O(log(n)) time). So the final time complexity is `O(nlog(n))`.

   ```java
       public int connectSticks(int[] sticks) {
           int n = sticks.length;
           if (n <= 1) return 0;
           Queue<Integer> q = new PriorityQueue<>();
           // Add all sticks in priority queue
           for(int i=0; i<n; i++){
               q.offer(sticks[i]);
           }
           int ans = 0;
           while(q.size() > 1){
               // Get the shortest two sticks
               int t1 = q.poll();
               int t2 = q.poll();
               ans += t1 + t2;
               // Add the new stick back to queue
               q.offer(t1+t2);
           }
           return ans;
       }
   ```

     

4. [Optimize Water Distribution in a Village][Problem4]

   We can use [Union-Find][UFWiki] data algorithm to solve this problem. First we sort all edges according to the distance. Then for each edge, we check if the two village are already connected, if not and the cost to build the pipe is lower than the cost of building an well in any of the two villages, we connect the two village with the pipe. Notice that when we connect two villages, we choose the village with lower cost to build a well as the parent. After finish all process all edges, the root of each union group is the only village that need to build a well. Then we add the cost of building well in these root villages to the final answer. The time complexity is `O(ElogE)` and space complexity is `O(n)`

   ``` python
       def minCostToSupplyWater(self, n, wells, pipes):
           """
           :type n: int
           :type wells: List[int]
           :type pipes: List[List[int]]
           :rtype: int
           """
           def find(i):
               # Find the root village of current village
               while parent[i] != i:
                i = parent[i]
               return i
           def union(i, j, v):
               # Union two groups
               pi = find(i)
               pj = find(j)
               # If two villages are already connected, return False
               if pi == pj:
                   return False
               # If build wells cost in both villages are all less than build the pipe, return False
               if v > wells[pi-1] and v > wells[pj-1]:
                   return False
               # Connected the two villages with the pipe
               if wells[pi-1] < wells[pj-1]:
                   parent[pj] = pi
               else:
                   parent[pi] = pj
               return True
           parent = [i for i in range(n+1)]
           # Sort the edges with the distance
           pipes = sorted(pipes, key = lambda x: x[2])
           ans = 0
           # Connect the villages with pipes if necessary
           for pipe in pipes:
               if union(pipe[0],pipe[1],pipe[2]):
                   ans += pipe[2]
           # Add the cost to build well for root of each group
           for i in range(1, n+1):
               if parent[i] == i:
                   ans += wells[i-1]
           return ans
   ```
   
   

[Problem1]: https://leetcode.com/contest/biweekly-contest-7/problems/single-row-keyboard/
[Problem2]: https://leetcode.com/contest/biweekly-contest-7/problems/design-file-system/
[Problem3]: https://leetcode.com/contest/biweekly-contest-7/problems/minimum-cost-to-connect-sticks/
[Problem4]: https://leetcode.com/contest/biweekly-contest-7/problems/optimize-water-distribution-in-a-village/
[TrieWiki]: https://en.wikipedia.org/wiki/Trie
[UFWiki]: https://en.wikipedia.org/wiki/Disjoint-set_data_structure
