<h1>Network Protocols</h1> 

Credits - https://www.youtube.com/watch?v=JwTiZ9ENquI | Shreyansh Jain

- When to use which protocols like WebRTC , TCP , UDP etc .
- When we are designing Whatsapp which protocol to use , or we are designing Google meet , which protocol to use ? We need to Know this.

- Network Protocols comes into picture when two machines want to communita ewirh each other.
- ![image](https://github.com/user-attachments/assets/3aec36cd-e4f5-4309-b646-b4ba230d0fae)

- It sets up rules and regulations followign which , the parties and connect and communicate accprdingly , To communicate they need to follow same rules right , thts why network protocols

- Application Layer
- we have below protocols
- ![image](https://github.com/user-attachments/assets/a5f148fd-8be1-4e54-b630-48b9447e007a)

- IN http , ftp , smtp there is one way communication , only client can send request to server and server send the response in return
- In WebSocket , its a 2 way communcation , client and server both can communicate with each other independently.
- Is web socket = peer 2 peer? NO.
- ![image](https://github.com/user-attachments/assets/bb0021aa-8efb-481c-91a5-32dfcdb07336)
  
- Because client can communicate with server and server can commnunicate to many clieants but clieant cannot communicate with another client (not peer to peer).

- In Peer to peer , all can talk to each other like below
- ![image](https://github.com/user-attachments/assets/26942317-62d9-4cc3-855c-36847bdcca3c)

Transport Layer 

![image](https://github.com/user-attachments/assets/88fba568-a7d5-447e-b407-77b410bc0643)

- In TCP, IP  we maintain a virtual connection and in those connection communication happens.
- We maintain the data into packets and send packets over connection, its gets received unordered , but ordering is maintained.
- ![image](https://github.com/user-attachments/assets/f95c81c5-a6a4-47b5-9118-1702961bd8e7)

- In UPD , there are various virtual connections not single.
- Data is divided into smaller datagrams and those are send in parallel over the multiple connections made , we dont maintain ordering here but fast.
- ![image](https://github.com/user-attachments/assets/7c50959d-9184-4cc2-938d-43989dd2a170)
- We dont have to take care of acknolwdgment , ordering and connection is not maintained.

- Usefull - video calling.(WebRTC uses UDP) and ordering is not an issue.

FTP :
In FTP 2 connections are made and We dont use FTP because the connection made is not encrypted , thats why we use HTTPS.
![image](https://github.com/user-attachments/assets/6a39c8d2-4706-4352-acc7-2c8476eadeac)







 
