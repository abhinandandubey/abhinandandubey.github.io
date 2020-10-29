---
layout: post
title: Understanding Synchronize In Java
tags: Java Programming Advanced-Java-Series
cover_url: https://source.unsplash.com/random?threads
cover_meta: 
  (c) UNSPLASH
color_scheme: tango
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

# The Problem

A lot of us misunderstand `synchronized` in java. The way this can be used (as a _block_ or as keyword to tag a function) - also makes it confusing.

## A Dumb Unbounded Producer

Lets say you were to create a simple producer, no bounds - just a simple producer which keeps on writing integers onto a shared queue.

```java
class Producer {

    Queue<Integer> sharedMessages;
    Producer(Queue<Integer> sharedMessages) {
        this.sharedMessages = sharedMessages;
    }

}
```

As shown above, we initialize the constructor with a `sharedMessages` object. We purposedly use a vanilla `Queue` (no fancy concurrent data-structure)

We'd need this as a runnable, so lets use the `Runnable` interface, and define our `run()` method


```java
class Producer implements Runnable {

    Queue<Integer> sharedMessages;
    Producer(Queue<Integer> sharedMessages) {
        this.sharedMessages = sharedMessages;
    }

    public void run(){
        int i = 0;
        while(true){
            produce(i++);
        }
    }
```

We'd probably also need a `produce()` method which adds these messages to the `sharedMessages` :

```java
    public void produce(Integer i) {
        this.sharedMessages.add(i);
    }
```

## A Dumb Unbounded Consumer

With a similar idea, we create an unbounded consumer:

```java
class Consumer implements Runnable{

    Queue<Integer> sharedMessages;
    Consumer(Queue<Integer> sharedMessages) {
        this.sharedMessages = sharedMessages;
    }

    public void consume() {
        System.out.println(sharedMessages.remove());
    }

    public void run(){
        while(sharedMessages.size() > 0){
            consume();
        }
    }
}
```

Next, lets create a shared object for the messages, a single producer thread and multiple consumer threads.


```java
// Initialize the shared object
Queue<Integer> sharedMessages = new LinkedList<>();

// Initialize a producer
new Thread(new Producer(sharedMessages)).start();

// Initialize many consumers running parallely
new Thread(new Consumer(sharedMessages)).start();
new Thread(new Consumer(sharedMessages)).start();
new Thread(new Consumer(sharedMessages)).start();
new Thread(new Consumer(sharedMessages)).start();
new Thread(new Consumer(sharedMessages)).start();
new Thread(new Consumer(sharedMessages)).start();
```

You'd definitely see some errors thrown _(If you don't see any errors, wait - or even better - start more consumer threads)_ :

```bash
47110
47111
47112
47086
null
47083
Exception in thread "Thread-4" Exception in thread "Thread-3" Exception in thread "Thread-2" Exception in thread "Thread-1" java.util.NoSuchElementException
	at java.util.LinkedList.removeFirst(LinkedList.java:270)
```

## A Smarter Producer-Consumer

### Smarter Consumer

The first instinct upon seeing the error message is to synchronize the `consume()` method.

```java
public synchronized void consume() {
    System.out.println(sharedMessages.remove());
}
```

That should do it, right? After all, adding `synchronize` means only one instance can mess around with the method. However, you'd still see the same error happening.

The reason being the `sharedMessages.size() > 0` within the `run()` method - what's happening is, Thread A calls `consume()` after seeing a size of 1, and at the same time Thread B sees a size of 1, and while Thread A was doing a remove, Thread B was a nanosecond late, and called a remove() too. Too sad, there were no elements, hence the error.

The next natural thing to do is to try adding a `synchronize` on the `run()` method. But that sounds _weird_ - well it's not only unconventional, it's actually self-contradictory. You want concurrency, you want multiple threads calling their `run()` method *concurrently* - and marking run() `synchronized` would prevent it from doing it. Instead, you'd adopt a more conventional approach, and do something like this:

```java
public void run(){
    synchronized (this){
        while(sharedMessages.size() > 0){
            consume();
        }
    }
}
```

To our dismay, we still see the dreaded error. The reason is yet another intricacy around the use of `synchronized` - 

I think a lot of tutorials on concurrency miss this point largely. There are two problems here.

1. **Un-synchronized method called from a synchronised block** : We're trying to call `consume()` from a synchronized block. Our `consume()` isn't synchronized. It's not either/or. If you're synchronising, you have to explcitly synchronize any methods you're calling from within the synchronised block.

2. **What are you synchronizing on?** : The problem here is that we're trying to synchronize on `this` - the instance of the class. When using `synchronized`, you need an object as a *monitor lock*. Only threads using the same monitor lock will be synchronized. 
   
<blockquote class="yellownote">
It is important that same object is shared across different threads in order for them to synchronize correctly.
</blockquote>

Thus, we want to ensure that our **synchronize is over the shared resource**. If you're passing `this` - that means you want to ensure that only one thread has access to this class instance at a time. But that also means that same class object should be passed to those threads.

Recall here that we are passing separate consumer objects, effectivelly rendering the `synchronize(this)` block useless.

```java
new Thread(new Consumer(sharedMessages)).start();
new Thread(new Consumer(sharedMessages)).start();
. . .
```

To sum our discussion, we need to synchronize here on `sharedMessages` object.
```java
synchronized (sharedMessages){
    while(sharedMessages.size() > 0){
        consume();
    }
}
```

You shouldn't see the dreaded error anymore! We finally have a smarter producer-consumer:

```java
class Consumer implements Runnable{

    Queue<Integer> sharedMessages;
    Consumer(Queue<Integer> sharedMessages) {
        this.sharedMessages = sharedMessages;
    }

    public void consume() {
        synchronized (sharedMessages){
            System.out.println(sharedMessages.remove());
        }
    }

    @Override
    public void run(){
        synchronized (sharedMessages){
            while(sharedMessages.size() > 0){
                consume();
            }
        }
    }
}
```


We follow a smilar strategy for Producer:

```java
class Producer implements Runnable {

    Queue<Integer> sharedMessages;
    Integer i = 0;
    Producer(Queue<Integer> sharedMessages) {
        this.sharedMessages = sharedMessages;
    }

    public void produce(Integer i) {
        synchronized (sharedMessages){
            this.sharedMessages.add(i);
        }
    }

    public void run(){
        synchronized (sharedMessages) {
            while (i < 10000) {
                produce(i++);
            }
        }
    }
}
```

### Testing the Smarter Consumer

Lets also check whether our consumer is actually using multiple threads. We can do this by making a small change - 

```java
public void run(){
    synchronized (sharedMessages){
        while(sharedMessages.size() > 0){
            consume();
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                System.out.println(Thread.currentThread().getName() + ": I was interrupted from my sleep!");
            }
        }
    }
}
```

Running this code would result in something like this:

```bash
0 consumed by Thread-1
1 consumed by Thread-1
2 consumed by Thread-1
3 consumed by Thread-1
...
9996 consumed by Thread-1
9997 consumed by Thread-1
9998 consumed by Thread-1
9999 consumed by Thread-1
```

Even with a thread.sleep, one thread acquires the lock, and doesn't release it. While the current thread is sleeping, it'd make sense for other threads to be able to consume messages, right?


### Seeing is Believing

It's important here to note here asking thread to sleep, you don't affect the change in the monitor on which the lock is held on - `sharedMessages`. A small change and the use of `wait()` makes a huge difference:

```java
@Override
public void run(){
    synchronized (sharedMessages){
        while(sharedMessages.size() > 0){
            consume();
            try {
                sharedMessages.wait(5);
            } catch (InterruptedException e) {
                System.out.println(Thread.currentThread().getName() + ": I was interrupted from my sleep!");
            }
            sharedMessages.notifyAll();
        }
    }
}
```


```bash
0 consumed by Thread-1
1 consumed by Thread-2
2 consumed by Thread-3
3 consumed by Thread-4
4 consumed by Thread-1
5 consumed by Thread-2
```

I highly suggest you to take a look at the <a href="https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html" target="_blank">Intrinsic Locks and Synchronization</a>

<br/>
Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>
<br/>

