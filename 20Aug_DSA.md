DSA : https://www.geeksforgeeks.org/problems/covid-spread--141631/1

<h1> Solved Covid Spread Problem in GFG </h1>

- Very basic question to apply in BFS.

- My solution :


```
class Solution{
    
    private class Pair {
        public int first;
        public int second;
        
        public Pair(int first, int second) {
            this.first = first;
            this.second = second;
        }
    }
    
    public boolean validate(int i , int j , int n , int m){
        return i<n && i>=0 && j<m && j>=0;
    }
     public int helpaterp(int[][] hospital) {
        // code here
        Queue<Pair> q = new LinkedList<>() ;
        
        for(int i=0;i<hospital.length;i++){
            for(int j = 0;j<hospital[0].length;j++){
                if(hospital[i][j] == 2){
                    // System.out.println(i + "," + j +"enetered");
                    q.add(new Pair(i,j));
                }
            }
        }
        
        int n= hospital.length;
        int m = hospital[0].length;
        int ans =0;
        while(!q.isEmpty()){
            ans+=1;
            int totalPatients = q.size();
            while(totalPatients-- > 0){
                Pair elem = q.remove();
                int i= elem.first;
                int j = elem.second;
                
                // System.out.println("Executing for " + i + "," + j);
  
                if(validate(i-1,j,n,m) && hospital[i-1][j] == 1){
                    hospital[i-1][j]=2;
                    q.add(new Pair(i-1,j));
                    // System.out.println((i-1) + "," + j + "enetered");
                }
                if(validate(i,j-1,n,m) && hospital[i][j-1] ==1 ){
                    hospital[i][j-1]=2;
                    q.add(new Pair(i,j-1));
                    // System.out.println("Entered " + i + "," + (j-1));
                    
                }
                if(validate(i+1,j,n,m) && hospital[i+1][j] ==1 ){
                    hospital[i+1][j]=2;
                    q.add(new Pair(i+1,j));
                    // System.out.println("Entered " + (i+1) + "," + j);
                }
                if(validate(i,j+1,n,m) && hospital[i][j+1] ==1){
                    hospital[i][j+1]=2;
                    q.add(new Pair(i,j+1));
                    // System.out.println("Entered " + i + "," + (j+1));
                    
                }
            }
  
        }
        
        for(int i=0;i<hospital.length;i++){
            for(int j = 0;j<hospital[0].length;j++){
                if(hospital[i][j] == 1){
                    // System.out.println("1 found in " + i + "," + j);
                   return -1;
                }
            }
        }
        
        return ans-1;
        
        
        
        
    }
}
```
