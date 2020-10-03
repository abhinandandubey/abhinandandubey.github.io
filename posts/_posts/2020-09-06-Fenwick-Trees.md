---
layout: post
title: Fenwick Trees
tags: Interview, Programming
cover_url: https://source.unsplash.com/random?tree
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

I was recently trying to solve the <a href="https://en.wikipedia.org/wiki/Range_query_(data_structures)" target="_blank">Prefix Sum Porblem</a> and came across two wonderful ways to approach this problem optimally.

One of them is Fenwick Trees (or <i>Binary Indexed Trees</i> - if you support the author here on this article published here on <a href="http://nautil.us/issue/89/the-dark-side/why-mathematicians-should-stop-naming-things-after-each-other">Nautilus</a>)

# The Problem

Lets say you have have an array $nums$ of $n = 15$ integers.

`[-1, 0, 4, 5, 2, 3, 6, -19, 2, 7, 0, 1, 0, 0, -1]`

You are designing a data structure which supports 2 operations.

1. Read range sum - sum from an index $i$ to an index $j$ - $rangeSum(i,j)$
2. Update value at an index $i$ with value $v$ - $update(i,v)$

# Intuition

A very basic approach is to do $rangeSum(i,j)$ in $O(n)$ time, where you sum all $n$ elements. Update would be $O(1)$. Space would be $O(n)$ for storing the array.

```python
class RangeSum(object):
    def __init__(self, nums):
        self.nums = nums[:]
    
    def rangeSum(self, i, j):
        return sum(self.nums[i:j+1])
    
    def update(self, i, v):
        self.nums[i] = v
```

If our system was a read-heavy system, we could optimize $rangeSum(i,j)$ by using precomputing prefix-sums. Now our initialisation is $O(n)$ on time, $O(n)$ on space, but our $rangeSum(i,j)$ becomes $O(1)$. 

```python
class RangeSum:
    def __init__(self, nums: List[int]):
        self.sums = [0]*(1+len(nums))
        for i in range(len(nums)):
            self.sums[i+1] = self.sums[i] + nums[i]
        

    def update(self, i: int, v: int) -> None:
        diff = v - (self.sums[i+1] - self.sums[i])
        for k in range(i,len(self.sums)-1):
            self.sums[k+1] += diff 
        

    def sumRange(self, i: int, j: int) -> int:
        return self.sums[j+1] - self.sums[i] 
```

However, it's clear here that we paid the price on $O(n)$ worst-case time on $update(i,v)$. For any update at index 0 would mean computing the whole array again.

## Here's an Idea

What if we were to divide our prefix sums into two.

This way, even if there was a call to $update(0, v)$, you would only update half the elements in worst case.

# Fenwick Trees

Lets take our idea to the world of computers, lets divide array based on indexes with their set bits.

We initialize `sums` with `n+1` indexes. The indexes which are having their first rightmost bit set get copied over to `sums` as-is. <i>(These are actually our "break-points" if you carefully observe)</i>

The indexes with their second rightmost bit as 1 get `sums[i] = sums[i] + sums[i-1]`. And we repeat this process like below:


<iframe height="400px" width="100%" src="https://repl.it/repls/MountainousRotatingAssignment?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals">
</iframe>





