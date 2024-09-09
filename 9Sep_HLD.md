<h1>Back-of-the-Envelop Estimation</h1>

- Back-of-the-envelope estimation is a quick way to make rough calculations in system design.
- It helps you get a basic idea of whether a system will work or not before spending time on detailed planning.
- This step allows you to figure out things like how much storage, computing power, or network capacity you’ll need, helping you avoid major mistakes early on.
- Essentially, it's a way to check if your design ideas are realistic.

- Without this step, jumping directly into designing the system can lead to significant issues.
- The system may be under-engineered, meaning it can't handle expected traffic, causing performance bottlenecks.
- Alternatively, it may be over-engineered, where unnecessary resources are allocated, leading to higher costs and wasted infrastructure.
- We should be able to reason each and every component (Load balancer , cache size , DB size tec) used in design according to the system constraints and requirements.

- THIS DRIVES OUR DECISIONS OF SYSTEM DESIGN.

**Typical metrics to focus on include:**
- Number of users (daily, monthly, peak load)
- Data storage requirements (e.g., per user, per request)
- Network bandwidth (data transferred per second)
- Server capacity (requests per second, CPU, memory)

**Basic Assumptions**
- char - ASCII(1Byte) | Unicode(2Bytes)
- Integer/long - 8bytes
- Images - 300KB (average)


**Important resources to compute**
- No of servers
-  RAM
- Storage
- and then tradeoff according to (CAP theorem)

**Trick to compute easily**

![image](https://github.com/user-attachments/assets/0d2a73a2-1f5b-462b-9637-157df1b46d07)
- for Storage
- if 'x' Million(6 zeroes) users X 'y' MB(6zeroes) =  (6+6 = 12 zeroes ~ TB) so , xy TB.

- if 5Million users X 2KB = (6+3) = 9 zeroes ~ GB so ,  (5*2)GB ~ 10 GB.



<h2>Let's do estimattion for Facebook </h2>

**Traffic :**

Total users : 1 Billion 
DAU (Daily active users) : 25% of Total users ~ 250 Million users
Queries / day - Let's say , one user does read-(5) and write-(2) ~ we get 7 operations per user.

Daily requests /second = (250 Million users/day * 7 ops) / (60 * 60 * 24 secs) == ~ close to 18K query per sec.

**Storage :** 

*Assumptions:*
- Every user is doing two posts , each post (250 characters).
- 10% user uploading 1 image.(300KB)

- so , One post - 250 characters ~ 250 * 2B(Unicode char) = 500 B. Two posts- 1000B ~ 1KB.

- DAU - 250M * 1KB = (6+3 )9 zeroes ~~ 250 GB/day.

- Similary , for images = 7500 GB ~ 8 TB/ day 

- Now if we want to store the data for 5 years (2000days)= 2000* 250GB = 500 TB and for images = 16 PB

**RAM estimation :**

- for each DAU - if we cache last 5 posts (2500 Bytes)

- 250Million * 2500 Bytes~(3KB) = 750 GB required.

- If one machine can hold 75GB , we need 10 instances for caching . 
 
**Latency :**
- Want to serve 1 request in 500ms in 95% of time.
- 18K queries/sec and one machine can serve 50 threads .
- If i am serving 1req in 500ms that means '2' requests in 1 sec = (2*500ms).
- So if 1sec - 2 requests and server has 50 threads we can serve 100 requests/sec.

- For 18K requests , we will need 180 application servers.


- So we have decided , no of servers , ram and storage through estimation .

- **Now , tradeoffs with CAP Theorem.**

- AS facebook is a social media platform , we can trade consistency with availability .

- SO we would choose AP.

**Some Considerations :**

- Dont spend a lot of time in estimations , as they are only rough ideas .
- Always use easy number like multiples of 10 , like 300GB and not 275GB.
