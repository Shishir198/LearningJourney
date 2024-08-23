<h1>Thread Local Class</h1> 

Credits - https://www.youtube.com/watch?v=pXVEEfYMwws | Shreyansh Jain

![image](https://github.com/user-attachments/assets/044c82ee-646d-4a52-a704-8807e964949c)

- Whenever we create a thread each thread has this class thread local class object in it.
- each thread got a thread local variable where we can store certain data
- thread local variable hold the value for a particular thread it can hold a string it can hold integer it can hold any object because it's a generic type.

![image](https://github.com/user-attachments/assets/c9dd5a14-5326-4bfa-b0fd-94c69d27ed32)

- Only one time we haev to create ThreadLocalClass object, then inside whichever threads we call 'set' function
- It will store the data against that thread.
- set function internally fetches the current thread and maintains a ThreadLocalMap to map data and thread against each other.


![image](https://github.com/user-attachments/assets/1500d3fe-332b-426e-a529-1bfd04aea78f)

- Remeber to clean up the data , when resuing the threads.
- Otherwise when the thread will pick up a new tasks , it will still be resuing the old data set during the previous tasks

To clean up : 
![image](https://github.com/user-attachments/assets/9a5f0751-534c-4f02-867a-6ff4579672a1)


<h1> Virtual Thread and Normal Thread(Platform Thread)</h1>

- Untill now we have seen are platform threads , so these threads are managed by JVM.
- They are just wrapper , whenever we create a platform thread JVM requests OS to create a native thread.
- Its just a wrapper around OS threads which then JVM manages.(mediator).

- So if we create 10 threads , is it one to one attached by platform thread and OS thread.

- Disadvantage - its slow , thats why we used executor as thread creation takes time.
  Why slow ? 
- Java calls OS , system call (so it takes time) expensive.

- DIsadvantage - when the thread does a DB call or IO call , e.g if its wait for 4 seconds , due to one to one mapping
- OS thread is also waiting for 4secs , it might have done other tasks otherwise.

Solution - Virtual Threads (JDK 19)

![image](https://github.com/user-attachments/assets/7a49d31e-aded-4f68-8c6d-4a87e623943d)

- There will be no one-one fixed mapping for the platform and OS thread 
- Now , virtual threads will created and manage by JVM, and those threads will get assigned to OS threads
only the time of execution .

- If the threads goes to waiting state for io call , then the link frm os thread will get detached and will be assigned to 
another thread which wants to execute.
- These are managed by JVM , so we dont have control on how many OS threads we can create , we can only create virtual thread and JVM will manage which to assign or not.

- Creation of Virtual Thread

- ![image](https://github.com/user-attachments/assets/0c31f4fb-cebb-4e61-8302-6efbef6bf9c1)
