<h1> How to design a KEY-VALUE Database </h1>

*Credits - https://www.youtube.com/watch?v=VKNIhztQnbY | Shreyansh Jain*

-  is a NoSQL database that stores data as key-value pairs
-  Data is stored as unique keys with associated values, which can be simple or complex data types.
-  Optimized for quick read and write operations, making it ideal for high-traffic applications.
-  Designed to scale horizontally, handling large volumes of data and high request rates by distributing data across multiple servers.
-  Supports dynamic schema, allowing easy adjustments as data requirements evolve.
-  Key-value stores like DynamoDB are well-suited for use cases such as caching, session management, and applications needing high availability and low latency.
-  Amazon used DynamoDB for its add to cart feature internally.


We need to achieve 
- Scalibility
- Decentralization
- Eventual Consistency

Steps to do : 

<h2> Partitioning  </h2> : 

- ![image](https://github.com/user-attachments/assets/d94165e2-5195-4d2a-89fb-809e10a35471)

- We store data in hashtable structure in memory .
- Now in scale of billions of users accross the region , only one hash table cannot serve teh objective , so we need to scale .
- How do we scale , - through partitioning.

- For partition it uses consistent hashing. (Already Explained , notes here - )

 - Summary : Consistent Hashing
   ![image](https://github.com/user-attachments/assets/c6a238e1-c253-48ca-875a-d08ffb1b9e5f)
  
  - All the servers are assignd different ranges , for it will be responsible to handle the key.
  
  - If "CAR" - generateKey --> 45 ,we will see clockiwise , and found server S1 . So s1 will be handling this key .
  
  - If keys are being generated in same range for certain hotkeys there will be load in single server , to solve it we use virtual nodes .
  
  - The instances of server1 , server2 etc.. are being spread acroos , to distrubute the load .

    ![image](https://github.com/user-attachments/assets/6258fda9-b08a-4df9-a7f0-cac096e19c42)

- This is how scalability is achieved.

<h2>Decantralization</h2> 

- means if any failure happens at one node , system should not be down.
- What if s1 goes down which has the data for key "car" , if it gets down , we cannot retrieve the value .
- Solution --- >> Replication.

  <h3>Replication</h3>
 
  - There is replication number by default - 3 (configurable) .
  - If the data is in server1(coordinator)  , So we need to replicate the data to N-1 nodes further in clockwise direction , so in s2 and s3 it will copy the data ahead.
  - Even if server 1 gets down , the request for key can be served by other servers .
 
  - As sson as data gets copied , server also maintains a preference lists .
  - Preference lists is for each key range .
 
    ![image](https://github.com/user-attachments/assets/a4be5403-eba1-452c-8fa7-9ea015d67275)

  - E.g for key range 1to50 : s1(actual server/co-ordinator) , first element is always coordinator , s2(replica server), s3(replica server) .
 
  - Its not necessary to always be in sequence as there are alogirthms according to different data centres , location etc.
 

  <h3>Get and Put operation</h3> 
 
    - ![image](https://github.com/user-attachments/assets/78ff981f-9db1-4034-9ca1-3f986f5bbe95)
   
    - In this db , each server knows about other servers preference lists .
   
    - In requests of PUT [Key1 ,"CAR"] --> generate(key) -->  say 45 --> assigned to server 1 --> it will replicate the data to other server accroding to N in asyncronous manner.
   
    - When we will send the success response ?  There is formula for R+W > N .
   
    - If W=1 . It will wait for only response from one replica server . We will hold the reponse of put requests untill we get reponse from either s2 or s3 . This is defined by W.
   
    - Now GET:
   
    - When their is requetss from client to GET(key) --> LoadBalancer[]
    - Now if load balancer is generic is can send to any server and as we know the server nodes are aware of each other ranges and preference list it will hop requests to that server. (It is simple to implement , but has high latency )
    - Now if Load balance is partition aware , it will send the requests directly to server responsible for handling the key which is within its range .
   
    - Now , when requets reaches the server - it will also poll all the replica servers to get value of the key .
    - Here comes the 'R' in formula of R+W > N.
   
    - R defines no of reponses the coordinator has to wait for read operation . Therefore if R is 2 . and coordintor is server 1 .
    - It will suppose polls 4 replica servers but it will only wait for any 2 responses( say s2 , s3) and then returns the success response to client. 


    - But why , why do we poll other replica servers
   
    - As no system is perfect , s1 might have been down due to failure , meanwhile the data update requests might have been placed and get fulfiiled and stored on other replica servers.
   
    - SO , in order to fetch the data with latest versioning we poll each servers and check which data has latest one and then return it as response. 
   
    - For data Versioning we use Vector Clock.
   
    - Vector clock - lists of [server,counter] for each object/key.
 
    - During a put request [key , CAR] we will store the value on server1 as CAR[S1,1] , and thus replicated on other servers

    - ![image](https://github.com/user-attachments/assets/edeabece-ebad-415a-965f-ca8f10d44b9c)
   
    - Now if , again put requests for same key [key , CART] , and s1 is not down . it will update its version to CART[s1,2] ans similar is passed to server2 as CART[s1,2] and server3 also has CART[s1,2];
   
     ![image](https://github.com/user-attachments/assets/67fa63ba-4236-49ab-aa81-50c3a5274a11)


    - If s1 is down now, and put req comes in for put(key1 , CARM) , is s1 is down requets is passed to s2 .
    - Now s2 will upadet itself as CARM[s1,2][s2,1] and in parallel a different requetss comes put(key1 , CARR) and it goes to s3 s3 will update itself as CARR[s1,2][s3,1].
   
    - ![image](https://github.com/user-attachments/assets/da3b97cd-cb21-4e9a-9e7c-1cb833d3235b)
   
    - s1 is down and s2 and s3 has partition.
   
    - Now if get request comes for key1 and s1 is awake now .
   
    - ![image](https://github.com/user-attachments/assets/472707ed-e74e-4ae9-9fa3-74277d7cc601)
   
    - It will poll other servers and check every server has base result so it isold it will discard the first one .
    - Between second and third one , s1 cannot resolve and its creates a conflict so it will return both the result to clieant .
    - Client can be a AddTOCartService - this should have alogirthm to resolve the conflict . There are several algos like LastWriteWin .
    - And then it will send put requetss again and s1 gets it , below is the result.
   
    - ![image](https://github.com/user-attachments/assets/5e8d685e-cdd5-474a-86e6-c013edc3e710)


<h2> Eventual Consistency </h2>

- If one get requests it is not necessary to get latest data , we can get stale data . But if we repeat we will eventually get the latest data .
- Thus from CAP theorem , it trade off consistency with High availability . (DynamoDB)



<h2> Gossip Protocol </h2>

- In a ring , each server is aware of everybody range . How everybody knows it and aware of if a service is down. That through gossip protocol.
- each server has membership list 
- At certain intervals , sever sends signals (heartbeats) to other nodes to say "Its up" , along with the heartbeat it sends additional information and ranges.

![image](https://github.com/user-attachments/assets/0c663a83-2b1b-4767-b815-6d85df54c195)

- If e.g server2 has not sent heartbeat to other servers and in atleast two server has updated heartbeat information of server2 , we decide it is down , we remove it from system.


<h2>Merkle Tree</h2>

- Merkle tree is used to confirm whether for certain range the replicas hold the updated values of not as per the coordinator.

- ![image](https://github.com/user-attachments/assets/7e23082d-b979-4208-a9c7-3a559e23f056)

- the keys are in bottom and it stores the hashcode of its children , in intermediate nodes upto root .

- If the root hascode of coordinator and replica is same , all keys in range (thousands or millions) are in sync no need to check further .

- If root values are not same, then it checks to left and right  .

- If left has same hashcode , we dont need to check in left further and will continue if right . Therefore we saved lot of comparisons and time as we are using binary tree concept.

- ![image](https://github.com/user-attachments/assets/5e88e4aa-723c-4b4b-b2ec-4f7104dd70a2)

  
 


