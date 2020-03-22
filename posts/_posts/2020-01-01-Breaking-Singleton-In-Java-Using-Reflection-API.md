---
layout: post
title: Breaking Singleton In Java Using Reflection API
tags: Coding Interview Programming Career Java DesignPatternSeries
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_3949.jpg
cover_meta: Traveler (c) AD Photography
color_scheme: tango
mathjax: true
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

## Singleton Design Pattern

The Singleton Design Pattern in Java is probably one of the most overused Design Patterns. I love to say that it actually is the mother pattern of all Design Patterns in the way that it might have actually lead to the "invention" or "realization" of something called _Design Patterns_.

The Singleton pattern basically makes sure that throughout the lifecycle of your application, you're having at most one instance of a class initialized. 

Here's a quick example;

```java
package creational;

public class DbSingleton {

    private static DbSingleton instance = new DbSingleton();

    private DbSingleton() {}

    public static DbSingleton getInstance() {
        return instance;
    }
}
```

In the code above, you basically have three parts:

1. An object of the class.
2. An empty constructor marked private to prevent overrides.
3. A `getInstance` method which returns the same global instance everytime.

A driver class to quickly test the setup;

```java
public class DbSingletonDemo {

    public static void main(String args[]){

        DbSingleton instance1 = DbSingleton.getInstance();

        System.out.println(instance1);

        DbSingleton instance2 = DbSingleton.getInstance();

        System.out.println(instance2);

    }
}
```

And it's clear to us that we're always getting the same object in the memory everytime we call the function.

```bash
creational.DbSingleton@60e53b93
creational.DbSingleton@60e53b93

Process finished with exit code 0
```

A little problem here is that you're creating an object `instance` at class instantiation. A large enterprise-grade application might have several such objects which can lead to a slow start-up.

A work-around is to have lazy-loading.

```java
public class DbSingleton {

    private static DbSingleton instance = null;

    private DbSingleton() {}

    public static DbSingleton getInstance() {
        if(instance == null) {
            instance = new DbSingleton();
        }
        return instance;
    }
}
```


### Breaking The Singleton

Life's good and we have a _perfect_ Singleton class. Or so we thought. Enter Reflection. 

Reflection is a feature in Java which allows the programmer to get access to objects and methods of a Java class. A quick modification to our driver program shows how easy it is to break the Singleton:

```java
        DbSingleton instance1 = DbSingleton.getInstance();

        System.out.println(instance1);

        DbSingleton instance2 = null;
        try {
            Constructor constructor = DbSingleton.class.getDeclaredConstructor();
            constructor.setAccessible(true);
            instance2 = (DbSingleton) constructor.newInstance();
        } catch (Exception ex) {
            System.out.println(ex);
        }

        System.out.println(instance2);
```

And you have two different objects:

```bash
creational.DbSingleton@60e53b93
creational.DbSingleton@5e2de80c
```

### Protection from Reflection

A simple way to protect from Reflection is to define your constructor in such a way that every time its called, it checks if there's already an instance initialised, and if so, throws a `RunTimeException`

```java
    private DbSingleton() {
        if(instance != null){
            throw new RuntimeException("Use getInstance() method instead !");
        }
    }
```

### Thread Safety

The modifications we made might have protected us from someone deliberately trying to break the Singleton, but if your application were to use Threads, you could easily run into issues

To avoid that, we need to add the `volatile` keyword to ensure the JVM never caches the value of the instance within the thread ("thread-local caching"), and that any reads and write will go to the main memory.


```java
    private static volatile DbSingleton instance = null;
```

The next order of business is to introduce a double check locking mechanism, basically trying to use `synchronised` to make sure thread safety. How?

> All synchronized blocks synchronized on the same object can only have one thread executing inside them at the same time. All other threads attempting to enter the synchronized block are blocked until the thread inside the synchronized block exits the block.

Now instead of putting the whole function in a synchronised (which is a bad practice), we could do it this way:

#### Double Check Locking Mechanism 

```java
    public static DbSingleton getInstance() {
        if(instance == null) {
            synchronized (DbSingleton.class) {
                if(instance == null){
                    instance = new DbSingleton();
                }
            }

        }
        return instance;
    }
```

