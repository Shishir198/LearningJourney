<h1>Futures, Callables, and CompletableFutures in Java</h1>

- In Java, Futures, Callables, and CompletableFutures are key components of the concurrency framework. They are used to handle asynchronous computations, enabling programs to execute tasks concurrently, which can lead to more efficient and responsive applications.

1. Future
- A Future represents the result of an asynchronous computation. It acts as a placeholder for a value that will eventually be available once a task is completed. The Future interface provides methods to check if the task is completed, wait for its completion, and retrieve the result of the computation.

- Key Methods of Future:

- boolean isDone(): Returns true if the task is completed.
- boolean isCancelled(): Checks if the task was cancelled before completion.
- V get(): Waits for the task to complete and retrieves the result.
- V get(long timeout, TimeUnit unit): Waits for a specified time for the task to complete and retrieves the result, or throws a TimeoutException.
- boolean cancel(boolean mayInterruptIfRunning): Attempts to cancel the task.

- Limitations of Future:

- Once a Future is created, you cannot manually complete it.
- There are no built-in mechanisms to handle dependencies between multiple Future objects.
- Blocking on the get() method until the computation is done can lead to thread blocking and reduced efficiency.


2. Callable
- A Callable is a functional interface that represents a task that can be executed asynchronously and is expected to return a result. It is similar to Runnable, but unlike Runnable, a Callable can return a value and throw a checked exception.

- Key Characteristics of Callable:

- The Callable<V> interface has a single method: V call() throws Exception.
- It can return a result of type V and may throw an exception.
- Callables are usually executed by a thread pool using an ExecutorService, which returns a Future representing the result of the computation.

Example:
```
Callable<Integer> task = () -> {
    // Simulate long-running computation
    Thread.sleep(1000);
    return 42;
};

ExecutorService executor = Executors.newFixedThreadPool(1);
Future<Integer> future = executor.submit(task);
Integer result = future.get(); // Blocks until the task is completed

```

3. CompletableFuture
- CompletableFuture is a more advanced feature introduced in Java 8 as part of the java.util.concurrent package. It represents a Future that can be explicitly completed and can be used to build complex asynchronous pipelines.

- Key Features of CompletableFuture:

- Completion: Unlike Future, a CompletableFuture can be manually completed using methods like complete(value) or completeExceptionally(exception).
- Non-blocking: It provides non-blocking methods such as thenApply(), thenAccept(), thenCompose(), and thenCombine() for chaining tasks and handling results asynchronously.
- Combining Futures: It supports combining multiple asynchronous tasks using methods like thenCombine(), allOf(), and anyOf().
- Exception Handling: Built-in methods for handling exceptions, like exceptionally() or handle().
- Parallel Execution: It supports parallel execution of tasks, making it easier to run multiple asynchronous operations concurrently.
Example:
```
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
    // Simulate long-running computation
    return 42;
});

CompletableFuture<Integer> processedFuture = future.thenApply(result -> result * 2);

processedFuture.thenAccept(result -> System.out.println("Result: " + result));
// Non-blocking call, can be chained
```


- Benefits of CompletableFuture:

- Asynchronous Execution: Allows for creating non-blocking, asynchronous tasks without explicitly managing threads.
- Composability: Enables combining multiple asynchronous computations easily.
- Declarative Style: Encourages a more functional and declarative programming style for handling asynchronous tasks.


- Summary
- Future: A placeholder for the result of an asynchronous computation, with basic methods for checking and retrieving results.
- Callable: A task that returns a result and can throw an exception, typically used with ExecutorService.
- CompletableFuture: A powerful and flexible future that can be manually completed, composed, and chained, supporting complex asynchronous workflows.

- Each of these components serves different purposes in Java's concurrency model, providing varying levels of control and flexibility for handling asynchronous programming.
