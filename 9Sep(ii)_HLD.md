<h1>SQL vs NoSQL Database |  What to choose , when ? </h1>

*Credits - https://www.youtube.com/watch?v=fG8c-huFt70* | Shreyansh Jain

- Choosing between SQL and NoSQL databases is crucial as it impacts data structure, scalability, performance, and flexibility.
- SQL databases are best for structured data and complex queries with strong consistency,
- While NoSQL databases excel in handling diverse data types, high scalability, and flexible schemas.
- Your choice should align with your application's specific needs and growth expectations.


- We will divide it into 4 quadrants.

![image](https://github.com/user-attachments/assets/c3c5d300-216c-4d13-910a-eb74c10aa25d)

- We will discuss these 4 points for both , and then we will get the idea when to choose what ? 

<h2>SQL (Structured Query Language)</h2>

- Used to query data from relational DBMS.

- There are tables with rows and coloumns and relations exists between multiple tables .

- ![image](https://github.com/user-attachments/assets/778e8670-a0f8-40ed-a98f-e058c111bf08)

- With SQl , we are managing the data present in the sttructured format .

*Structure*
- is it maintained tables, row ,column and it is a pre-determined schema i.e (What types of values and in which format the data is going to be stored is already predefined) like Table name, fields name and its data types.

- We plan the schema first and then only we can insert the data and query in it.

*Nature*
- We say , the nature is concentrated or centralised .
- Means , all the data relating to an entity can be in other tables but all the tables holding related data should in One server.
- ![image](https://github.com/user-attachments/assets/46dab376-867f-4421-b5cf-e38e23e9645a)

- E.g - if we store employee info (id etc) in EmployeeTable and there are relations with other tables like SalaryTable where we store (empId, Salary) or AddressTable(empID,Address),
- All these 3 tables, should be present in one server .

- It can never be like some parts of row data is on server1 and rest are in server 2 ,neither 2 of coloumns of EMployeeTable will be in server 1 and th rest in server 2 .
- This doesnt likely happend with SQL , although we can achieve this but its not a general case. 


*Scalability*

- We say , My complete data should be present in server 1.
- If there is User table , which has millions of daata and still increasing , we need to scale .

- WE have two ways to scale :
- Vertical (add more CPU , RAMS and resources)
- HOrizontal (sharding ) - This will allow distributing the partial data from one server to another like half coloumns/rows in server 1 and rest in server 2 (but) its not well supported in SQL.
- Thats why , Vertical scalign fits better to SQL.

*Property*

- It follows ACID Property (Atomicity , Consistency , Isolation , Durability).
- This acid property make sure the data integerity and consistency.




<h2>NoSQL (Non relational / Not only SQL)</h2>

- *Structure*

- It works on unstructured data . It's not like only table coloumn rows .
- Structure can be of many types in this case , like key-value DB, document DB , coloumn wise DB , Graph DB .

- *Key-Value (Dynamo DB)*

- ![image](https://github.com/user-attachments/assets/83aedd68-3ad5-4a21-879a-5c9d8060886a)

- if key : 1 , value can be of any data type (integer , string , json etc) which means it is opaque.(any)
- Therfore , we cannot query/search based on value , we can only query based on the key , thats why its fast.

- *Document DB (Mongo DB)*

- It also has key and value , but value can be JSON or XML.

![image](https://github.com/user-attachments/assets/d26657c4-cecf-4226-910e-19af37a442b8)

- Difference between key-value is  , here we can query/search even on the value which was not the case with key-value DB.

- *Coloumn-Wise DB*

- It has a key , and the value is Pair of [coloumn and its value].
  ![image](https://github.com/user-attachments/assets/82ef75f0-1723-44d2-a7e0-48435e657d7f)

- E.g - key :10001 , Value : {  first-name : Shishir , Dept : IT , Interest : Coding }

- Key : 10002 , Value : {first-name : Annu , Dept : Sports} .

- Here , coloumns is dynamic . Some key haev more no of coloumns some can have less.

- *Graph DB*

- Data is stored in node and edge (shows the relationship).
- ![image](https://github.com/user-attachments/assets/5d7a0bea-bcba-4ed0-82fe-2a9455d5a702)

- This is commonly used in Social Networking and Recommendation systems .

- However , in sql also we have relationship like parent -child but it scans the whole rows and finds the relations.

  ![image](https://github.com/user-attachments/assets/b0e55267-a55d-45ea-a464-d46d2a318297)

- Here , we have direct relationsip so its very fast  .



 *Nature* 
- Here the data can be stored in multiple nodes because it doesnt maintain any relations. so , Node 1 can have certain piece of data , and other node2 ,3 can have other piece of data.
- So , nature is distributed .
- So in User we have 10million records , 2 million in node 1  , 4 million in node 2 .... and its still easy to maintain .

*Scalibility* :

- Horizontal scaling is easy - If size increases we can create multiple nodes and store the user data .

*Property* :
- It doesnt follow ACID , it follows BASE .
- BA - Basically available - (Highly available DB), the data is stored in distributed manner and also replication happens between the nodes .If one node down another can server the same requests.
- S - Safe State - State of data can be changed without interaction itself , because when node sync up happens in distributed system some node might be holding old data which when sync with latest node will upadte itself
- E - Eventual consistency - If we query , we might get old data but after sometime if we retry , we will get latest data .


**When to use SQL/NoSQL**

- If we want flexible query funcionality - *Choose SQL becasuse NOSQL doesnt support complex query* .
- If Data is relational (too many hierarchy among data ) -  *SQL*
- If data integrity is imporant - *SQL  , if we cannot compromise consistency even for a slightest bit. e.g In banks , financial instituions .*
- NoSQL is made to handle large data so losing one two recorrds in billions , doesnt matter in nosql.
- Availability or High Performance with some inconsistency - *NoSQl , searching capability is very high .*
