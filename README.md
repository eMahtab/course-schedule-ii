# Course Schedule II
## https://leetcode.com/problems/course-schedule-ii

# Implementation :
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
