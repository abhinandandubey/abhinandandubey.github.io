---
layout: post
title: LRU Cache in Java
tags: 
cover_url: https://github.com/alivcor/lightforest/raw/master/A629F21F-176B-4247-A48E-87705E4DBC3F.jpg
cover_meta: Okanogan-Wenatchee National Forest from above (c) AD Photography
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

# LRU Cache

It's probably one of the most _classic_ problems that have been holding the forts of Coding Interviews. This makes it a perfect candidate for a detailed blog post!

## The Problem

Design and implement an [LRU Cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU) which supports the following operations:

1. `get(key)` - Get the value of a key (if the key exists in the cache). If it doesn't then return -1.
2. `put(key, value)` - Insert a new key with a given value or update the value if the key is already present. If the cache reaches its capacity, it should evict the least recently used item before inserting a new item.

## The Constraints

Both of your operations should be $O(1)$.


## Intuition

$O(1)$ almost always implies a hashmap. Since order here is important, one can immediately think of something on the lines of `OrderedDict` in python, or the Java `LinkedHashMap`. The `LinkedHashMap` stores the ordering implicitly and provides `removeEldestEntry()` method which makes the implementation _very_ simple.

A more subtle approach is using a Doubly Linked List glued to a HashMap.

<img src="data:image/png;base64,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" style="cursor:pointer;max-width:100%;" onclick="(function(img){if(img.wnd!=null&&!img.wnd.closed){img.wnd.focus();}else{var r=function(evt){if(evt.data=='ready'&&evt.source==img.wnd){img.wnd.postMessage(decodeURIComponent(img.getAttribute('src')),'*');window.removeEventListener('message',r);}};window.addEventListener('message',r);img.wnd=window.open('https://www.draw.io/?client=1&lightbox=1&edit=_blank');}})(this);"/>

## Implementation

### Define a Doubly Linked Node

This is simple

```java
public class DLinkedNode {
    int key;
    int value;
    DLinkedNode prev;
    DLinkedNode next;

    public DLinkedNode(int key, int value) {
        this.key = key;
        this.value = value;

    }
}
```


### Define a Doubly Linked List

A simple constructor for this could take up a `key` and a `value`. I prefer to keep `head` and `tail` markers to mark the begining and end of my list.

```java
public class DLinkedList {

    public DLinkedNode head, tail;

    public DLinkedList() {
        head = new DLinkedNode(-1, -1);
        tail = new DLinkedNode(-1, -1);

        head.next = tail;
        tail.prev = head;
    }
}
```

We do not need to implement all methods here. Since we want the ordering to reflect the LRU constraint, we should always add a new node after `head`, which is the first node of the list.

```java
    public void addFirst(DLinkedNode node) {
        node.next = head.next;
        node.prev = head;

        head.next.prev = node;
        head.next = node;
    }
```

Alike the `removeEldestEntry`, the last node in our list will always reflect the eldest accessed item.

```java
    public void popLast() {
        tail.prev.prev.next = tail;
        tail.prev = tail.prev.prev;
    }
```

And another method to pull out a node from the middle. 

```java
    public void removeNode(DLinkedNode node) {

        node.prev.next = node.next;
        node.next.prev = node.prev;

    }
```

Just a helper function to print out the list. And yet again I crib about [why Java doesn't have a `repr` function](https://stackoverflow.com/questions/1350397/java-equivalent-of-python-repr)

```java
    public void printCacheNodes() {
        DLinkedNode curr = head;
        while(curr!=null){
            System.out.print(curr.value + "->");
            curr = curr.next;
        }
        System.out.println();
    }
```


### The LRU Cache Class

The game doesn't end there. In fact, the most interesting part of the plot is yet to reveal itself. We start by implementing a simple constructor.

```java
import java.util.*;

public class LRUCache {

    private int capacity;
    private Map<Integer, DLinkedNode> cache;
    private DLinkedList dlist;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<Integer, DLinkedNode>();
        this.dlist = new DLinkedList();
    }

}
```

Now here comes the **most** tricky part. When to evict? If you go by the problem statement, it reads "it should evict the least recently used item **before** inserting a new item." But that doesn't mean you have to do it before insert a new item in your `put` method. 

> If the cache has reached its capacity, you should evict an item before inserting a new one

To make things more clear, if say you cache is full on Step $i$, you should evict it before continuing on to step $i+1$. 

```java
    public void put(int key, int value) {
        if(cache.containsKey(key)){
            DLinkedNode val_node = cache.remove(key);
            dlist.removeNode(val_node);
        }

        DLinkedNode node = new DLinkedNode(key, value);
        cache.put(key, node);
        dlist.addFirst(node);

        if(cache.size() > capacity){
            System.out.println("evicting");
            cache.remove(dlist.tail.prev.key);
            dlist.popLast();
        }

        this.printCacheContents();
    }
```


It is also important to note here that the `get` method also needs to use `removeNode` and `addFirst`. Since these methods are both $O(1)$, instead of moving the node to the first position, you'd rather remove and re-insert.

```java
    public int get(int key) {
        if(cache.containsKey(key)){
            DLinkedNode node_val = cache.get(key);
            dlist.removeNode(node_val);
            cache.remove(key);

            DLinkedNode node = new DLinkedNode(key, node_val.value);
            dlist.addFirst(node);
            cache.put(key, node);

            this.printCacheContents();
            return node_val.value;
        } else {
            System.out.println("Key not found");

            this.printCacheContents();
            return -1;
        }
    }
```

A simple helper function for debugging.

```java
    public void printCacheContents() {
        System.out.println("Size : " + cache.size() + " | capacity : " + this.capacity);

        System.out.println("Cache Nodes : ");
        dlist.printCacheNodes();

        System.out.println("HashTable : ");
        System.out.println(cache.toString());

    }
```


## The `HashMap` vs `HashTable` Debate

If you're designing an actual Cache which uses multi-threading, it might be better to use a `HashTable` because it's synchronised. It also does not allow null keys or values. However, in high-grade production systems, you wouldn't just be able to get by thread safety just by using a HashTable. Read [this](https://stackoverflow.com/a/41454), or use Scala. :)

