<h1>Microservices Design Patterns</h1>

- monolith -  everything in done under single code base , single application.

Disadvantage - 
- Overloaded IDE.
- Very tight coupled application(if changed at one module , it might affected other modules and whole application is to tested and deployed)
- scaling is hard.
- module are not independlty deployable and scalable.(If only we want to scale payment service , we need to scale entire system)

Microservices - 
THe project is divided into independent services which are deployed as single unit so scaling partitcular module is easy.
![image](https://github.com/user-attachments/assets/b0b1f15e-a2eb-422a-96a5-1e0e923ebbb2)

Disadvantage - 
- Debugging in hard , we need to trace request in entire flow.
- Single function call in monolith becomes entire netowrk call.(latency)
![image](https://github.com/user-attachments/assets/84cdfbb0-6448-49ec-88a7-5b9962748e97)

- When a service(say service3) deployed with some new code changes and may be response is changed and service2 was using the response from service3
- It will break the service2 is response is changed and its completely dependent on it ,
- Now if service1 was dependent on response of service2 , if serice2 breaks then service 1 will also break leads to chaining effect.

- So , when a service goes live with new code changes , debugging beceomes hard that what clients might get affected .
- So Monitoring becomes difficult.

- Transaction management is little bit difficult because 
- ![image](https://github.com/user-attachments/assets/7c6bcfc9-0289-4418-8e15-c4a9874606d3)
- in mononlithicc we have one db so managing and tracking transaction becomes easy , once the db operatio is completed I can stop ,  if prb occurs it can roll back in single db

- ![image](https://github.com/user-attachments/assets/74cd1695-b1bf-4076-805c-5ede02b1f213)
- In microservice , we have several services with independent databases each , No wif a rquest is to be fulfilled by combination of 2 services ,
- Single service can manage the transaction of its own db , so there cannot be a common transaction for request .
- What if transaction succeeded in one service but failed in the other one , it becomes difficult to manage .


<h1>Microservices Design Pattern</h1>

- These patterns state some rules , on how to build microservices system from ground up .
- There are various phases in building microservices.(Decomposition , DataBase , Communication , Integration .... Observability ... MOnitoring...so on.)

- Decomposition pattern states how to break monolithic service to smaller services (how much small, how to decide), like decomposition by business capability or
  decomposition by subdomain etc.

- Database - After you have created smaller service how to decide whether we need to assign each service a db or keep a common database.

- Communication - Ways/patterns to communicate between services like API , or events etc ?

- Integration - Also needs to be integrated by other applications , how to integrate - e.g API gateway?

- So in every phases , there is design decision problem  from decomposition to monitoring everything , which can be solved following Design Patterns at every phase.

1.) <h2> Decomposition Pattern </h2>

- ![image](https://github.com/user-attachments/assets/a6b65b45-26df-41e2-80c6-a6d2f0fed873)

-<h3> Business capability pattern </h3>

- E.g Online Order Application
- Business functionality includes - Order mangement , product management , Account management , Logging , Billing , Payment .....
- So , these all will be our different microservices.
- It reuiqres good knowledge of business requirements .

- <h3> By SUbdomain (Domain Driven Design) </h3>

- So if order management is one domain (in previous pattern it was microservice) , so this domain can have multiple microservices under it.
- Payment can be another domain which can have other microservices within it .
- ![image](https://github.com/user-attachments/assets/266a1ca7-3003-4565-a23c-f9d3aa876412)




