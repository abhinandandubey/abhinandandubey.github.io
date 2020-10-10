---
layout: post
title: Processes, Threads and Concurrency
tags: 
cover_url: https://source.unsplash.com/random?horses
cover_meta: 
  (c) UNSPLASH
color_scheme: tango
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

Lets run this task which we created on a thread.


```java
Runnable greetingTask = () -> {
    System.out.println("Hello from " + Thread.currentThread().getName());
};

Thread thread = new Thread(greetingTask);
thread.start();
```


```bash
Hello from thread-0
```

## So that's all we need for Concurrency, right?

Well consider doing a real task. See **that's why I hate Dog extends Animal examples**. You are trying to make a pizza, and you assign different threads different tasks.

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

![](https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/assets/images/2020-10-10-13-08-00.png)

If you run this code over and over, you'll see really weird results. Since there's no way of knowing which statement would be executed first, it was a pain to build programs which used concurrency. Java introduced
Concurrency API with Java 5 back in 2004. Here's an interesting fact - this was about the same time Mark Zuckerberg launched Facebook from his Harvard Dorm Room.

![](https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/assets/images/2020-10-10-13-15-02.png)

Continue (here)[] with the Concurrency API.

Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>
<br/>
