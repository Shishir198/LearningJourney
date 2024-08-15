DSA : 
Credits  : Rohit Negi , Coder Army .

Detect Cycle in a directed graph. 

Applications - it is used in OS , to detect deadlock in processes .

if p1 hold r1 , p2 hold r2 . Now , p1 wants r2 and p2 wants r1 . Theres a dilemma , P1 says first get me r2 then I will let r1 , P2 says first get me r1 then will let r2 . 
So , dillema , no progress | stucked.

![image](https://github.com/user-attachments/assets/5d87fefc-87d3-4ddf-b11c-cd1fb627ae5d)

As we have directed , we dont need to keep track of parent node .
Mark visited as 0 when coming out  , as we only have to keep track of nodes visited in current path only

Used dp for optimization.
    


USING  DFS:

    public static boolean cycle = false;
    
    public void dfs(int node , int[] visited , ArrayList<ArrayList<Integer>> adj , int[] dp){
        
        if(dp[node]==1){
            return;
        }
        
        visited[node] = 1;
        
        for(int i=0;i<adj.get(node).size();i++){
            if(visited[adj.get(node).get(i)] == 0 && cycle==false){
                dfs(adj.get(node).get(i) , visited , adj ,dp);
            }
            else{
                cycle = true;
                return;
            }
        }
        visited[node] = 0;
        dp[node] =1;
    }
    
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        
        int[] visited = new int[V];
        int[] dp = new int[V];
        
        for(int i=0;i<V;i++){
            if(visited[i]==0 && cycle==false){
                dfs(i , visited , adj,dp);
                dp[i] = 1;
            }
        }
        boolean ans = cycle;
        cycle = false;
        return ans;
        
    }




Using BFS : kahns algo.

We know we can only create topo sort of directed acylic graph . WHat if we run topo sort in direcetd cyclic graph.

result -> All node will not get picked / printed on traversal , as we only add node to queue whose indegree becomes 0.

If a graph has cycle , all nodes indegree will be alteast 2 , Therefore it will never get inside queue .

We can use this result to detct cycle using BFS.

USING BFS : 

    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
    
        int[] indegree = new int[V];
        int count = 0;
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
            count+=1;
            for(int i = 0 ;i < adj.get(node).size();i++){
                int childNode = adj.get(node).get(i);
                indegree[childNode]-=1;
                if(indegree[childNode] == 0){
                    q.add(childNode);
                }
            }
            
        }
        
        return !(count==V);
    }
