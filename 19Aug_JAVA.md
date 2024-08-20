<h1>Multithreading </h1>

Credits - Concept and COding by Shreyansh Jain | https://youtu.be/TpYIcJN9EV8?si=umzOwNM_IFc8PNjg

![image](https://github.com/user-attachments/assets/137e8e23-c98c-4bc8-b21d-5201ee6f01f9)

![image](https://github.com/user-attachments/assets/a8a39d8f-60f8-4d05-a7f8-9d33c1839848)

![image](https://github.com/user-attachments/assets/c1b13e6d-6d86-4344-8bd9-2e1dacaf4f3c)

![image](https://github.com/user-attachments/assets/cf7de84e-0425-45cd-aa5c-cf6ae549026b)
![image](https://github.com/user-attachments/assets/34152335-bd23-4545-8692-51b018c2f4e3)

![image](https://github.com/user-attachments/assets/e84527c5-bdbd-4a65-98d8-e4818f68be25)

![image](https://github.com/user-attachments/assets/baf5c9b3-33f5-442d-98c6-b3fc9e76609e)

- JVM has slots of memory like heap memory , stack memory , code segemnt , data segment , registers and progam counters.
- Whenever a new process is created , a new JVM instance is allocated to that process .
  
  ![image](https://github.com/user-attachments/assets/6155c375-76f0-498a-b8ad-fe8993390aac)

  ![image](https://github.com/user-attachments/assets/5a1900e9-86fe-4244-97f2-17fed474606a)


  ![image](https://github.com/user-attachments/assets/a1abc151-1068-46e5-91d6-b0dd2c018594)

  ![image](https://github.com/user-attachments/assets/4fc350e8-913a-4982-a50c-3970690871b3)

  ![image](https://github.com/user-attachments/assets/c3678235-7d63-4172-b2ed-f8f696092859)

  ![image](https://github.com/user-attachments/assets/966afe3f-7c7d-4b5d-84f4-873f488b3754)

  ![image](https://github.com/user-attachments/assets/bdd52519-a7ee-4314-894e-544b3c2a3f5a)

Multitasking - 
- Creation of two different processes which doesnt share common resources .Program executes multiple processes by 
using context switching

Multithreading-
- inside a process , there are different threads running to complete the tasks using shared resources.



Why STOP, SUSPEND method deprecated


The stop() and suspend() methods in Java's Thread class were deprecated mainly due to safety concerns. Here's a more detailed explanation:

1. Thread.stop()
- Unsafe Termination: The stop() method immediately terminates a thread without giving it a chance to release locks, close resources, or complete its work. This abrupt termination can leave shared resources, like files or data structures, in an inconsistent state, leading to deadlocks or data corruption.
- Exception Propagation: When stop() is called, it causes a ThreadDeath exception to be thrown, which can be caught and ignored, making it hard to guarantee that the thread will actually stop.
- Alternatives: The recommended approach is to use a boolean flag that the thread checks periodically, allowing it to terminate gracefully.
2. Thread.suspend()
- Deadlock Prone: The suspend() method pauses a thread, but if the thread holds a lock on a resource, other threads might be blocked indefinitely waiting for that resource. If the thread is never resumed, this leads to a deadlock.
- Inconsistent State: Suspending a thread can leave objects in an inconsistent state if the thread is interrupted in the middle of a critical section, causing unpredictable behavior.
- Alternatives: Instead of suspend(), it's better to design the thread logic to wait using wait(), sleep(), or join() methods, or to 
    use higher-level concurrency utilities like Semaphore or Lock.
- Summary
- These methods were deprecated because they can cause threads to behave unpredictably, potentially leading to deadlocks, data corruption, or other synchronization issues. Java encourages using safer alternatives that give the developer more control over thread lifecycle management.


<h2> Thread Joining , Thread Priority & Daemon Thread</h2>

- Thread Joining
Joining a Thread is a way to ensure that one thread waits for the completion of another thread before it continues its execution. The method used for this is join().

How it Works:

When you invoke join() on a thread, the calling thread (the thread that invoked join()) will be paused until the thread on which join() was called has finished executing.

For example:
```
Thread t1 = new Thread(() -> {
    // Some lengthy task
});

t1.start(); // Start the thread
try {
    t1.join(); // Wait for t1 to finish
} catch (InterruptedException e) {
    e.printStackTrace();
}
// Code here will not execute until t1 has finished
```
This mechanism is useful when one thread depends on the outcome of another, ensuring that tasks are executed in the correct order.
Overloaded Methods:

join(long millis): The calling thread waits at most millis milliseconds for the thread to finish.
join(long millis, int nanos): In addition to milliseconds, you can specify extra nanoseconds.
If the time runs out before the thread finishes, the calling thread will resume without waiting further.
Use Cases:
- Coordination: When threads need to synchronize their execution, like in a parallel computing task where you need all worker threads to finish before processing the results.
- Waiting for Setup: If one thread is setting up resources that another thread depends on, join() ensures the setup is complete before proceeding.
  
2. Thread Priority
- Thread priority in Java is a mechanism to hint the thread scheduler about the importance of a thread relative to others. However, it's important to note that thread priorities are largely system-dependent and do not guarantee the order of execution.

How it Works:

Each thread has a priority, represented by an integer value between Thread.MIN_PRIORITY (which is 1) and Thread.MAX_PRIORITY (which is 10), with Thread.NORM_PRIORITY (5) as the default.
The thread scheduler may choose to give more CPU time to higher-priority threads, but it does not guarantee that higher-priority threads will always run before lower-priority ones.
Example:
```
Thread t1 = new Thread(() -> {
    // Task for t1
});

Thread t2 = new Thread(() -> {
    // Task for t2
});

t1.setPriority(Thread.MIN_PRIORITY); // Low priority
t2.setPriority(Thread.MAX_PRIORITY); // High priority

t1.start();
t2.start();
```
Limitations:

Platform Dependency: The effectiveness of thread priority is platform-dependent. Some operating systems may ignore Java thread priorities.
Non-deterministic: Even with different priorities, the actual order in which threads execute is not guaranteed. The scheduler may not always respect the priorities, especially in a multi-core environment.

<h6>Use Cases:</h6>

- Optimizing Performance: In certain real-time applications, you might use thread priorities to give preference to more critical tasks.
- Fair Resource Allocation: You might want to prioritize threads handling user interface interactions over background tasks.

3. Daemon Thread
- A Daemon Thread in Java is a low-priority thread that runs in the background to perform tasks such as garbage collection, background monitoring, etc. The JVM doesn't wait for daemon threads to finish before exiting; it will terminate all daemon threads when the last non-daemon thread completes.

How it Works:

You can mark a thread as a daemon by calling setDaemon(true) before the thread is started.
Daemon threads are typically used for background services that should not prevent the JVM from exiting if no other user threads are running.
Example:
```
Thread daemonThread = new Thread(() -> {
    while (true) {
        // Background task, e.g., monitoring
    }
});

daemonThread.setDaemon(true);
daemonThread.start();
```
The JVM will terminate daemonThread once all non-daemon threads have completed.
Characteristics:

Background Service: They provide services or perform tasks without interfering with the main application logic.
Uncertain Execution: Since the JVM may terminate a daemon thread abruptly, they should not be used for tasks that require a guaranteed execution or proper cleanup.
Use Cases:

Garbage Collection: The JVM’s garbage collector is typically implemented as a daemon thread.
Background Tasks: Any task that should run in the background, like periodic monitoring, logging, or clean-up activities, can be a daemon thread.


Summary
- Thread Joining ensures one thread completes before another continues, useful for task coordination.
- Thread Priority provides a way to suggest which threads should be given more CPU time, though its effectiveness is not guaranteed.
- Daemon Threads run in the background and do not block JVM termination, suitable for background tasks that don’t need to finish before the program







