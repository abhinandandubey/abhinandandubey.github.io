---
layout: post
title: Largest Rectangle In Histogram
tags: Coding Interview Programming Career Stack
cover_url: https://github.com/alivcor/lightforest/raw/master/logan_sunset.jpg
cover_meta: Sunset (c) AD Photography
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

## The Problem

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.


## Intuition

Lets take the example `[2, 1, 5, 6, 2, 3]`

<img src="https://github.com/alivcor/lightforest/raw/master/largest1.png"/>


Lets start by thinking of a brute force, naive solution. Pick two bars and find the maxArea between them and compare that to your global maxArea. To do that, you'll need to find the bar that "restricts" the height of the forming rectangle to its own height - i.e; the bar with the minimu height between two bars.

For instance, between bars at positions `2` and `5`, the bar at position `4` decides the height of the largest possible rectangle, which is of height `2`.

<img src="https://github.com/alivcor/lightforest/raw/master/largest1_1.png"/>

The brute-force will thus require two pointers, or two loops, and another loop to find the bar with the minimum height. This gives us a complexity of $O(n^3)$

But we could do better. When we move our right pointer from position `4` to `5`, we already know that the bar with minimum height is `2`. So when we move the right pointer to `5`, all we have to do is compare `2` with `3`. This reduces our complexity to $O(n^2)$