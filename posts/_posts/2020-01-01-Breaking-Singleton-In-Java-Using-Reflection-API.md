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


```bash
creational.DbSingleton@60e53b93
creational.DbSingleton@60e53b93

Process finished with exit code 0
```


