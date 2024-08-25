<h1> CAP Theorem</h1>


Desirable property of distributed system with replicated data.

C - Consistency
A - Availability
P - Partition Tolerance

These 3 are desirable prooerties for a distrbuted system which can have replicated data.
E.g - We have application which can send request to DB node B and C and these nodes will store replicated data . (Data present in B will also be in C)
![image](https://github.com/user-attachments/assets/5ae2bd2e-557d-4c4f-9da1-422a62781594)

But , we cannot get all of these 3 property under same system .
![image](https://github.com/user-attachments/assets/c0650df7-afef-4651-9502-28085261301f)

CAP -All three cannot exists together , we have to select 2 out of three according to our requirment.
We have to think of trade offs and priority.

C - If application changes the data in Node b from a =4 to5 , then it should be reflected in Node C too , if fteched the same data in C we should get same result 
Thats consistency

A - All nodes should respond.

P - means system should be resilient , if something breaks user should be able to perform operations atleast.
![image](https://github.com/user-attachments/assets/43e534b9-9518-40b7-a2f0-a41e572c5d6e)

If there is a breakage between B and C , user should still be able to query.


Why we cannot have all three .


![image](https://github.com/user-attachments/assets/8d6c2be1-b615-42da-a2b9-f899b66974af)


- Now , lets take a scenario if I want my system to be consistent , available and partition tolerant .

- Lets say there is a breakage betweem B and C , not data updated in B cannot be synced to C , Altough the system is up (P) and I am not willing to down any of my node(A)
- but when data is fetched from C , it will fetch old data (inconsistency). 


- But if we want consistency and partition tolerant , in the above scenario we need to stop our Node C , so that it cannot take new requests and B will be fulfilling all requests.
- (means we need to trade "Availability" in these case).

- But if we want C and A (not willing to stop any node) , then if any breeakage happens , our system will need to go down , 
- because if allowed any requests it will read inconsistent data what we dont want so we need to stop our system(application) in that case .


- So we understand from this that we cannot trade P , if any communication breakage happens, (which is common) we do not want our application to shut down.
- So need to keep P as mandatory propperty , means we can trade off in terms of Consistency and Availability




