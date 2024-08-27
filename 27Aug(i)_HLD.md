<h1> Scaling from 0 to million users </h1>

Credits - https://www.youtube.com/watch?v=rExh5cPMZcI

Nine Steps evolution steps of system 
![image](https://github.com/user-attachments/assets/bc84060f-fedd-4f22-9005-33097ddc720b)
![image](https://github.com/user-attachments/assets/e086d79e-d344-427a-a1bb-fb3d8d9c8e2b)


1.)Basic

![image](https://github.com/user-attachments/assets/870ece10-a0ae-448c-ab16-143bfc8c0e56)

Very basic level of application  , so server application and DB both are in same server .

2) Segregate Server and DB

   ![image](https://github.com/user-attachments/assets/f297f6f3-1a6f-4dc0-a83b-c33b5e8ace15)

- Now we can independently grow Server nodes or DB nodes as per needs .
- It also adds level of security

3) Load balancer and multiple app servers
   ![image](https://github.com/user-attachments/assets/64d5785c-9ca5-4a6f-b6fb-5ece61e3b0ea)
   - if scaled to more users and created more server nodes , we need to have load balancer setup.
   - It bringgs a layer of security as clieant cannot access app server directly and can only contact through load balancer
   - Load balancer talks with server on private ip.
   - It routes the request to different server as per needs because single server has limits.
4) Database replication

   ![image](https://github.com/user-attachments/assets/8f1d038e-be1d-4060-a3ed-5b9cd668c87e)

   - we have master slave database setup in the system were read operations are done though slave and create or update operations are done in master.
   - There are mutliple nodes of slaves db , and if in case master db fails one of the slave server will get promoted to master server.
   - We have replicas of data from master to slave.
  
5) Cache

   - This is needed when the application sever wants to talk with DB.
   - DB opreation is expensive and its a network call
   - So everytime before hitting DB we will check data on cache if present (cache hit) , other wise it will fetch in DB (cache miss) and write in cache.
   - In cache , it has TTL(time to leave) with data , which when expires the data will flush out. 
  
   ![image](https://github.com/user-attachments/assets/fbd73593-9f24-4eb9-8efe-c3735936417d)

6) CDN (Content Delivery Network)
   - CDN aslo does cachig , but not all who does caching is CDN.
  
   - CDN is used when the user is widespread geographically and send requests from different parts of world
   - If we have datacentre only in few places , only the clients nearer to data centre will get fast response and users requests far from center will suffer latency.
     ![image](https://github.com/user-attachments/assets/0c43fc9e-44d0-40ec-a618-e103b6c72e44)

   - This problem is solved by CDN
  
   - So , Multiple CDN node can be added in near most of country to server requests of user geographiaclly .
   - CDN does caching of static content like HTML , CSS , Videos etc so user reuqests will not have to travel long and hit Data centre every time.
  
     ![image](https://github.com/user-attachments/assets/5f3d9747-fc0a-45af-98b8-835ab199bfe2)

   - If the data is not present in current CDN , it will try to find in teh neighbouring CDN , and at last will try to fetch data from Data center.
  
   - Thus it also adds a layer of security , and improve performance for data centre as not all requests are being routed to main DB.
   - It can help safe from DDOS attacks as only the CDN nodes might get affected , we can make CDN smart to detect whether its a bot or not before routing back to DB.
  
     ![image](https://github.com/user-attachments/assets/8da31d4d-1ded-41ef-a9cc-1ed407602747)


7) DATA Center

- I can have more data centers

  ![image](https://github.com/user-attachments/assets/12b0818a-3937-43ad-b110-31522e766657)
- LB can route requests based on geographic data centers

- It also has DB replication from one data centre to another . so if one failes , another cam take care of requests.


8) Message Queue

- It gives a asyncronous nature to the system (E.g - send webhooks , send emais to users)
  ![image](https://github.com/user-attachments/assets/ac8eff04-395f-4163-9491-b1a121a03fc9)
- We cannot hold the thread untill we have notified teh customer or user , it can be done asynchornously .

![image](https://github.com/user-attachments/assets/715aba49-88c3-4ecc-b87e-2a552c20c231)

- Producers sends routing key with message to the exchange system  .
- Exchange compares binding key and routing key and decides in which queue data is to be sent.
- And notfies all the subscriber of that queue.

- Different setup for exchange and queue
- 1_) Direct
- if(routing key == binding key) send data to queue

- 2) Fanout
     - exchange will send the data to all the queue and subscribers and the subscribe who chose to work will work otherwise ignore

- 3) Topic
     ![image](https://github.com/user-attachments/assets/3ca5c427-cf7c-403c-8916-30599c5f7075)

     - Exchange can send data wo multiple queue according to condition matached with wildcard(not exact comparision)

9) Database SCaling

   -  It is of two types:Vertical and Horizontal
   -  Vertical - add more CPU and RAM (add more resources) but there is limit
   -  Horizontal - add more nodes of databases
  
   -  Now the implementation of Horizontal databases is SHarding
  
   -  ![image](https://github.com/user-attachments/assets/ca1dade3-fc7e-4878-b931-6a65b710ac17)
  
   -  Horizontal Sharding
   -  A large table is divided into multiple tables in terms of rows
  
   -  ![image](https://github.com/user-attachments/assets/040c8514-8640-471c-85da-b68b63552b74)
   - if a large table has rows 1 to 1000 , it will be divided into 2 tables with table1(shard1) : rows 1 to 500 and table2(shard2): 501 to 1000
  
   - Example if names startes from A to P , will be in shard1 and Q-Z in shard2.

  

    - Vertical Sharding
    - A large table is divided into multiple tables in terms of coloumns
    - ![image](https://github.com/user-attachments/assets/33ae220a-62d9-464b-8a0c-d8b263ff093d)
  

       - Disadvantage
     - If names with A is more then shard 1 will get filled up and might be it again needs to be sharded and might be again . SO , it can create a tree structure of shards.(solved by consistent hashing)
     - If rows divided - problem in join (solved by denormalization)

  
  







   





