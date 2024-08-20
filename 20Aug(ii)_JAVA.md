<h1>Synchronization mechanisms in Java:</h1>

<h2>ReentrantLock, ReadWriteLock, StampedLock, Semaphore, and Condition (await, signal, signalAll)</h2>

<h2>Reentrant Lock</h2>

-  It offers the same basic behavior as the implicit monitor lock provided by synchronized blocks, with additional features.
-  A thread can acquire the lock it already holds without blocking, hence the term "reentrant."
- For example, if a thread enters a method protected by a ReentrantLock and tries to re-enter that method or another method that requires the same lock, it won’t deadlock. The lock maintains a hold count, incrementing it with each acquisition and decrementing it with each release.


```
ReentrantLock lock = new ReentrantLock();

public void outer() {
    lock.lock();
    try {
        inner(); // This method also acquires the lock
    } finally {
        lock.unlock();
    }
}

public void inner() {
    lock.lock();
    try {
        // critical section
    } finally {
        lock.unlock();
    }
}
```

 - A ReentrantLock can be created with a fairness policy. A fair lock favors granting access to the longest-waiting thread, reducing thread starvation. This is in contrast to the default behavior, which is non-fair, where the lock is given to any thread that requests it, potentially leading to unfairness.
 - Unlike synchronized, ReentrantLock.lockInterruptibly() allows the lock acquisition to be interrupted, making it more responsive in scenarios where a thread might need to be interrupted while waiting for a lock.


<h2>ReadWrite Lock</h2> 

- ReadWriteLock is an interface that defines a pair of locks: one for read-only operations and one for writing operations.
- Read Lock: Multiple threads can acquire the read lock simultaneously, as long as no thread holds the write lock. This allows multiple readers to access a resource concurrently, improving performance in read-heavy scenarios.
- Write Lock: Only one thread can acquire the write lock, and it prevents all other threads from acquiring either the read or write lock until the write lock is released.

```
ReadWriteLock lock = new ReentrantReadWriteLock();
Lock readLock = lock.readLock();
Lock writeLock = lock.writeLock();

public void readResource() {
    readLock.lock();
    try {
        // perform read operations
    } finally {
        readLock.unlock();
    }
}

public void writeResource() {
    writeLock.lock();
    try {
        // perform write operations
    } finally {
        writeLock.unlock();
    }
}
```

<b>Advantages:</b>

- Concurrency: It significantly improves concurrency in applications where reads are much more frequent than writes, as readers do not block each other.
- Performance: It is particularly useful in scenarios where you have many threads reading data and only occasionally writing, thus reducing contention and improving performance.

<b>Limitations:</b>

- Write Preference: If write locks are held frequently or for long periods, readers may be starved and blocked indefinitely, leading to performance bottlenecks.

<b>Use Cases:</b>

- Caching: Managing read-heavy caches where multiple threads need to read data concurrently while occasionally updating the cache.
- Shared Data Structures: Situations where a data structure is frequently read by many threads but only occasionally updated.

<h2>Stamped Lock</h2>
- StampedLock is a lock introduced in Java 8 that provides an alternative to ReentrantReadWriteLock with additional features and improved performance, especially under contention.

- How it Works:

- Stamping: It returns a stamp (a long value) that represents the lock state, which must be passed back to unlock or convert the lock.
  - There are three main operations:
- Read Lock: long stamp = lock.readLock() returns a stamp that can be used to release the lock later.
- Write Lock: long stamp = lock.writeLock() similarly returns a stamp for write operations.
- Optimistic Read: long stamp = lock.tryOptimisticRead() allows a thread to read data optimistically, without acquiring a full read lock. After reading, the thread can check if the stamp is still valid, ensuring no write occurred during the read.

```
StampedLock lock = new StampedLock();

public void optimisticRead() {
    long stamp = lock.tryOptimisticRead();
    try {
        // perform read operations
        if (!lock.validate(stamp)) {
            // Re-acquire the lock if a write occurred
            stamp = lock.readLock();
            // perform read operations again
        }
    } finally {
        lock.unlock(stamp);
    }
}

```
- Advantages:

- Optimistic Locking: This feature allows for non-blocking reads, where threads can perform a read operation and only validate the lock state afterward, leading to significant performance gains, especially in low-contention scenarios.
- Flexibility: It allows more nuanced control over locking, such as upgrading from an optimistic read to a full read lock if contention is detected.


- Limitations:

- Complexity: StampedLocks are more complex to use correctly due to the need to handle stamps and validate them.
- No Reentrancy: Unlike ReentrantLock, StampedLock does not support reentrancy, meaning a thread cannot re-acquire a lock it already holds.

- Use Cases:

- Low Contention Reads: In systems where reads are predominant and contention is low, StampedLock can provide significant performance improvements.
- Fine-grained Locking: Situations where you need the flexibility of optimistic reads combined with the ability to upgrade to a full lock when necessary.

<h2>Semaphore Lock</h2>

- Semaphore is a signaling mechanism that controls access to a shared resource by maintaining a set of permits.
- It can be used to restrict the number of threads that can access a particular resource simultaneously.

- How it Works:

- A Semaphore maintains a count of available permits.
- Threads can acquire a permit by calling acquire() or release it by calling release().
- If no permits are available, the acquiring thread will block until a permit is released by another thread.
- Semaphores can be initialized with a fixed number of permits, which controls the concurrency level.

```
Semaphore semaphore = new Semaphore(3); // 3 permits

public void accessResource() {
    try {
        semaphore.acquire(); // acquire a permit
        // access the resource
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    } finally {
        semaphore.release(); // release the permit
    }
}
```

Types of Semaphores:

- Binary Semaphore: Also known as a mutex, it only has two states: 0 (unavailable) or 1 (available). It's often used to protect a critical section.
- Counting Semaphore: A general semaphore that allows more than one thread to access the resource simultaneously, based on the number of permits.

Advantages:
- Concurrency Control: Semaphores are useful for controlling access to a finite number of resources, like limiting the number of database connections or controlling access to a pool of threads.
- Coordination: They can be used for signaling between threads, ensuring that certain tasks are performed in order.

Limitations:

- Overhead: Using semaphores involves some overhead in terms of managing permits, especially if the semaphore count is large.
- Complexity: Semaphores can introduce complexity in program logic, especially when dealing with multiple threads and resources.

Use Cases:
- Resource Pooling: Managing a pool of resources where a fixed number of threads can access a resource at any given time.
- Rate Limiting: Controlling the rate at which threads access a particular resource, like limiting the number of concurrent API calls.

5. Condition (await, signal, signalAll)
- Condition objects, used in conjunction with Lock objects (like ReentrantLock),
- provide a way to manage thread communication and synchronization beyond what Object.wait() and Object.notify() can offer.

How it Works:

- Condition objects are associated with a Lock, and they allow threads to wait for a specific condition to be met before proceeding.

Key methods:
- await(): Causes the current thread to wait until it is signaled or interrupted.
- signal(): Wakes up one waiting thread.
- signalAll(): Wakes up all waiting threads.
```
ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();

public void waitForCondition() {
    lock.lock();
    try {
        while (!conditionIsMet()) {
            condition.await(); // wait for the condition to be signaled
        }
        // proceed with execution once the condition is met
    } finally {
        lock.unlock();
    }
}

public void signalCondition() {
    lock.lock();
    try {
        // update the condition state
        condition.signal(); // signal one waiting thread
    } finally {
        lock.unlock();
    }
}
```
Advantages:

- Multiple Conditions: Unlike the synchronized block's wait-notify mechanism, where there is only one wait set per object, Condition allows you to create multiple wait sets per lock, enabling finer control over thread coordination.
- Timeouts: The await() method has overloaded versions that accept timeouts, allowing threads to wait for a condition with a time limit.

Limitations:

Complexity: Using Condition objects requires careful design to avoid missed signals or deadlocks, especially in complex multithreaded environments.
Use Cases:

Producer-Consumer: In scenarios where threads need to wait for a certain condition to be true before proceeding, such as in producer-consumer problems.
Complex Synchronization: When you need more control over the wait/notify mechanism, particularly when multiple conditions must be handled within the same lock.


Summary
ReentrantLock: Offers more flexibility than synchronized, with features like fairness, interruptibility, and condition variables.
ReadWriteLock: Improves concurrency by allowing multiple readers or a single writer, ideal for read-heavy workloads.
StampedLock: Provides an alternative to ReadWriteLock with better performance under contention, especially with optimistic reads.
Semaphore: Controls access to a resource by limiting the number of concurrent threads, useful for managing resource pools or rate limiting.
Condition: Extends the capabilities of traditional wait/notify by allowing more complex thread communication and synchronization within a Lock.
