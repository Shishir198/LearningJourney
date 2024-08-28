<h1>Consistent Hashing</h1>

- Consistent hashing is a technique used in distributed systems to efficiently distribute data across a dynamically changing set of nodes, such as servers or database shards. 
- It is particularly useful when the size of the node set is not fixed.
- In traditional (normal) hashing, you distribute data across N nodes by computing hash(key) % N. 
- This method works well when N is constant, but it has significant drawbacks when nodes are added or removed.

- Becasue for example there is data request (data1), we will assign/find the shard using normal hasing when size is 3
- if we hashed key(data1) , hash(data1)%3 --> 1 (position) .i,e we need to fetch or add data1 to shard1 lets say .
- Now if more node is added , N=4 when we again want to search for data1 : hash(data1)%4 --> '?'
- it will lead to any other number or mayebe 1 but its not a guarantee , so problem aroused when the size was changed , so we will need to rebalance the data again from shard1 to shard2 etc.
- So this is not effecient .

- This is solved by consistent hashing .
- It maintains a rebalancing factor to 1/n of the total keys where N is no of nodes.
- This means , In consistent hashing, ideally, when a node is added or removed, only about 1/N (where N is the total number of nodes) of the keys need to be rehashed and reassigned.
- This means that even with a dynamic number of nodes, the system remains relatively balanced and efficient.


Working : 

- It consists of virtual ring (circular queue).
  ![image](https://github.com/user-attachments/assets/466b9758-fe8d-41fa-a0b1-286cd245c163)

- Lets suppose there are three servers currently , and we used mod hashing under it like
- ![image](https://github.com/user-attachments/assets/7b2e1e25-9d6c-480f-9d37-24711731f053)
 and s1 , s2 and s3 has been assigned slot3 , slot7 and slot 11 respectivelty which is equidistant from one another .

- Now we encountered several keys which after hashign is assigned this slots
- ![image](https://github.com/user-attachments/assets/bacac7ae-7638-4263-af7c-e5a4362a8750)
- We will start checking clockwise from the keys
- e.g clockwise from k1 , k2 we will encounter server1 , hence this server will serve the key .
- k5,k7 - server 2 .
- k6,k3,k4 - server 3

- If Addition of server lets say s4 at slot 5:
  
  ![image](https://github.com/user-attachments/assets/eace2f2e-2c1a-4e2f-b0ca-b5435a57785b)

so we have to do rebalancing of k7 from server 2 to server 4 . (Rebalancing is minimum)
![image](https://github.com/user-attachments/assets/74cd1457-04f4-41d8-894c-f03ece408481)


- If Addition of server lets say server2 at slot 7:
 
  - Now k5 will get server3 , again we need to do minimum rebalancing
  - ![image](https://github.com/user-attachments/assets/dffb72ad-d501-4382-8af6-8b5ec0ab15e8)


Disadvantage of consistent hashing

- WHat if server positions is not equidistant , one server will have to bear much load .(Consistent hashing purpose was to divide the load equally)
- ![image](https://github.com/user-attachments/assets/81839c1c-8c74-4511-8980-915020b543b7)
- When running clockwise , k1...k6 all needed to be covered with server 1.
- SOlution - Virtual objects

- ![image](https://github.com/user-attachments/assets/72505050-0560-4f22-bfd8-82236b9ae94a)
- servers are replicated to other random points.

- ![image](https://github.com/user-attachments/assets/99b27db1-3b02-4a4a-a73c-205e98d9df7d)

- Now, whenever a new key will arrive it will get the server more quicky to fulfill the requests
- ![image](https://github.com/user-attachments/assets/8e17d62b-5cc5-4a43-bdaa-de5d27e3c2a3)

- Not keys are equally divided .
- ![image](https://github.com/user-attachments/assets/f6a1a688-9110-4ad0-88cd-4a228240f931)

 - How much rebalance is required -
 - Replicate the server untill we reach the point where only do rebalancing of 1/n percent of the total keys.
 - ![image](https://github.com/user-attachments/assets/87529ffb-0e86-412f-a39a-6fd22bd9ff77)


Uses : 
where we have to spread the load equally to different nodes (server nodes or DB nodes) and these no of nodes are not fixed in size i.e its dynamic. becasue in dynamic sizes normal hasing doesnt work
E.g - load balancing of app node servers, horizontal sharding in db .







   

