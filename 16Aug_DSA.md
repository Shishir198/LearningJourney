DSA : 
Credits - Rohit Negi | Coder Army .


Proeblem - Detect Bipartite graph

-Its a graph where you can divide all the vertices of graph into 2 groups , such that no edges should be present within nodes of same group .
Examples - 
- ![image](https://github.com/user-attachments/assets/9d14f8ac-d125-4035-ac2e-9c27b6791829)
- group 1              group2
- 1                      0,2

  There is no edge between 0 and 2 so it can be in one group .

  ![image](https://github.com/user-attachments/assets/704e6954-5ecf-40cd-9449-9d038b937bad)
- group 1              group2
- 0 ,1                 2 ,

Now where to put 3 ?  it cannot sit in group 1 because 0 is connected to it , neither in group 2 as 2 is connected to it .
SO we cannot divide all vertices into 2 groups - not a bipartite graph.


Strategy 1 - , bipartite graph doesnt exists if graph has cycle of odd length , we can detect if any cycle in graph is odd length - not a bipartite graph.



Startegy 2 - brute force (2 colorable algorithm)

Assign colors to each vertices such that no adjacent nodes should have same colour , if we are unable to do that - not a bipartite graph.


Code with BFS :

    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj){
    
        int[] color = new int[V];
        for(int i = 0;i<V;i++){
            color[i] = -1;
        }
        Queue<Integer> q = new LinkedList<>();
        
        for(int i=0;i<V;i++){
            if(color[i]==-1){
                q.add(i);
                color[i] = 0;
                
                while(!q.isEmpty()){
                    int currNode = q.remove();
                    for(int j = 0;j<adj.get(currNode).size();j++){
                        int connNode = adj.get(currNode).get(j);
                        
                        if(color[connNode] == -1){
                            q.add(connNode);
                            color[connNode] = color[currNode]==0 ? 1 : 0;
                        }
                        else if(color[connNode] == color[currNode]){
                            return false;
                        }
                    }
                }
            }
        }
        
        
        return true;


Applications : 

1) Recommendation system
- ![image](https://github.com/user-attachments/assets/0c611363-cf6a-49ea-808a-f6df7492ea23)

- if user 1 and user 2 has common interest i.e Product 1 .
- Since , they have common interest if user 2 is interested in Product 2 , product 2 will be recommended to the user 1.

- Same works with Youtube.

2) Stable marriage problem

3) Vehicle Routing
- Bipartite graphs can model vehicle routing problems, where vehicles are matched to routes or destinations.




