---
layout: post
title: The Concurrency API
tags: Advanced-Java-Series Java Programming
cover_url: https://source.unsplash.com/random?india
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

# Executors

As we saw in the <a href="https://abhinandandubey.github.io/posts/2020/10/10/Processes,-Threads-and-Concurrency.html" target="_blank">previous post</a>, it's tedious and dangerous to create and manage threads manually. The Concurrency API defined in `java.util.concurrent` package, introudces `ExecutorService` for this very purpose.


```java
Runnable greetingTask = () -> {
    System.out.println("Hello from " + Thread.currentThread().getName());
};
ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(greetingTask);
```

If you run the code above, you'd see something like this, but you'd see this program never exits!

```bash
Hello from pool-1-thread-1
```

The `ExecutorService` defines two methods for graceful exits

- `shutdown()` waits for currently running tasks to finish, and stops `ExecutorService` from accepting any new tasks. It DOES NOT BLOCK the thread it was called from, 
- `shutdownNow()` interrupts all running tasks and shuts the executor down immediately, returning all tasks which were awaiting execution.
- `awaitTermination()` doesn't actually shut down your executor. It DOES BLOCK the calling thread. (just like the `join()` method)
- `invokeAll()` waits for all tasks to complete. 

#### If `shutdown()` already waits for tasks to complete, why do we need `awaitTermination()`

Suppose you can wait only for 5 minutes after calling `shutdown()` , post 5 minutes, you want to forcibly kill the ExecutorService, by issuing a `shutdownNow()`. `awaitTermination()` blocks until all tasks have completed execution after you call a `shutdown()`.

`awaitTermination()` doesn't actually shut down your executor first. So unless another thread shuts down the executor, `awaitTermination()` will just sit there until the timeout runs out.

<blockquote>
To summarize, I see `awaitTermination()` as a blocking mechanism, and `shutdown()` as a way to signal the executor service that a closure is imminent. 
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


# 




<blockquote class="yellownote">
This blog post is part of the <a href="https://abhinandandubey.github.io/posts/tags/Advanced-Java-Series">Advanced Java Series</a>. Continue <a href="https://abhinandandubey.github.io/posts/2020/10/10/The-Concurrency-API.html" target="_blank">here with the Concurrency API.</a> 
</blockquote>

Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>
<br/>
