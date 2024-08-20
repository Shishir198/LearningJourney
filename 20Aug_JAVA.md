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


