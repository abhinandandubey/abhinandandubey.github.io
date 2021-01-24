---
layout: post
title: The Concurrency API
tags: Advanced-Java-Series Java Programming
cover_url: https://images.unsplash.com/photo-1602313015746-6e311e7657cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1398&q=80
cover_meta: 
  (c) UNSPLASH
color_scheme: zenburn
mathjax: true
mathjax: True
---
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>

<style>
blockquote.yellownote {
    border-left: 12px solid #dc0;
    background-color: #ffa;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
blockquote.sidenote {
    border-left: 12px solid #dc0;
    background-color: #ffa;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
blockquote.palenote {
    border-left: 12px solid #3d0c02;
    background-color: #ffe4c4;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>

The Java Concurrency API contains the following interfaces:

- `Executor` – a simple interface for executing tasks, which we will cover in this tutorial.
- `ExecutorService` – this is a much more complex interface which manages the executor itself.
- `ScheduledExecutorService` – This interface extends ExecutorService with methods for scheduling the execution of a task.


# Executors

As we saw in the <a href="https://abhinandandubey.github.io/posts/2020/10/10/Processes,-Threads-and-Concurrency.html" target="_blank">previous post</a>, it's tedious and dangerous to create and manage threads manually. The Concurrency API defined in `java.util.concurrent` package, introudces `ExecutorService` for this very purpose.


```java
Runnable greetingTask = () -> {
    System.out.println("Hello from " + Thread.currentThread().getName());
};
ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(greetingTask);
```

If you run the code above, you'd see something like this, but you'd also observe this program never exits!

```bash
Hello from pool-1-thread-1
```

The `ExecutorService` defines a few methods for graceful exits

- `shutdown()` waits for currently running tasks to finish, and stops ExecutorService from accepting any new tasks. It DOES NOT BLOCK the thread it was called from, 
- `shutdownNow()` interrupts all running tasks and shuts the executor down immediately, returning all tasks which were awaiting execution.
- `awaitTermination()` doesn't actually shut down your executor. It DOES BLOCK the calling thread. (just like the `join()` method)
- `invokeAll()` waits for all tasks to complete. 

#### If `shutdown()` already waits for tasks to complete, why do we need `awaitTermination()`

Suppose you can wait only for 5 minutes after calling `shutdown()` , post 5 minutes, you want to forcibly kill the ExecutorService, by issuing a `shutdownNow()`. `awaitTermination()` blocks until all tasks have completed execution after you call a `shutdown()`.

`awaitTermination()` doesn't actually shut down your executor. So unless another thread shuts down the executor, `awaitTermination()` will just sit there until the timeout runs out.

<blockquote class="palenote">
To summarize, I see awaitTermination() as a blocking mechanism, and shutdown() as a way to signal the executor service that a closure is imminent. 
</blockquote>

Recommended way to use `shutdown()` and `awaitTermination()`:

```java
try {
    System.out.println("Attempting to shutdown executor");
    executor.shutdown(); // signal a shutdown
    executor.awaitTermination(5, TimeUnit.SECONDS); // block for 5 seconds 
}
catch (InterruptedException e) {
    System.err.println("An interruption occured");
}
finally {
    if (!executor.isTerminated()) {
        System.err.println("Canceling tasks awaiting execution");
    }
    executor.shutdownNow(); // force shutdown
    System.out.println("shutdown successful");
}
```


# Callables

The API also provides another type of task, which allows you to return values from the task. 

```java
Callable<Integer> task = () -> {
    try {
        TimeUnit.SECONDS.sleep(1);
        return Math.sqrt(172);
    }
    catch (InterruptedException e) {
        throw new IllegalStateException("task interrupted", e);
    }
};
```

The syntax looks pretty similar to Runnable, but how about getting the return value? Enter `Future`.

## Future

The `submit()` method doesn't wait until the task finishes execution. To remedy that, Executor returns a special type called Future which can be used to retrieve the actual result at a later point in time.

```java

Callable<Double> getSquareRoot = () -> {
    try {
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Hello from " + Thread.currentThread().getName());
        return Math.sqrt(172);
    } catch (InterruptedException e) {
        throw new IllegalStateException("task interrupted", e);
    }
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Double> future = executor.submit(getSquareRoot);
try {
    System.out.println(future.get());
} catch(InterruptedException e) {
    e.printStackTrace();
}
```

One thing to observe here is that `get()` blocks the thread. It will wait until it can fetch the result from the future.

### Example 

Lets create a program which computes a square root, and square on different threads at the same time.

We  the `getSquare()` method

```java
Callable<Double> getSquare = () -> {
    try {
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Hello from " + Thread.currentThread().getName());
        return Math.pow(172, 2);
    } catch (InterruptedException e) {
        throw new IllegalStateException("task interrupted", e);
    }
};
```

And we now use `newFixedThreadPool` to initialize a thread pool of size 2. I encourage you to go through or at least have a look at the <a href="https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html" target="_blank">variety of thread pools available in Java</a>



Generally, a Java thread pool is composed of:

- the pool of worker threads, responsible for managing the threads
- a thread factory that is responsible for creating new threads
- a queue of tasks waiting to be executed


```java
ExecutorService executor = Executors.newFixedThreadPool(2);
Future<Double> squareRoot = executor.submit(getSquareRoot);
Future<Double> square = executor.submit(getSquare);

try {
    System.out.println(squareRoot.get()*square.get());
} catch(InterruptedException e) {
    e.printStackTrace();
}
```

If you run the code multiple times, you should expect to see something like this:

```bash
Finished in 0 ms
Hello from pool-1-thread-2
Hello from pool-1-thread-1
387990.52260590077

Finished in 0 ms
Hello from pool-1-thread-1
Hello from pool-1-thread-2
387990.52260590077

Finished in 0 ms
Hello from pool-1-thread-1
Hello from pool-1-thread-2
387990.52260590077
```

### Timeouts

We almost never want to block our application code. Since `get()` is a blocking operation, we definitely want to have some sort of control so as not to render our application unresponsive.

Turns out, this is rather simple using 

```java
future.get(5, TimeUnit.SECONDS); 
```

### Invoking Multiple Callables

Executors also have an added support for supplying multiple callables and invoke them via `invokeAll()`. You supply a collection of callables and it will return you a list of futures.

```java
ExecutorService executor = Executors.newWorkStealingPool();

List<Callable<String>> callables = Arrays.asList(
        () -> "task1",
        () -> "task2",
        () -> "task3");

executor.invokeAll(callables)
    .stream()
    .map(future -> {
        try {
            return future.get();
        }
        catch (Exception e) {
            throw new IllegalStateException(e);
        }
    })
    .forEach(System.out.println);
```

I didn't want to stuff this post with too much detail on `invokeAll`, but please feel free to go through the documentation. 

# ScheduledExecutorService

This interface provides you method which allow you to schedule tasks at your leisure. Want to schedule something 3 minutes from now? Want something to run every 2 minutes? You got it.

```java
ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
Runnable task = () -> {
    System.out.println("Hello from " + Thread.currentThread().getName());
}
ScheduledFuture<?> future = executor.schedule(task, 3, TimeUnit.SECONDS);
```

You can also schedule tasks to be executed periodically, using `scheduleAtFixedRate()` and `scheduleWithFixedDelay()`.

```java
executor.scheduleAtFixedRate(task, initialDelay, period, TimeUnit.SECONDS);
```

#### What's the difference?
`scheduleAtFixedRate()` doesn't consider the actual duration of the task. So if you specify a period of 5 minutes but the task needs 10 minutes to finish execution, then expect your thread pool to get overloaded pretty soon.

If you don't know how long your task is going to take, you'd rather use `scheduleWithFixedDelay()`. Using scheduleWithFixedDelay, the wait time period applies **between** the two tasks. 



<blockquote class="yellownote">
This blog post is part of the <a href="https://abhinandandubey.github.io/posts/tags/Advanced-Java-Series">Advanced Java Series</a>. Continue <a href="https://abhinandandubey.github.io/posts/2019/10/10/The-Concurrency-API.html" target="_blank">here with the Concurrency API.</a> 
</blockquote>

Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>

</center>
<br/>
Feeling even more generous ? 

<a href="https://foodforeducation.org/" target="_blank"><b>$20 can feed a poor child for a whole year</b></a>. Akshaya Patra *(Aak-sh-ayah pa-tra)* is the world’s largest NGO school meal program, providing hot, nutritious school lunches to over 1.8 million children in 19,257 schools, across India every day. Your 20$ makes all the difference.

<a href="https://foodforeducation.org/" target="_blank"><img src="https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/2020-10-10-16-55-08.png"/></a>

<br/>
<br/>
