---
layout: post
title: Trapping Rain Water
tags: Coding Python Interview Programming Career Arrays
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_1898_1.jpg
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

<img src="https://github.com/alivcor/lightforest/raw/master/TrappingWater2.png"/>

We can now see that we have an extra block of water above three blocks in the gap. Only that now, the maximum has increased to $3$. This implies that **the amount of water trapped in a block depends on the heights of the minimum block sizes on either sides of it**. 

A simple lookaround these two facts should be able to get us to the following equation:

$$ water_i = min(left\_max_i, right\_max_i) - height_i$$

## Implementation

The intuition seems easy, right? The implementation isn't - the reason being it has two _"gotchas"_.

### 1. What's the first value in `left_max` and the last value in `right_max`?
It should be clear that the length of the arrays `left_max` and `right_max` should be same as length of `height`. It is also clear that `left_max[i] = max(left_max[i-1], height[i])`. For the first element, we can't use this same expression though. In this case, we need to ask ourselves what is this expression really calculating - it's the maximum of all heights seen on the left **including the current one**. Since the ones on the left are none or 0, thus for the first index, the maximum height is the height of the block itself. A similar analogy can be applied to find last value of `right_max` which is basically `height[height.length]`


### 2. How's the final array calculated on the edges?
Since water cannot be trapped on the edge buildings _aka_ blocks, we need to consider only the middle ones.

With the above two _gotchas_ clear to us, the following solution should now seem simple;

```java
public int trap(int[] height) {
    if(height.length == 0){
        return 0;
    }

    int[] left_max = new int[height.length];
    left_max[0] = height[0];
    for(int i = 1; i < height.length; i++){
        left_max[i] = Math.max(left_max[i - 1], height[i]);
    }
    
    int[] right_max = new int[height.length];
    right_max[right_max.length - 1] = height[height.length - 1];
    for(int i = height.length-2; i > 0; i--){
        right_max[i] = Math.max(right_max[i + 1], height[i]);
    }
    
    int ans = 0;
    for(int i = 1; i < height.length - 1; i++){
        ans += Math.min(left_max[i], right_max[i]) - height[i];
    }
    return ans;
}
```


.