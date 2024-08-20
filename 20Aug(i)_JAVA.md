<h1>Thread Basics and Lifecycle</h1>

Basics of Threads 
     * Creating Threads
         * Extending the Thread Class
         * Implementing the Runnable Interface
     * Thread Lifecycle
         * New
         * Runnable
         * Blocked
         * Waiting
         * Timed Waiting
         * Terminated


This overview covers fundamental concepts about creating and managing threads in Java. Let’s break down each topic:

Creating Threads
Extending the Thread Class

Definition: You can create a thread by defining a new class that extends the Thread class and overriding its run() method.
Usage: The run() method contains the code that will be executed by the thread.
Example:
```
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running.");
    }
}


public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start(); // Starts the thread
    }
}
```
Details: When you call start() on the thread instance, it invokes the run() method in a new thread of execution.
Implementing the Runnable Interface

Definition: Another way to create a thread is by implementing the Runnable interface, which has a single method run().
Usage: The Runnable interface is more flexible and allows you to extend other classes as well.
Example:

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable is running.");
    }
}

public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start(); // Starts the thread
    }
}
```
Details: You create a Thread object with a Runnable instance and then start the thread. This way, you can separate the task (represented by Runnable) from the thread management.
Thread Lifecycle
New:

Definition: The thread is in the “New” state when you create an instance of the Thread class, but before you call start().
Example:
java
Copy code
Thread thread = new Thread(new MyRunnable()); // Thread is in New state
Runnable:

Definition: A thread enters the “Runnable” state after you call start() on it. The thread is now eligible for running, but the actual execution depends on the thread scheduler.
Example:
java
Copy code
thread.start(); // Thread is in Runnable state
Blocked:

Definition: A thread is in the “Blocked” state when it’s waiting for a monitor lock (e.g., trying to enter a synchronized block) that another thread holds.
Example: If a thread tries to enter a synchronized block that another thread has already entered, it will be blocked until the other thread exits the synchronized block.
Waiting:

Definition: A thread is in the “Waiting” state when it’s waiting indefinitely for another thread to perform a particular action (e.g., calling wait() on an object).
Example:
java
Copy code
synchronized (object) {
    object.wait(); // Thread goes into Waiting state
}
Timed Waiting:

Definition: A thread is in the “Timed Waiting” state when it’s waiting for a specified period (e.g., calling sleep(), join() with a timeout, or wait() with a timeout).
Example:
java
Copy code
Thread.sleep(1000); // Thread goes into Timed Waiting state for 1 second
Terminated:

Definition: A thread is in the “Terminated” state after it has completed its execution (i.e., the run() method has finished) or if it has been terminated due to an exception.
Example: Once the run() method completes, the thread enters the Terminated state.
Summary
Creating Threads:

Extending Thread Class: Override the run() method.
Implementing Runnable Interface: Implement run() and use it with a Thread instance.
Thread Lifecycle:

New: Initial state before starting.
Runnable: After calling start().
Blocked: Waiting for a monitor lock.
Waiting: Waiting indefinitely for a condition.
Timed Waiting: Waiting for a specified time.
Terminated: After execution completes.





<h2>Difference between Sleep and Wait.</h2>


In Java, both sleep and wait are methods that can cause a thread to pause its execution, but they serve different purposes and have different behaviors. Here’s a breakdown of their differences:

Thread.sleep()
Purpose: Thread.sleep() is used to pause the execution of the current thread for a specified amount of time. It’s often used when you want to delay a thread’s execution or to simulate some kind of waiting period.

Usage: This method is called on the Thread class. It takes two parameters: the number of milliseconds and optionally, nanoseconds to sleep.

```
try {
    Thread.sleep(1000); // Sleep for 1 second
} catch (InterruptedException e) {
    e.printStackTrace();
}
```
Behavior: When a thread calls sleep, it does not release any locks or monitors. The thread simply pauses for the given time and then resumes execution. It’s important to handle InterruptedException, which is thrown if another thread interrupts the sleeping thread.

Scope: It only affects the thread that calls it and does not interact with other threads or the thread’s synchronization state.

Object.wait()
Purpose: Object.wait() is used for thread synchronization and communication. It makes the current thread wait until another thread invokes notify() or notifyAll() on the same object.

Usage: This method is called on an object, not on the Thread class. It must be called from within a synchronized block or method on that object.

```
synchronized (object) {
    try {
        object.wait(); // Wait until notified
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}

```
Behavior: When a thread calls wait, it releases the lock or monitor it holds on the object and enters a waiting state. It remains in this state until another thread calls notify() or notifyAll() on the same object or until it is interrupted. This is essential for thread communication in concurrent programming.

Scope: It’s used in the context of inter-thread communication and synchronization, often in producer-consumer scenarios or in other scenarios where threads need to coordinate their actions.

Summary
Thread.sleep(): Pauses the thread for a specified time but does not release locks or interact with synchronization mechanisms. It’s more about introducing a delay.
Object.wait(): Used for thread synchronization and communication; it releases the lock on the object and waits until it’s notified by another thread.
Understanding these differences is crucial for effectively managing thread behavior and synchronization in Java applications.





<h2>Why STOP, SUSPEND method deprecated</h2>


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


