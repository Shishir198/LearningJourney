<h1>Thread Pool and Thread Executor</h1>

![image](https://github.com/user-attachments/assets/ca6f3013-cba5-4eb1-86d4-e73ffba15523)

Previously we were creating the new threads by using 
Thread t1 = new Thread() and then t2 , t3... so on .
But thread creation is itself a time consuming process as it needs memory allocations etc.

SO , in thread pool , threads are already been created and can be reused . 

Any new tasks, just assign empty threads to the task which saves time .

![image](https://github.com/user-attachments/assets/7ec4f17a-f718-4dbe-b6a6-90b0f544cc48)

If task1 is assigned to thread 1 and task2 -> thread2 , then if a new task3 comes and all threads are busy 
we will store it in a queue , whenever a thread comes back to pool and check any tasks pending in queue 
and then task3 will get picked up.


![image](https://github.com/user-attachments/assets/4f7d9120-7b37-4461-963d-79a09959a25a)

Apart from time saving , Managing the thread lifecycle is not our headache now , it will be handled by framework.

if we create a new thread everytime a new tasks enters , we will have loads of threads
which means more context switching will occur which we dont want because during the context switching no effective tasks happends
it will decrease the throughput.

![image](https://github.com/user-attachments/assets/0c58be46-17a5-4095-97e9-34ebb634e631)

We have a framework for this - Executor Framework

![image](https://github.com/user-attachments/assets/c81de050-4ed7-48b2-b7a3-51e6258f2427)

Flow : 

If new tasks comes to executor and demands thread , first check of any thread avaialble it will assign 
other wise it will add the tasks in the queue.

![image](https://github.com/user-attachments/assets/9158a7e5-1b38-4cc2-9fb1-c47a15bb0b83)


It has feature like 
- min no of threads e,g -3
- max no of threads e.g - 5 (5 threads can be inside the pool)

Queue also has size .

Whenever there is no thread present and the queue is full too , 
it will check if current pool size <  max pool size is true ,
then it will create a new thread in the thread pool and assign to the newest task .

e.g - t1--> task1 , t2--task2 ,t3-- task3
and in queue(size = 5) [task4,task5,task6,task7,task8];
If task9 comes , a new thread t4 will be created and will get assigned to "task9".

If task10 , new thread t5 assigned to task10.

if task11 arrives - queue is also full and max no of threads is also reached .
then it has to reject that task.

![image](https://github.com/user-attachments/assets/0816e39f-9f21-4fb0-ad66-91d21a65cefe)

![image](https://github.com/user-attachments/assets/82a5311a-367a-43d4-b71d-5a2c105b4819)

Core pool size - min no of threads that shoudl be avaialble in the pool , even if they are idle.


Allow Core thread time out(TRUE/FALSE) 
- its  a property that states if the thread remains ideal for a particular period of time,delete that thread.
- that specific time is decided by KeepAliveTime , is set to TRUE.


![image](https://github.com/user-attachments/assets/b6ba7216-ea3f-46ba-92b8-df0f1b12aaa8)
Keep alive time only specified the magnitude , TimeUnit specifies the type - ms or second or hr



![image](https://github.com/user-attachments/assets/b9629a5f-7e3c-4f1a-bb9e-ac2b1131759b)


if min size is 3 and busy already , and new task4 comes why to store it in the queue . Why not create a new thread there 
itself and server task4 it will save time right ? 

- Minimum size of pool is set in such a mindset that , those threads will be sufficient most of the time on average.
- The new threads are being made when during the peek time , or more load than expected comes into the picture.
- If new thread is created for a new tasks without usinf queue , it will end up being idle from next time onwards because
- minimum thread were already enough to server average case.
- Thats why we store it in the queue first and only when more load arrives , it creates a new threads.


![image](https://github.com/user-attachments/assets/40657b13-a319-4d55-846e-1f0f6bf11336)



![image](https://github.com/user-attachments/assets/46472c96-bb5f-4d58-9af7-29a9621150e3)



![image](https://github.com/user-attachments/assets/4eb5e8c3-0565-4e31-acfe-95cbeaef41e2)



![image](https://github.com/user-attachments/assets/591b1ded-a984-4d89-be62-9c8e939e1a8f)


![image](https://github.com/user-attachments/assets/ffe15036-5eca-402b-9e8b-b34800217d99)



![image](https://github.com/user-attachments/assets/3760fb14-f370-44b3-a287-1c71a36b016b)

For shutdown , there is a method called shutdown().

For Stop (Forced Shutdown) there is a method called shutdownNow();


![image](https://github.com/user-attachments/assets/aa31fce3-abf1-479c-827b-510a031e92b8)

![image](https://github.com/user-attachments/assets/ae5b6923-1b43-46c5-a76d-6407f2cb1b12)



![image](https://github.com/user-attachments/assets/fef4d86d-c878-4327-92ee-4944675b44d7)

Implementing scenario 1 


![image](https://github.com/user-attachments/assets/af8ad6ca-52f2-423e-91c7-3ed95157b48f)



![image](https://github.com/user-attachments/assets/aa9dca73-9800-43d3-b211-107146d6db45)
- We can also use default thread factory, whose impl is same as above.


![image](https://github.com/user-attachments/assets/28fa8c72-8bdc-4a73-8ec1-0d4da273df5c)


![image](https://github.com/user-attachments/assets/50bd6440-71b5-4df1-8012-932ed8ccb4cc)

For rejection policy we pass like this 

![image](https://github.com/user-attachments/assets/d498baf7-b852-440d-8f88-eaf6691716ca)

We can alsu build custom one


![image](https://github.com/user-attachments/assets/f93d2ff9-294e-4426-b400-a8515559fa6e)


How to use : 
![image](https://github.com/user-attachments/assets/16cff823-8b41-4494-bfa0-1d98c68dabb8)

This is how we submit a task to executor , executor.submit accepts a Runnable which is passed by lamba exp.




![image](https://github.com/user-attachments/assets/1268c7da-4585-482e-8943-1a17b98c66b1)

Reasoning to decide :
What will be the minimum no of threads should be maintained

It is a subjective answer :


![image](https://github.com/user-attachments/assets/38530f91-7680-46ad-9b54-c207fb69f30d)


![image](https://github.com/user-attachments/assets/4b6e224f-bfb3-459b-8d5e-e3bd19143a3c)
Its not full proof , it will give the max value 

When its  CPU intensive - request waiting time << processing 
IO - request waiting time >> processing 


![image](https://github.com/user-attachments/assets/200cbfdd-1f57-4a19-a0fc-06a37932e7d2)


![image](https://github.com/user-attachments/assets/9121a61b-8db2-4bb6-9c6c-0f3dda0136c3)

![image](https://github.com/user-attachments/assets/94864dcf-917e-4f7a-91ae-815054f18ef5)

So , approx x= 500

This 500 mb we can use it for threads. 
Out of this , we have to create a stack, registers etc .

Lets say 1 thread - 5mb .
SO max threads - 100 threads.

Is this good ? 

memory required for process to request - 10mb(e.g)

Now if 100 threads are created and eacch will require 10mb of heap space to fulfull one request then 
100 * 10 = 1000mb ~ 1GB , so full heap space will be occupied , it will be risky again.

So we can take buffer , lets say 60 threads  so 600 mb will be used at a time


So min - 60  , max - 70 lets say.

Then we can do iterative monitoring , load testing and optimsise the number. 





















  



