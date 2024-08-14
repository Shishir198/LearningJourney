DSA :

Topological sort in graph .

It gives a linear ordering of the nodes , in which the nodes needs to be traversed such that for each directed edge u-->v , u comes first then v.  

If we use analogy  , for u-->v means 

-in order to reach V we have to reach U first .
  Topological sort - will give ordering which nodes to reach first so all the nodes can be reached further without getting stuck.

-in order to complete task 'v' , 'u' needs to be completed first.
  Topological sort - will give ordering which tasks to complete first in order all the other tasks can be completed .

  E.g- 
![image](https://github.com/user-attachments/assets/f046578d-7da2-4f59-a6aa-b1aab97b5719)

One possible Topological order for the
graph is 3, 2, 1, 0.

0 is dependent in 1,2,3 i.e,  1,2,3 should be (completed/visited) before 0.

Its one of possiblity - there are other valid ans like - 1,2,3,0 or 2,3,1,0.

![image](https://github.com/user-attachments/assets/ad48a991-0643-4181-951b-b14f38a05aea)

One possible Topological order for the graph is 5, 4, 2, 1, 3, 0.


Point to be notes  : The graph should always be a directed acyclic graph for topo sort .
why ? 
Directed - If it would be undirected - we cannot determine which node should come first . if and edge between 0 -- 1 ? 
            whats the priortity? who is dependent on whom?  0 is dependent in 1 or 1 is dependent on 0 .

Acyclic - if it has cycle 
          ![image](https://github.com/user-attachments/assets/d62e2cae-e7d0-456f-a546-0f27aa4b40fa)
          it will state 0 should come before 1 , 1 should come before 2 and then again 2 wants to come before 0 . How it will possible ? 



If we want to traverse all nodes we need DFS or BFS , 

with DFS I did :

{Currently feeeling lazy to write the whole HOW to think, will add it later}

DFS Logic : 
  
    static int dfs(Stack s , int[] visited , int node ,ArrayList<ArrayList<Integer>> adj){
        visited[node] = 1;
        
        for(Integer it: adj.get(node))
        {
            if(visited[it]==0)
            {
               int x= dfs(s,visited,it,adj);
               s.push(x);
            }
        }
        
        
        return node;
        
    }
 
    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) 
    {
        int[] visited = new int[V];
        Stack<Integer> s = new Stack<>();
        
        for(int i=0;i<V;i++){
            if(visited[i] == 0){
                s.push(dfs(s, visited, i , adj));
            }
        }
        
        int[] ans = new int[V];
        int index =0;
        
        while(!s.empty()){
            ans[index++]=s.pop();
        }
        
        return ans;
        
    }




BFS (Kahns algorithm)

How to think  : That node will come first which has indegree 0 , i.e no incoming edges .i.e , not dependent on anyone.
create an array indegree: which will store count of incoming edges for each node.
So try to find node which has 0 indegree , and remove the edge(decrement the countof indegree) going from node to its children .

Then again find 0 indegree nodes and do the same  .

 
        
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








