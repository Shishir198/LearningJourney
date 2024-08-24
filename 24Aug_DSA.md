Graph - Alien Dictionary (https://www.geeksforgeeks.org/problems/alien-dictionary/)

Today I solved this HARD problem , I got it right in 2nd attempts.


    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {
        
        int[] indegree = new int[V];
        int[] ans = new int[V];
        int ansIndex = 0;
        Queue<Integer> q = new LinkedList<>();
        for (int i=0;i<adj.size();i++){
            for(int j=0;j<adj.get(i).size();j++){
                indegree[adj.get(i).get(j)]+=1;
            }
        }
        for(int i=0;i<V;i++){
            if(indegree[i] == 0){
                q.add(i);
            }
        }
        
        while(!q.isEmpty()){
            int node = q.remove();
            ans[ansIndex++] = node;
            for(int i = 0 ;i < adj.get(node).size();i++){
                int childNode = adj.get(node).get(i);
                indegree[childNode]-=1;
                if(indegree[childNode] == 0){
                    q.add(childNode);
                }
                
                
            }
            
        }
        
        return ans;  
    }
    
    public void compareString(String a , String b , ArrayList<ArrayList<Integer>> adj){
        
        int index = 0;
        while(a.charAt(index) == b.charAt(index)){
            index++;
            if(index >= a.length() || index>=b.length()){
                return;
            }
        }
        int alpbhabetIndexa = (int)a.charAt(index) - 97;
        int alphabetIndexb = (int)b.charAt(index) - 97 ;
        
        adj.get(alpbhabetIndexa).add(alphabetIndexb);
    }
    
    public String findOrder(String[] dict, int n, int k) {
        
        ArrayList<ArrayList<Integer>> adj = new ArrayList<ArrayList<Integer>>();
        
        for(int i = 0;i<k;i++){
            adj.add(new ArrayList<Integer>());
        }
        
        for(int i=0;i<dict.length-1;i++){
            compareString(dict[i] , dict[i+1] , adj);
        }
        
        int[] integer_ans = topoSort(k , adj);
        
        String final_ans = "";
        
        for(int i=0;i<k;i++){
            int ch = integer_ans[i];
            final_ans += (char)(ch+97);
        }
        
        return final_ans;
        
        
    }





Next I attempted another HARD problem -  Parallel Courses 3 (https://leetcode.com/problems/parallel-courses-iii/)


I understood the logic and can apply it well in all cases.

but I got stuck in implementation part.

Below is not my solution , just pasted it for reference.

```
class Solution {
  public int minimumTime(int n, int[][] relations, int[] time) {
    List<Integer>[] graph = new List[n];
    int[] inDegrees = new int[n];
    int[] dist = time.clone();

    for (int i = 0; i < n; ++i)
      graph[i] = new ArrayList<>();

    // Build the graph.
    for (int[] r : relations) {
      final int u = r[0] - 1;
      final int v = r[1] - 1;
      graph[u].add(v);
      ++inDegrees[v];
    }

    // Perform topological sorting.
    Queue<Integer> q = IntStream.range(0, n)
                           .filter(i -> inDegrees[i] == 0)
                           .boxed()
                           .collect(Collectors.toCollection(ArrayDeque::new));

    while (!q.isEmpty()) {
      final int u = q.poll();
      for (final int v : graph[u]) {
        dist[v] = Math.max(dist[v], dist[u] + time[v]);
        if (--inDegrees[v] == 0)
          q.offer(v);
      }
    }

    return Arrays.stream(dist).max().getAsInt();
  }
}
```
