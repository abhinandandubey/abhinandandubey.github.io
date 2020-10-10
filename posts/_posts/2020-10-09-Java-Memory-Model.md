---
layout: post
title: Java Memory Model
tags: Java Series, Java, Programming
cover_url: https://images.unsplash.com/photo-1580711508185-2ddd1d9d2e1b
cover_meta: 
  (c) UNSPLASH
color_scheme: zenburn
mathjax: true
mathjax: True
---
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script src="https://cdn.jsdelivr.net/npm/@duckdoc/termynal@1.0.0/termynal.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@duckdoc/termynal@1.0.0/termynal.min.css">
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

# Why study the memory model?

It's important to understand the memory model, this is specially true for Java because Java handles its variables in a much different way than non-JVM languages. It even has special keywords like `volatile` which wouldn't make sense without having a proper knowledge of how these variables take shape inside the memory.

# The Java Memory Model

<img src="https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/Java_Memory_Model.svg"/>

The above diagram shows how it looks like. As you go down from Registers to RAM, size increases, latency increases, and things get further from the processor.

## Field Visibility 

Whenever we talk about “visibility” - it's in reference to visibility to among threads.

### Concurrency 

The visibility of writing operations on shared variables can cause problems during delays that occur when writing to the main memory due to caching in each core. This can result in another thread reading a stale value (not the last updated value) of the variable.

**Field Visibility Problem** is solved using `volatile`. In below example, if writer and reader functions are getting called simultaneously in different threads, `x` might be `1` in local cache of core `1` but might not have updated value in shared cache. Hence when reader loads value of `x`, it might load it as `0`.

```java
public class FieldVisibility {
    int x = 0;

    public void writerThread() {
        x = 1;
    }

    public void readerThread() {
        System.out.println(x);
    }
}
```

### Happens Before Relationship

Java solves this problem using _Happens Before_ Relationship. Whatever happened before “write of x” should be visible after “read of x”. Hence, all the changes which are visible to thread `T1` before a `volatile` write or a `synchronized` unlock will be visible to thread `T2` after a `volatile` read of the same variable or locking on the same monitor.

Consider the program below:

```java
public class Unstoppable {

    private static boolean killSwitch;

    public static void main(String[] args) 
        throws InterruptedException {

            Thread backgroundThread = new Thread(new Runnable() {
                public void run() {
                    int i = 0;
                    while (!killSwitch)
                        i+=1
                        System.out.println("Value of i is" + i.toString());
                }
            });

            backgroundThread.start();
            TimeUnit.SECONDS.sleep(1);
            killSwitch = true;

    }
}
```


It never stops because the updated value of `stopRequested` never becomes visible to `backgroundThread`

<div id="termynal" data-termynal>
    <span data-ty="input">java -jar Unstoppable.jar com.alivcor.Unstoppable</span>
    <span data-ty>Value of i is 1</span>
    <span data-ty>Value of i is 2</span>
    <span data-ty>Value of i is 3</span>
    <span data-ty>Value of i is 4</span>
    <span data-ty>Value of i is 5</span>
    <span data-ty>Value of i is 6</span>
    <span data-ty>Value of i is 7</span>
    <span data-ty>Value of i is 8</span>
    <span data-ty>......87212 more lines</span>
</div>


## Java - Order of Execution

1. Static initialization blocks will run whenever the class is loaded first time in JVM
2. Initialization blocks run in the same order in which they appear in the program.
3. Instance Initialization blocks are executed whenever the class is initialized and before constructors are invoked. They are typically placed above the constructors within braces.

```java
class FileReader { 
  
    FileReader(String filePath) 
    { 
        System.out.println("Reading file with file path"); 
    } 
  
    FileReader() 
    { 
        System.out.println("Read file with no path"); 
    } 
  
    static
    { 
        System.out.println("This gets called 1st"); 
    } 
  
    { 
        System.out.println("This gets called 3rd"); 
    } 
  
    { 
        System.out.println("This gets called 4th"); 
    } 
  
    static
    { 
        System.out.println("This gets called 2nd"); 
    } 
  
    public static void main(String[] args) 
    { 
        new FileReader(); 
        new FileReader(8); 
    } 
} 
```



Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
