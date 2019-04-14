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
        inlineMath: [['$','$'], ['\(','\)']],
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

If you're designing an actual Cache which uses multi-threading, it might be better to use a `HashTable` because it's synchronised. It also does not allow null keys or values. However, in high-grade production systems, you wouldn't just be able to get by thread safety just by using a HashTable. Read (this)[https://stackoverflow.com/a/41454], or use Scala. :)