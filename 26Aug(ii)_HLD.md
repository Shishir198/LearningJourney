<h1> Design Patterns | Microservices </h1>

Three Important Design Patterns :
![image](https://github.com/user-attachments/assets/7d838412-3a48-41c7-aa38-db7dc68f0495)

1.Strangler Pattern 

- used when refactoring from monolithic to microservices.

- Decomposition pattern states that how much small a services is to be made , (It defined the final look)
- But , the process of refactoring from monolithic to microservices is governed by Strangler pattern (It defines the process to reach the final look)

- We never convert the monolithic to ms in one go , we do it slowly and only some services at a time are being convcerted and tested on some % of traffic.

- ![image](https://github.com/user-attachments/assets/a65de38f-b304-40e6-a445-8393009e2224)
- ![image](https://github.com/user-attachments/assets/e6fd2590-fbdb-4410-a9b8-9cc6de4a67e3)

- We can have a controller which governs requests and traffic concentration to be districuted to monolithic or ms architecture .
- If only 1-2 servicees are being converted , the controller serves those requuests to the service and beign tested on e.g 10% of traffic .
- If any issues observed , we can stop the ms and all requests will be fulfilled by monolithic one .

- Once we gain confidence , we keep increasing the traffic concentration to ms and 0% to the monolithic then ultimately it gets deleted.

2. SAGA Pattern

   There are 2 ways for data managenent in MS
   i )- Independent DB for each service
   ii)- Shared DB between the services.
   
- SAGA and CQRS is used in scenario where the independent databases exists for each microservices.

- Why Shared DB is not successfull?
  - ![image](https://github.com/user-attachments/assets/03732e47-f334-4d76-82e9-0857ebffdc93)
  
  - Lets say , s1 - order service , s2 - inventory service , s3 - payment service
  - Issues:
  - So if inventory data is not often added to the db , whereas order data are coming in millions .
  - Now when we want to scale the db only for order data , we need to scale the entire db if shared . We cannot scale independently for business use.
  
  - If for tables concerning s3 , we want to change table coloumns or rows e.g deleting a coloumn we cannot do that because it might affect the other services
  - because of shared db
  
   Advantages  -
  - Easy to query join 
  - Easy to mantain transaction propoerty (ACID)
  - this advantages becomes disdvantages for different DB per service method


- For different DB/services 
![image](https://github.com/user-attachments/assets/03967177-794c-40d0-974e-0b454b2737b1)
- If a request invloves s1 , s2 , s3 , I want to maintain a common transaction ACID property ,
- It becomes difficult to maintain ACID property in each databse transactions because the transaction is spannign distributed DB.
- This problem, is solved by SAGA pattern.

- There is difficulty in joining tables which might be present in different databases - This is solved by CQRS.


<h2>Saga Pattern</h2>

![image](https://github.com/user-attachments/assets/642df927-f8ca-417d-ab93-b17e6eb063db)

- Sequence of local transactions.
- If we have 3 services , s1 - order service , s2 - inventory service , s3 - payment service .
- Incoming Request - Place an order  , we need to make DB changes on all three service's databases.

- According to this pattern , teh s1 will send [Success Order Event] which will get listened by s2 and based on the event it will update its inventory database 
- S2 will create event passed to s3 to state inventory is updated , tehn s3 will updated its own db accordingly . Now , if in s3 the db transaction gets failed.
- S3 will send Failure event to s2 suggesting to roll back the changes at their database , which s2 will again pass failure event to s1 which will rollback its own database based on the events.
  
- Now , even if the transaction spans different dbs , ACID is maintained (either full transacrtion is done or none) on all databses based on event which was earlier a problem.i.e(If s3 gets failed , how we will update previous state changes which are in another db)

SAGA is of 2 types 
1 - Choreography
![image](https://github.com/user-attachments/assets/c5ccd463-ca80-4ef1-b6fe-68214e64c994)

- There is message event queue , and services post there events and check in the queue. 
- The services themselves are responsible to send event to queue and retrieve event of there concern and do the tasks accordingly.
- I failed it will add event in failure queue .

- the services are independently listening and adding events to queue ,and working accordingly .

Drawback - cycle dependency might be created , event created from s1 to s2 , in return event created from s2 to s1 and it can form a cycle.

2 - Ochestration

- There is orcehstrator component between the services which will manage sending and routing the events to appropiate services.

- ![image](https://github.com/user-attachments/assets/3cd2f46d-55d4-43a7-9135-5be65586c4bc)

- O will call s1 , accordin to response from s1,  it will call s2 and then s3 .
- If reponse is failure from s3 , it will call s2 to rollback its changes accordingly , if s2 fails O will direct s1 to rollback

- So here , O will direct services to update or rollback its changes .

- INterview QS :

![image](https://github.com/user-attachments/assets/8e1fd7d5-a351-46f6-9871-1357891fb9e2)

If there is payment request from person a to b , and there is 2 microservices "Payment" which will store transations and "Balance" which will store amoutn of users.

REQuest - Pay Rs.10 from A to B.
Now ,  request goes to balance to will check if enough balance avilable and if true it will detect rs10 and upadte payment db for tranbsaction details (A->B | Rs.10 )
but if failed , how will we manage further , because the balance is deducted.

Solution - SAGA Pattern 

If failed the payemnt will create a failed_transaction_event which balance ms will listen and rollback the changes i.e make the balance +rs10 in its db.

 




3 .CQRS Pattern 

- Command Query Request Segragation
- Command - (Create Update Delete)
- Querey - (Select)

   Problem -
  ![image](https://github.com/user-attachments/assets/40b76db2-e28c-4d63-8fb2-a6dbe45ccdbe)

  - If we want to create a query to join table 1 and tabl2 present in different db's.

 Solution - segrgeate the db for create , update , delete and running select query
  - ![image](https://github.com/user-attachments/assets/2331020f-52d5-4b11-89bc-a41fb4cea580)

  - Create a new View which will store all the data from table1(Order) and table2(Inventory) , lets say (Order_Inventory_View).
  - Do the CUD command in its respective table and read queyry from the new table which will store common data for both tables.
  - How will it gets updated whenever a new CUD command is executed in respectuve table

  - SOlution - Event

  - ![image](https://github.com/user-attachments/assets/dd3b9f60-50c6-424a-9ff6-8c0dbe6b480c)

  - Any change made in table1 or table2 which will create event and will be listened by new table which will apply the same commands  there.

  - SOlution - DB trigger

  - Whenever a changes in tabl1 or 2 , a db trigger or procedure will come into action and make similar changes in new table




  

     


