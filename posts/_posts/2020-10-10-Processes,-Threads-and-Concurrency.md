---
layout: post
title: Processes, Threads and Concurrency
tags: Advanced Java Series, Java, Programming
cover_url: https://images.unsplash.com/photo-1547581849-38ba650ad0de?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80
cover_meta: 
  (c) UNSPLASH
color_scheme: zenburn
mathjax: true
mathjax: True
---
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
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


# Processes & Threads

Most operating systems support concurrent executions and programming languages allow you to develop programs which allow you to utilise this awesome super-power. Except Python, which suffers from a disease called [Global Interpreter Lock](https://wiki.python.org/moin/GlobalInterpreterLock)

**Processes** are programs which typically run *independent* to each other. Within these processes, we have **Threads**, which usually work within the same leaps and bounds of a process accessing variables colocated, that is to say written in the same code.

Java supports threads since JDK 1. To define a *thread*, you also define what it has to do, often referred as a *task*. This can either be a task which returns something, called a **Runnable**, or something which does some processing, and just returns an exit code, a **Callable**.

## Define a Task

Here, we define a simple task which says hello.

```java
Runnable greetingTask = () -> {
    System.out.println("Hello from " + Thread.currentThread().getName());
};

greetingTask.run();
```

The output is going to look something like this

```bash
Hello from main
```

## Create a Thread

Lets create a separate thread to run this task on. Observe the thread creation syntax below:


```java
Runnable greetingTask = () -> {
    System.out.println("Hello from " + Thread.currentThread().getName());
};

Thread thread = new Thread(greetingTask);
thread.start();
```

And you should see something like:

```bash
Hello from thread-0
```



### So that's all we need for Concurrency, right?

Well consider doing a real task. (See *that's why I hate Dog extends Animal examples*). Either way, lets say you were trying to make a pizza, and you assign different threads different tasks - `makeDough`, `sprinkleCheese`, `bakePizza`, and finally you expect to `eatPizza`.

You rely on order of execution and expect things to be executed sequentially.

```java
Runnable makeDough = () -> {
    try {
        System.out.println("Making Dough");
        TimeUnit.SECONDS.sleep(4);
        System.out.println("Dough is Ready!");
    }
    catch (InterruptedException e) {
        e.printStackTrace();
    }
};

Runnable sprinkleCheese = () -> {
    try {
        System.out.println("Getting some Mozarella");
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Sprinkling some Mozarella");
    }
    catch (InterruptedException e) {
        e.printStackTrace();
    }
};

Runnable bakePizza = () -> {
    try {
        System.out.println("Baking Pizza");
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Pizza Baked");
    }
    catch (InterruptedException e) {
        e.printStackTrace();
    }
};

Runnable eatPizza = () -> {
    try {
        System.out.println("Eating Pizza");
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Yummy Yummy In My Tummy");
    }
    catch (InterruptedException e) {
        e.printStackTrace();
    }
};

Thread thread1 = new Thread(makeDough);
Thread thread2 = new Thread(sprinkleCheese);
Thread thread3 = new Thread(bakePizza);
Thread thread4 = new Thread(eatPizza);
thread1.start();
thread2.start();
thread3.start();
thread4.start();
```

What you'd expect:

```bash
Making Dough
Dough is Ready!
Getting some Mozarella
Sprinkling some Mozarella
Baking Pizza
Pizza Baked
Eating Pizza
Yummy Yummy In My Tummy
```

What you might end up seeing:

```bash
Making Dough
Getting some Mozarella
Baking Pizza
Eating Pizza
Sprinkling some Mozarella
Pizza Baked
Yummy Yummy In My Tummy
Dough is Ready!
```

If you run this code over and over, you'll see really weird results. Since there's no way of knowing which statement would be executed first, it was a pain to build programs which used concurrency. Java introduced
<a href="https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/package-summary.html" target="_blank">Concurrency API</a> with Java 5 back in 2004, about the same time when this lad launched Facebook from his Harvard Dorm Room.

![](https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/2020-10-10-13-15-02.png)

Continue <a href="https://abhinandandubey.github.io/posts/2020/10/10/Processes,-Threads-and-Concurrency.html" target="_blank">here with the Concurrency API.</a> 

Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>
<br/>
