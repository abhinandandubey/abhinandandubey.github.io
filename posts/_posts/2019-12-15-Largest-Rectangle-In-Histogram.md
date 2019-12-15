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


Lets start by thinking of a brute force, naive solution. Pick two bars and find the `maxArea` between them and compare that to your global `maxArea`. To do that, you'll need to find the bar that "restricts" the height of the forming rectangle to its own height - i.e; the bar with the minimum height between two bars.

For instance, between bars at positions `2` and `5`, the bar at position `4` decides the height of the largest possible rectangle, which is of height `2`.

<img src="https://github.com/alivcor/lightforest/raw/master/largest1_1.png"/>

The brute-force solution thus requires two pointers, or two loops, and another loop to find the bar with the minimum height. This gives us a complexity of $O(n^3)$

But we could do better. When we move our right pointer from position `4` to `5`, we already know that the bar with minimum height is `2`. So when we move the right pointer to `5`, all we have to do is compare `2` with `3`. This reduces our complexity to $O(n^2)$

### Solution Using Stack

I have to be honest and accept that despite numerous attempts at this problem, I found it hard and uneasy to grasp this solution, but I am glad I finally did.

#### Why Stack?

I will try my best to answer this question - 

1. Why could there be a better solution than $O(n^2)$ ? How would we know that ? 

    Because if the length of the array is $n$, the largest possible rectangle has to have a height one of the elements of the array, that is to say, there are only $n$ "possible largest rectangles". So we don't really need to go through every pair of bars, but should rather search by the height of the bar.

2. Our aim is to iterate through the array and find out the rectangle with maximum area. At each step, there are 4 possibilities:
    1. There's a rectangle forming just using the height of the current bar which has an area larger than the maxArea previously recorded.
    2. There's a rectangle forming using the width or entire spread of the area starting from a bar seen long back which has an area larger than the current maxArea
    3. There's a rectangle forming using width and height of recent tall bars which has an area larger than the current maxArea.
    4. There's no rectangle with larger area at this step.

3. In either of these cases, at each step we need the information of previously seen "candidate" bars - bars which give us hope. These are the bars of increasing heights. And since they'll need to be put in the order of their occurence, stack sould come to your mind. 

If it's not clear now, just put a pin to all your questions, and it should become more clear as we walk through the example.

Lets start by adding a bar of height `0` as starting point to the stack. This has no inherent meaning, and is just done to make the solution more elegant.

<img src="https://github.com/alivcor/lightforest/raw/master/largest2.png"/>

The first bar we see is the bar at position `0` of height `2`. It is definitely as "candidate bar" as it gives us hope of finding a larger rectangle, so we just add it to the stack.

The next one we see is the bar at position `1` with height `1`. At this point, we look at the stack and see that the "candidate bar" at the top of the stack is of height `2`, and because of this `1`, we definitely can't do a rectangle of height `2` now, so the only natural thing to do at this point is to pop it out of the stack, and see what area it could have given us.

<img src="https://github.com/alivcor/lightforest/raw/master/largest3.png"/>

This bar started at position `-1`, and ended at position `1`, thus giving a width of $1-(-1)-1 = 1$, and height of $2$ hence we update our `maxArea` to $2$, and now check the next element on top of the stack (to see if that could be popped out as well) - and since it is `0 < 1`, it can't be popped out.

We now append `1` to the stack and move onto position `2` with the bar of height `5`. We don't need to pop out any elements from the stack, because the bar with height `5` can form a rectangle of height `1` (which is on top of the stack), but the bar with height `1` cannot form a rectangle of height `5` - thus it is still a good candidate (in case `5` get spopped out later). We append `5` to the stack, and move forward without any eliminations.

We observe the same thing when we arrive at `6` (at position `3`). We don't need to pop out any elements from the stack, because the bar with height `6` can form a rectangle of height `5` (which is on top of the stack), but the bar with height `5` cannot form a rectangle of height `6` - thus it is still a good candidate (in case `6` get spopped out later). We append `6` to the stack, and move forward without any eliminations.

<b>At this point it should be clear that we only pop from the stack when height of the current bar is lower than the height of the bar at the top of the stack.</b>

<img src="https://github.com/alivcor/lightforest/raw/master/largest4.png"/>

When we reach the bar at position `4`, we realize we can't do a bar of height `6` anymore, so lets see what it can give us and pop it out. The height of this rectangle is `6`, and the 


