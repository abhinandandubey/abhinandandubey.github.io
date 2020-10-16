---
layout: post
title: Java - Common Points of Confusion
tags: Java Programming Advanced-Java-Series
cover_url: https://images.unsplash.com/photo-1515859005217-8a1f08870f59?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1398&q=80
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

I thought of making this post when I found myself confused by some very common methods we use in Java. 

### `length` vs `length()` vs `size()`

- **`size()`** is a method defined in `java.util.Collection`, so it is available for any collection in Java, and returns to the number of elements in the collection. You cannot use this with strings.
- **`length`** is a field on an array object. If you initialize the array with `int[] a = new int[4]`, `a.length` always returns 4 (the *capacity* of the array)
- **`length()`** is exclusively for strings. String, StringBuilder etc - to know the length of the String.

To summarize, use `length` only on arrays (`int[]`, `double[]`, `String[]`) to know the length of the arrays. 

`String` is not a vanilla array, so you can't use `length` and it's not even a Collection (so you can't use `size()`. This is the only case where you'd use `length()`.

### `int` vs `Integer`

A lot of content online focuses on what these are. (`int` is a primitive type. `Integer` is a class) - but I think that's sort of not very helpful when it comes to application. The way I understood is `Integer` is a wrapper around `int` which makes your life much easier with predefined methods. You might also find people using some jargon on the lines of *boxing* and *autoboxing*. It's just a fancy way of saying:

```java
// You can do this and java won't 
// shout back at ya
Integer n = 8;
// It would internally do
//  something like this
Integer n = new Integer(8);
// or this
Integer n = Integer.valueOf(8);
```

#### Verdict
Use `int`. However, if the value can be `null` or is to be used as an Object or Collections -  use `Integer`. Using `int` also gives you performance benefits that come with primitive types.



### `HashMap` vs `Hashtable`

`Hashtable` is usually considered legacy ("old") code. There's virtually nothing about Hashtable that can't be done using HashMap or derivations of HashMap. However, there are some differences:

`Hashtable` is synchronized, whereas `HashMap` is not. This makes HashMap better for non-threaded applications, as unsynchronized Objects typically perform better than synchronized ones. However, you're still better off using `Collections.synchronizedMap()` or `ConcurrentHashMap` - read more in the StackOverflow answer below

This is taken from <a href="https://stackoverflow.com/a/40878/5102599" target="_blank">this answer by Josh Brown here</a>


### `public` vs unspecified vs `protected`

<a href="https://raw.githubusercontent.com/abhinandandubey/abhinandandubey.github.io/master/assets/images/2020-10-11-12-00-00.png" target="_blank"><img src="https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/2020-10-11-12-00-00.png"/></a>

I made an effort to show access modifiers with some actual code. Click on the image above to open it in a new tab.

### `StringBuilder` vs `StringBuffer`

`StringBuffer` is synchronized, `StringBuilder` is not.

### `implements Runnable` vs `extends Thread`

Use `implements Runnable`. Period.

### Pass-By-Value / Pass-By-Reference

There are a ton of upvoted answers on this question on StackOverflow. And they are heavily upvoted. However, from a beginner standpoint, I find them **highly misleading**.

Give yourself 2 minutes of your time to understand the code below, and I assure you you'll understand how Java really handles variable passing.

```java
public class Main {
    
    public static void method1(List<List<Integer>> a, List<Integer> b, int c){
        a.add(Arrays.asList(1,2,3));
        b.add(7);
        c = 5;
        return;
    }
    
    public static void main(String[] args) {
        List<List<Integer>> x = new ArrayList<>();
        List<Integer> y = new ArrayList<>();
        y.add(4);
        y.add(5);
        y.add(6);
        int z = 1;
        method1(x, y, z);
        System.out.println(x + " " + y + " " + z); // prints [[1, 2, 3]] [4, 5, 6, 7] 1
    }
}
```

In short, collections/objects seem to be pass by reference, primitive types are pass by value.

### `add()` vs `offer()` in PriorityQueue

The two functions come from two different interfaces that PriorityQueue implements:

`add()` comes from Collection.
`offer()` comes from Queue.

- For PriorityQueue, the two functions are synonymous. 
- For a capacity-constrained queue, the difference is that add() always returns true and throws an exception if it can't add the element, whereas `offer() `is allowed to return false if it can't add the element.

### `LinkedList` vs `Stack`

Stack class is mostly a leftover that has become more or less redundant with the newer Java Collections Framework.


<blockquote class="yellownote">
This blog post is part of the <a href="https://abhinandandubey.github.io/posts/tags/Advanced-Java-Series">Advanced Java Series</a>. Continue <a href="https://abhinandandubey.github.io/posts/2020/10/09/Java-Memory-Model.html" target="_blank">here with the Java Memory Model</a> 
</blockquote>

<br/>
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
