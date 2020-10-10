---
layout: post
title: Java Memory Model
tags: 
cover_url: https://images.unsplash.com/photo-1580711508185-2ddd1d9d2e1b
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

# Why study the memory model?

It's important to understand the memory model, this is specially true for Java because Java handles its variables in a much different way than non-JVM languages. It even has special keywords like `volatile` which wouldn't make sense without having a proper knowledge of how these variables take shape inside the memory.

# The Java Memory Model

<img src="https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/Java_Memory_Model.svg"/>

The above diagram shows how it looks like. As you go down from Registers to RAM, size increases, latency increases, and things get further from the processor.

## Order of Execution

 - Performance driven changes by Compiler, JVM or CPU-
## Field Visibility 

Whenever we talk about “visibility” - it's in reference to vsisbility to among threads.


### Concurrency 

The visibility of writing operations on shared variables can cause problems during delays that occur when writing to the main memory due to caching in each core. This can result in another thread reading a stale value (not the last updated value) of the variable.

**Field Visibility Problem** is solved using `volatile`. In below example, if writer and reader functions are getting called simultaneously in different threads, `x` might be `1` in local cache of core `1` but might not have updated value in shared cache. Hence when reader loads value of `x`, it might load it as `0`.

```Java
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


Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
