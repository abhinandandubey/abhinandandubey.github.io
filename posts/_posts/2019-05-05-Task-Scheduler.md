---
layout: post
title: Task Scheduler
tags: Programming, Interview 
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_0338.jpg
cover_meta: The North Reading Room at Stony Brook University (c) AD Photography
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

This problem was of my particular attention for three reasons - it doesn't have a very obvious solution, it's one of the popular problems tossed around in coding interviews, and none of the posts on Leetcode do justice to the wonderful solution this problem has. It's like people have derived their own random meanings from reading someone else's code. And don't be mistaken - I've done that too, but at least I managed to put it in a way it was probably meant to be presented.

## The Problem 

The problem gives us a Character array representing a certain number of CPU Tasks, labeled by alphabets, Tasks could be taken up in any order. In any given interval, CPU can finish a task or just stay `IDLE`.

The constraint is that **there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.**

The problem with most of the solutions is that they forget to focus on this. They start with some arbitrary shit which seems like some kind of witchcraft to me at least.

## Intuition

The first thing you should be worried about is that no two same tasks can be within $n$ distance of each other. So what do we have to ensure? **In cycles of $n$, we can have at most $n$ different kind of tasks, but if we run out of unique tasks, that means we just have to keep the processor `IDLE`.** To me at least, this is the most important statement a solution has to make in order for it to make sense, which unfortunately no other solution does.

The second thing is the ordering of these tasks. Naturally, if I have lesser unique tasks, I'll be worried about processor staying `IDLE`, which is bad. In other words **I have to be concerned about tasks with higher frequencies**. This makes it a perfect candidate for a Priority Queue, or a Max-Heap.

## Solution

Lets count the frequencies. Also, lets put them in a queue, The tuple `(-1*tasksMap[task], task)` representing the priority aka the frequency (multiplied by $-1$, thanks to Python not having a Max Heap implementation) and the task Id (character representing a task) itself.

```python
tasksMap = collections.Counter(tasks)
q = []
for task in tasksMap:
    heapq.heappush(q, (-1*tasksMap[task], task))
```

The outer loop below is on the q, *and then again on the q?* - That's another particularly bizarre thing about this solution. To understand this, read the first bold point in our intuition. What we're really looking at is **number of unique tasks which is `len(q)` and the cycle of $k$ itself**. We either run out of cycles, or unique tasks. If we run out of cycles (or $k = n+1$) first, that means we exhausted all of cycles, and no intervals were `IDLE`. But if we run out of $len(q)$ first, this means we still have some $k$ left, which is the count of number of intervals processor has to be `IDLE`.


```python
count = 0
while q:
    k = n+1
    temp = []
    while(k > 0 and len(q)>0):
        # len(q) actually represents the number of unique tasks 
        # so we are checking either we run out of k, or num of unique tasks
        task = heapq.heappop(q)
        #print("EXEC " + task[1])
        temp.append((-1*task[0]-1, task[1]))
        k-=1
        count +=1
    
    #print("temp = {}".format(temp))
    for task in temp:
        if task[0] > 0:
            heapq.heappush(q, (-1*task[0], task[1]))
    
    
    #print(k, len(q))
    if len(q) == 0: break
    count += k; # if k > 0, then it means we need to be idle, so just add it to the count
    #print("IDLE"*k)
    #print("-----------")
return count
```
        

Thanks for reading through ! Here's an interesting function - can you solve the <a href="https://abhinandandubey.github.io/posts/2020/10/07/A-Confusing-Confused-Function.html" target="_blank">riddle of this confusing function</a> ?

Feeling generous ? Help me write more blogs like this :) 

<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>