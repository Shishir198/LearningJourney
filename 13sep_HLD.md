<h1>Design Idempotent POST API</h1>

Credits - https://www.youtube.com/watch?v=mI73eTlSqeU

![image](https://github.com/user-attachments/assets/b19ed8ae-b88b-4507-a9ea-a61135857c29)


- There is difference between idempotency and concurrency .

Concurrency
- 
- When multiple requests are being made for single resource e.g Booking same seat while paying for movie ticket in BookMyShow.
- This is concurrency issue.


Idempotency 
- 
- Its take care of duplicate requests.
- Idempotency refers to the property of an operation where performing it multiple times has the same effect as performing it once.
- In other words, no matter how many times an idempotent operation is executed, the result remains the same, and the system state does not change after the first execution.
- When a user confirms a booking, the operation should be idempotent. If the user accidentally clicks the "Confirm" button multiple times (or if the request is retried due to network issues), the system should only process the confirmation once.
- Subsequent requests with the same bookingId should not result in multiple confirmations or additional charges.
- The booking status will only change once, and the system remains in the same state after the first successful confirmation.

- It enables user to retry many times safely , without worrying about side effects of the operation.

- By default , get put delete are idempotent , but not post.

- If we continue multiple get calls , it will not put any effect on DB it will return the same value .
- PUT - if we put requests multiple time to change ("a" , "b") , it will change first time , but further requests will not cause effects on db as its already changed .

- But POST , if we make payment transaction from A to B , for Rs.10 .
- post (A - B , rs10) ,db will create a row in it and further transaction will occur trasnfer of rs10 from a to b.
- If user tried same multiple requests again as retry , it will again create row in  and again trasnfer rs10 from a to b.



Approach to handle
-
- USING IDEMPOTENCY KEY (IK), which will be a unqiue id (UUID) (universal unique id)

- Client should generate a idemtotency key .
- For each different operations a different key should be generated .

- Now.Lets take a scenario , where user is trieng to add product to cart

- ![image](https://github.com/user-attachments/assets/17dc976f-129f-4508-bb4c-b4565830990b)

- User will make a post requests in the client , which will be generate IK and it will store it in the header of request and calls the server .

- Server received the requests will follow the below process.

- Validation : It will check is there any IK present on Header -- If no - HTTP 400 ERROR (Validation ERROR)
- IF YES - server will check if the IK is present on DB .

- ![image](https://github.com/user-attachments/assets/5eb5103c-fbcd-4e58-8ed2-6725c977741c)

- IF key not present - It will create a entry of [IK , STATUS] in the DB , and the status can be 'CREATED'.
- If operation is success (E.g item is added to cart) - it will consume the IK and status will be 'CONSUMED' , response - 201 Resource Created

- ![image](https://github.com/user-attachments/assets/db615b07-a298-47b4-9ebc-01aab3e43511)

- If key already present - (duplicate request) / case of retry .

- IF IK STATUS - CONSUMED -------> means the operation is already performed , reponse - HTTP 200 (requests success but resource is not created).
- IF IK STATUS - CREATED --------> means the previous request is under processing , response - HTTP 409 (conflict with request).


- So , in  below retry case this will work fine 
![image](https://github.com/user-attachments/assets/437a3d4b-f7d9-4ba8-ba9f-821afe050235)




- WHAT IF Duplicate requests if encountered parallely at same time may be due to another server.
  ![image](https://github.com/user-attachments/assets/7e335fb7-2cc4-44a6-8dd2-28ffbf40013a)


- IF requests come in parallel , it will pass validation IK in header or both and both will try to read the DB and create an entry which will be an issue .
- This is kind of critical section problem which can be solved by adding MUTUAL EXCLUSION , by using mutex or semaphores.
- Where only one requests will acquire the lock of DB , and will allow to perform the operation . 



- WHAT IF there are different DB's beign used for different cluster of servers .

![image](https://github.com/user-attachments/assets/4a13fe22-f5a0-4b4c-9d34-3cfc3b5fedf9)

- If the requetss will same IK gets to different server cluster based on geolocations etc . and have independent DB's .
- By the time sync happens between DB's , the validation steps of the requests will be passed for both the requests and both might have craeted entries to their DB.

- Solution - CACHE 
- because sync in cache will be much faster , in ms where as in DB its in minutes.

![image](https://github.com/user-attachments/assets/85aeb5bb-119a-4cce-a0e8-1cf1ef295167)
