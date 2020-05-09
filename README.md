# Course Schedule II
## https://leetcode.com/problems/course-schedule-ii

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.
```
Example 1:

Input: 2, [[1,0]] 
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .
Example 2:

Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
```
**Note:**

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.

# Implementation : BFS
```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] indegree = new int[numCourses];
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        int[] topologicalOrder = new int[numCourses];
        
        // Create the adjacency list representation of the graph
        for (int i = 0; i < prerequisites.length; i++) {
          int dest = prerequisites[i][0];
          indegree[dest]++;
            
          int src = prerequisites[i][1];
          Set<Integer> neighbors = graph.getOrDefault(src, new HashSet<Integer>());
          neighbors.add(dest);
            
          graph.put(src, neighbors);
        }
        
        // Add all vertices with 0 in-degree to the queue
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < indegree.length; i++) {
          if (indegree[i] == 0) 
             q.add(i);
        }
        
        int i = 0;
        while (!q.isEmpty()) {
          int node = q.remove();
          topologicalOrder[i++] = node;
          // Decrement the in-degree of each neighbor of node by 1
          if (graph.containsKey(node)) {
            for(Integer neighbor : graph.get(node)) {
                 indegree[neighbor]--;

              // If in-degree of a neighbor becomes 0, add it to the queue
              if (indegree[neighbor] == 0) {
                q.add(neighbor);
              }
            }
          }
        }
        
        return i == numCourses ? topologicalOrder : new int[0];
    }
}
```

# References :
1. https://leetcode.com/articles/course-schedule-ii
2. https://www.youtube.com/watch?v=TJkYn3oqX0k
