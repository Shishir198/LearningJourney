DSA :

Solved : Detect Cycle in an Undirected Graph | BFS + DFS

How to think : 
(My first unsuccessfull attempt)

- if graph has n vertices then edges should less than n-1 , so that it doesn't have cycles.(Tree property).
- if edges becomes equal to n or more , then definitely there will be a cycle.
- Few test cases passed , rest failed. Why?
  I didnt consider cases for disconnected graphs , the above property hold true only for graph which is connected i.e (every vertices has path to any other vertices)

- so for disconnected graphs we have statregy to detect cycles using DFS and BFS , I personally favour DFS as its easy to implement ans seems straight forward in this case.
- Traverse using DFS/BFS and if we encounter already visited node again , that means we found a cycle ..
- Wait ! one twist . Here we have undirected graph , so in adjacency list we would store e.g :

  {{1}, {0, 2, 4}, {1, 3}, {2, 4}, {1, 3}} 

  now if we start with 0 (mark 0 as visited) and get to 1 (mark 1 as visited) now when checking 1's edges where to go next we again encounter 0 (already visited node).
  Does that means we hit a cycle !! HEll no!! 
  ![image](https://github.com/user-attachments/assets/265e5d93-0596-49a7-b219-e9a6cd89a2e9)

  its just an edge , so we also have to keep a mark that the new node which found already visited , should not be previous node or parent node.

  Now that completes the logic , if we hit a already visited node and its not previous node , Yeah! we encounter a cycle then.

  My complete solution :
  {
   public boolean detectCycle(int[] visited , int curr , int prev ,ArrayList<ArrayList<Integer>> adj ){

        visited[curr] =1;
        
        for(int i=0;i<adj.get(curr).size();i++){
            
            int connectedNode = adj.get(curr).get(i);

            
            if(visited[connectedNode] ==1 && prev!=connectedNode) {
                return true;
            }
            
            if(visited[connectedNode] == 0 && detectCycle(visited , connectedNode , curr ,adj )){
                return true;
            }
            
        }
        return false;
        
     }
    }
    
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
        
        int[] visited = new int[adj.size()];
        
        for(int i=0;i<adj.size();i++){
            if(visited[i]==0){
                if(detectCycle(visited , i , -1 ,adj)){
                    return true;
                }
            }
        }
        return false;
        
    }

    --------------------------------------------------------------------------------------------------------------------------

  Why are we doing this - 
      for(int i=0;i<adj.size();i++){
            if(visited[i]==0){
                if(detectCycle(visited , i , -1 ,adj)){
                    return true;
                }
            }
        }
        return false;

  and not only
  {
    return detectCycle(visited , 0 , -1 , adj)
  }


  because to handle disconnected graphs .

  If we not have a loop , we only start with a particular node i.e 0 and check its edges is there is cycle , but it can be only a component of graph ,
   may be we never check other nodes which were disconnected with vertex 0 for the cycle.

  ![image](https://github.com/user-attachments/assets/0a54ce28-7929-4cc1-84a9-a7094752efd1)

  So we allow a loop , to check on the component that is not already visited to make sure we are checking all the nodes and components for cycle.


  

  
  


