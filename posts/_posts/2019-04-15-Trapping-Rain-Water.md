---
layout: post
title: Trapping Rain Water
tags: Coding Python Interview Programming Career Arrays
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_1898.jpg
cover_meta: The NYC Skyline (c) AD Photography
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

We're given an array of length $n$ consisting of non-negative integers which represent an elevation map - just like blocks of buildings where the width of each bar is 1, we need to compute how much water will be trapped after it rains.

## Intuition

This is a fairly common problem. Imagine you have the following array (no block representing $0$):

<img src="https://github.com/alivcor/lightforest/raw/master/TrappingWater1.png"/>

Consider the water trapped in the gap between the blocks of size $2$ and $3$. In that gap, there are two blocks each of height $1$ and one empty block. Consider the water above the block of size $1$ - It's 1 block of water. 

But in the empty block in the same gap, we have trapped 2 blocks of water. It's clear that **the amount of water trapped in a block depends on the height of the block itself. The lesser the height, the more water it can trap**

Also notice that the maximum water trapped in any of the blocks in this gap is less than or equal to $2$. Now, what if I were to increase the height of the block on left side of the gap from $2$ to $4$.

<img src="https://github.com/alivcor/lightforest/raw/master/TrappingWater1.png"/>

We can now see that we have an extra block of water above three blocks in the gap. Only that now, the maximum has increased to $3$. This implies that **the amount of water trapped in a block depends on the heights of the minimum block sizes on either sides of it**. 

A simple lookaround these two facts should be able to get us to the following equation:

$$ water_i = min(left\_max_i, right\_max_i) - height_i$$