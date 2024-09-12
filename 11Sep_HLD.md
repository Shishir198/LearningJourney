<h1>HLD of chat appliaction like Whatsapp</h1>

Credits - https://www.youtube.com/watch?v=a8KUKOh3YXk | Shrayansh Jain

Functional Requirement (Mandatory)

- One to One send and receive message capability.
- Group message support
- Last seen/online
- user authentication


Non Functional (Good to have)

- Scalable
- Low latency
- Availability.


Back of the enveloper estimation

- Total User - 2 Billion
- DAU - 50 Million
- 1 user send 10 messages to 4 people per day .
- Messgs per day - 50 M * 40 messages = 2 billion message/day .

- Memory - 2billion * 100 Bytes = 200GB/day

- Storage for Chat history for 10 years - 200GB/day * 365 * 10 .


DESIGN


![image](https://github.com/user-attachments/assets/23fef9f8-e6b5-4fda-abff-2b268dd0e4a8)

- When we know the ip address of the user we can send message directly.
- But this peer to peer connection is not scalable and we also want to maintain a chat history its not possible in peer to peer.

 - SO user a and B will not be talking directly , but via chat server (client server architecture).

   ![image](https://github.com/user-attachments/assets/ff7c8f3a-d1c0-4044-b562-74498cfe17e1)


 - Client server is reponsible for scaling , availability , groupnig and maintaing chat history .

 - Which protocol to use ?

 -  HTTP
 - ![image](https://github.com/user-attachments/assets/8d8982f4-103e-43eb-be60-8a10621de41f)

 - Http works on request response model .I
 - It is good while sending message to server , but it fails on receive function as server cannot send reponse directly to receiver without a requests in HTTP.

  Soliutions : 

Polling  -
     ![image](https://github.com/user-attachments/assets/3251f63c-3e17-4295-a187-fcb4c5dc7b6d)

  - This is type of enhanced HTTP .
  - Here the client keeps asking again and again (polling) the server whther it has the message for it .?
  - AS HTTP works on TCP protocol , connection has be maintained while send request server via handshake alogithms etc , and after response is received the connection is removed.
  - Then , again client jhas tp create a connection and then again requests for is there any new message for it ? , 
  - This lead to wastage of resources.
  - Its not scalable

Long Polling

  ![image](https://github.com/user-attachments/assets/d5f6ddef-3f8d-4aad-87db-9bc64261c2f9)

  - Connection will be created and the client send the requests to server , is there any mssg ? .
  - Now the server will not respond immediately , it wiill wait for threshold (e.g 1min) and then response a Yes/NO.
  - Better than polling , but still not scalable as its blocking requests .
    
WebSockets
  
  ![image](https://github.com/user-attachments/assets/3bcea849-5c24-4d98-b735-8e415ce381bc)

 - It is bidirectional and persistence connection.
 - the first time connection is created by sending handshake and ack from server.
 - As soon as connection is created , they can talk with each other bidirectiionaly . Client can send message to server , server can also send message to client .
 - This connection is persistent , it doesnt get close after sending response .
 - It gets close when one of client or server disconnects the connection or get down.



For one to one communcation 

![image](https://github.com/user-attachments/assets/aa04e106-38af-4620-ba5b-7538f2c54fec)

- SO here different client/users are connected to different servers .
- If user1 requests sendMessage(user2, "Hello") . Now there are 1000 of chat server , how will chat server know that it in which server it has to pass the message in order to send to user2.

- For this we have different service "UserMappingService" , which will store the information particular user is connected to which chat server.

![image](https://github.com/user-attachments/assets/e22b9371-178c-453e-8581-62ebb5ae9e75)

- WE can say this is provided by ZooKeeper - It is coordinator which helps to coordinate between servers in distributed environments by maintaing info.

- How does it maintains ?

  ![image](https://github.com/user-attachments/assets/b92e4ce2-5d56-4881-9041-8e2d9e742caf)


- Wheneever the client established connection , (as soon it gets online) , we need to assign chat server based on geograpphy and other things .
- Zookeeper , is responsible to assign a chat server to newly coming user , and adds it into record. e.g user1 is assigned server100

- Now when user1 sends requests to sendMessage(from , to , message , type(single/group))
- as sendMessage(user1 , user2 , "hi" , single)
- it will be fulfilled by server100 , server100 will contact zookeeper to know which server does user2 is connceted and it will pass the message to that server.
- That server will be responsible to send message to user 2.

- This is happy flow , when both users are online .

- If we want to maintainchat history , chat server will be connceted to DBs.

- Which DB  : we can use NoSQL as data hierarchy is not there , we want faster search with low latency with millions of data .

![image](https://github.com/user-attachments/assets/87f433b2-a00c-492c-b820-7f922d037e9a)


- How data can be stored - we can use coloumn DB (used by discord , fb messenger etc.)

![image](https://github.com/user-attachments/assets/5509322b-5270-4590-9050-8beca1482ce2)

- We can use [from and to] as partition key .
- Message id is used to maintain sequence.



How to handle when some chat server becomes down :

![image](https://github.com/user-attachments/assets/a7c5f6b3-4073-45b8-93ba-39fcf24af042)

- if user2 has internet issues or chatserver2 gets down .

- There will be no mapping for user2 with a chatserver in that case .

  ![image](https://github.com/user-attachments/assets/0f6a290e-4bfe-45d6-bf7c-d0ca44530ed2)

- If user1 send message to user 2 , chatserver1 will check with zookeper and it will find no entry for user 2 , so the message will not be able to send so its adds to DB.

- When user 2 comes online , (e.g -authenticated) -- it will get to mapping server and it will assign a chat server (server3).

- Now , server3 will check db , if there are pending messages , it will send it to user .


![image](https://github.com/user-attachments/assets/45ef0a3a-8bec-4fed-9dc0-198c6f35ec2f)



Group messaging :

- we will have seperate entity as GroupService
![image](https://github.com/user-attachments/assets/03825425-d5ca-4a36-9b26-5e4145b568f0)

- if group1 has user1,2,3,4 and user1 send a message to the group .

- The chat server will call group service to know which all users are present and ask zookeper to know their connected chat servers for u2,3,4.
- Thus sends the message .


LastSeen/online

- We can have a presenceSystem which will be responsible to maintain this .

- Users in particular interval of time will send heartbeat signals to this server , to indicate their presence .
- If for partitulcar interval lets say 1 min , server doesnt get signal from user it will mark it offline and display it last heartbeat time.

- CAn we also use zookeper to detcetd the online /offline like , when the chat server is allocated to user it is online , if no server allocated user is offline.  _ NO.

- Because for e.g user is in network fluctuation area , it will toggle everytime offline-online-offline-online that will be bad UX . So thats why present system has threshold time ..

- ![image](https://github.com/user-attachments/assets/339985b9-523a-4f1a-a252-51d5fe5cc7b3)


