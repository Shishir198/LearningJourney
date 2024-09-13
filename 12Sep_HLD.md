<h1>Rate Limiter</h1>

- It is a technique used to control the number of requests a user or system can make to a server within a given timeframe.
- It helps manage the load on servers and prevent abuse or misuse of resources like DDos attack.
- DDoS attacks aim to exhaust server resources (like CPU, memory, or bandwidth) , to overwhelm a server or network with a flood of malicious requests. 
- It is a key defense mechanism against Distributed Denial of Service (DDoS) attacks.


Algorithms to implement Rate limiter

- Token Bucket
- Leaking Bucket
- FIxed window counter
- Sliding window log
- Sliding window counter


<h2>Token Bucket </h2>

![image](https://github.com/user-attachments/assets/b408b466-b547-48d9-a833-fbaacd357fde)

- The Token Bucket algorithm uses a "bucket" to control the flow of data or requests.
- The bucket has a fixed capacity and holds "tokens" upto a capacity , lets say 4 .
- You can image Tokens: Tokens are like little coins that go into the bucket. Each token allows you to make one request or send a small amount of data.
- Tokens are added to the bucket at a constant rate by refiller  ,lets say in 1 min it will insert 2 tokens .
- If token capacity is full , the extra tokens will get overflowed , means they are removed.
- When you want to make a request or send data, you need a token. If there’s a token in the bucket, you can use it and the token is taken out. If there are no tokens, you have to wait until more tokens are added.
- If a lot of tokens are in the bucket, you can make several requests in a short time (a burst). If there are only a few tokens, you can only make a few requests at a time.

- In simple terms, the Token Bucket algorithm helps manage how much traffic can be sent, allowing for occasional bursts but keeping overall traffic within limits.

- We can configure the bucket for each user / api.

- E.g - 3 tokens /min for  API to post/tweet for each user.

- WE can maintain a counter against each user with a timestamp like user1 {counter : 3 , timestamp : 10:01AM}

- Whenever user does a post , we will degrage the counter , and if wihtin one minute user made 3 posts , the counter will be 0 .
- Now the user will not be able to make posts in same minute , the HTTP Response will be 429 : excess requests within time limit.
- After one minute the refiller will fill up the counter to 3 and then user will  be able to post after certain time .

  ![image](https://github.com/user-attachments/assets/1df19630-7811-4d83-9fac-b910736d978e)

- Token bucket can be simply implemenetd through counter.



<h2> Leaking Bucket</h2>

![image](https://github.com/user-attachments/assets/4d812e75-8222-4425-ae1c-c72474b9577b)

- Imagine a bucket with a small hole at the bottom. The bucket fills up when data or requests are added, and it leaks out slowly through the hole.
- The hole represents the constant rate at which data or requests can be processed.
- Bucket Capacity: This is the maximum amount of data or requests the bucket can hold. If the bucket fills up and reaches this capacity, any additional incoming data or requests will overflow or be dropped.
- Leak Rate: The rate at which data or requests leak out of the bucket. This rate is fixed and determines how fast the system can handle the data or requests over time.
- Overflow: If data or requests arrive faster than they can leak out of the bucket, they will overflow or be discarded once the bucket is full.

- This is implemented thourgh QUEUE.

- <h3>Advantages</h3>
- Simplicity: The Leaky Bucket algorithm is straightforward to implement and understand.
- Consistent Output Rate: It ensures a consistent output rate, smoothing out bursts of incoming traffic.

- <h3>Disadvantages</h3>
- No Burst Handling: Unlike the Token Bucket algorithm, the Leaky Bucket algorithm doesn’t handle bursts of traffic as well.
- If the bucket is full, incoming data is discarded, which can lead to loss of important data.
- It's not suitable for amazon where a burts can come during sale .

<h2>Fixed window counter</h2> 

- We have 24 hours time frame , which is divided into different window and a threshold of requetsts are set for each window.

- ![image](https://github.com/user-attachments/assets/3d384037-c6d0-4443-9a67-1a000b6120cc)

- Example - window size : 5mins , threshold : 3 . Each window gets 3 counters .

- It tracks the number of events or requests that occur within a fixed time window.
- If the number of events exceeds a specified limit during that time window, further events are either delayed or rejected until the next time window begins.

<h3>How It Works</h3>
- Counting Events: During each fixed time window, the system counts the number of incoming events or requests.

- Checking Against Limit: If the count of events within the current window is below the limit, the events are allowed. If the count reaches or exceeds the limit, additional events are blocked or delayed until the window resets.

- Window Reset: At the end of each fixed time window, the counter resets to zero, and counting starts again for the next window.

<h3>Advantages</h3>

- Simplicity: The Fixed Window Counter is easy to understand and implement. It uses a simple count within a time window to enforce limits.

- Predictability: It provides predictable behavior since the rate limit is enforced strictly according to the fixed time window.

<h3>Disadvantages</h3>

- Burstiness: The Fixed Window Counter can lead to problems with burst traffic.
- For example, if a user sends 100 requests right at the end of one window and 100 more at the beginning of the next, they effectively get 200 requests in a short period, despite the limit being 100 per window.

- Inefficiency at Boundaries: Because the window is fixed, the system may not handle requests efficiently right at the boundary of the window.


- The above problem is solved by sliding window log .

<h2>Sliding window log </h2>

- Sliding Window: This technique maintains a dynamic window that "slides" over time. It helps track the number of operations that have occurred within the most recent time frame.
- Log: This is a record of timestamps of the events (e.g., API requests) that have occurred.

<h3>How It Works</h3>

- Initialization: Define the size of the time window (e.g., 1 minute) and create an empty log to store timestamps of events.

- Add Event: Each time an event (e.g., an API request) occurs, record its timestamp in the log.

- Maintain the Window: Continuously remove timestamps from the log that fall outside the current time window to ensure that the log only contains events within the window.

- Rate Check: To determine if the rate limit is exceeded, simply count the number of timestamps in the log. If the count exceeds the allowed limit, the request is denied; otherwise, it is permitted.

<h3>Benefits</h3>

- Accurate Rate Limiting: Provides a precise way to enforce limits based on real-time data.
- Flexible: The sliding window approach adjusts to varying rates and is suitable for applications with bursty traffic patterns.
- Simple Implementation: Easy to understand and implement while offering effective rate limiting.

<h3>Disadvantage</h3>

- Even if the requetss is removed , but we still store its log as timestamp which consumes memory.


<h2>Sliding Window Counter</h2>

- It is combination of fixed window counter and sliding window log.

- E.g - if we need to limit 5 requests / min(window)

- Instead of tracking each individual request timestamp, you divide the minute into smaller intervals (buckets) and track the number of requests in each bucket.


-Lets say ,between  8:00 -- 8:01 , 6 requests arrived.
-    Now , 8:01 - 8:02 - This is our current frame  , so the 8:00 - 8:01 will be previous timeframe.
- We maintain a window  , lets say curretnly its in 8:00:50 - 8:01:50 , which has edge of both frames : Previous time frame's : 10 second and current frame's 50seconds

- Now let say , current frame already 2 requests arrived : count = 2.

- Now when a new requests will come within window [ 8:00:50 - 8:01:50] , it will check whether we crossed the limit . How ?

- current time frame of 50 second - 2 requests
- To calculate %requests of 10secs of previous timeframe -  in previous frame 1min(60second) - 6 requests arrived , so in 10 secs = ? (6/60*10).
- This is how it calculated the percentage and checks , whether we the new arriving requests will be fulfilled or not.

![image](https://github.com/user-attachments/assets/037984d1-4a46-443c-a689-cef541ac47bb)




HLD Decisions :

- E.g For token bucket : we need to maintain a counter which will increment very often so we cannot use disk or db , we need to use Redis(in memory database) for fast operations.

![image](https://github.com/user-attachments/assets/566f1820-e018-4472-8532-729a397efab7)

- We can implemnent rate limiter in API gateway itself .

![image](https://github.com/user-attachments/assets/f6a504ed-8d5b-4728-8018-34156886aa2d)

- Similary as counter we need to mantain a config file , which will store info for how many requets to allow per min , bucket size , timeframe etc.
- WE can store it in disk as its not needs to change frequently and do proper caching.

- Even in case of distributed rate limiting we can use centralized Redis DB .

- ![image](https://github.com/user-attachments/assets/80e99264-3f0b-4741-959d-ea981e85011a)

