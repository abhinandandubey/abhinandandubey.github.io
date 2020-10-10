---
layout: post
title: Median of Two Sorted Lists
tags: Interview Programming
cover_url: https://github.com/alivcor/lightforest/raw/master/spo.jpg
cover_meta: The Observation Deck at The Space Needle (c) AD Photography
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

Given two sorted arrays of size $m$ and size $n$, find the median of the merged array in $\mathcal{O}(log(min(m, n)))$ time.

## Intuition

The naive solution would be to merge arrays and find median which takes linear time. We can do better than that. How? 

The naive solution performs comparisons linearly on all elements it would take $\mathcal{O}(min(m, n))$. But we don't really have to do all those comparisons, the only comparison we need to make are the ones which help us separate the merged array into two halves, middle of which is our median.

If your first array has $m$ elements, and the second one has $n$ elements, your merged array will have $m+n$ elements.

If I were to partition the first array into $x$ and $m-x$ elements, what does that leave me for performing a partition on the second array? Well, we need to part the second array in a way that both left and right halves together of both arrays have same number of elements (or the left has one less). So our second array has to be partitioned at $(m+n)/2 - x$ _Think!_. It should now be clear that the first array here dictates where to partition the second array to maintain the equality constraint.

However, median also needs the array to be sorted, so a partition which violates this constraint, isn't acceptable to us. Hence the second constraint - _All elements on left sides should be less than or equal to the ones on the right side_. 

Since our arrays are already sorted, all we need to check are the border elements.

Lets take an example of following arrays

<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted1.png"/>

If you have read solutions elsewhere, it might have puzzled you why would we flip the arrays? This is just to ensure that we always perform the binary search on the smaller array. If you don't understand this part, it shall be more clear later. Hold on to that thought!

Since second one is shorter, lets flip.

<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted2.png"/>

Lets number the indices of the array. The first array now dictates where to partition.

<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted3.png"/>

Lets Partition - since size of first array is $m$, we partition at $m/2 = 5/2 = 2$. This implies that there are two elements on **left side of the first array**. 

<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted4.png"/>

What does that leave for second array? $3$ on the left side, so that total on left side (in blue) = $5$ which is **same or lesser** than the number of elements on right side.


<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted5.png"/>

Before we jump onto comparing the border elements, lets name them for sake of clarity. Also, just notice for now how we move the window, it becomes more clear int the next step.


<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted6.png"/>

We need to make sure 2 things here:

1. `maxLeftX <= minRightY`
2. `maxLeftY <= minRightX`

If not, we need to check whether our partition on $X$ - (the first array _aka_ `nums1`) needs to move left, or right. To do so, we just need to check how we're doing on point (1) above. If `maxLeftX > minRightY` then we're basically having too large elements in the first array, and we should shift our partition point to the left, otherise, to the right.

As shown in the above, we ask ourselves whether $16 <= 9$ **and** $7 <= 20$. False. Since $16$ (`maxLeftY`)is too big of a number than $9$, we need to move our partition to the **right** so that `minRightX` becomes larger or equal to that.

<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted7.png"/>

We perform a similar check on new partitions to know that we've finally satisfied all our constraints. Thus median is `max(maxLeftX, maxLeftY)` since the total $m+n$ is odd, and we have a single middle number (we do not need to average). Thus,


<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted8.png"/>

It is also helpful to see it visually, how it'd have looked - had we opted for the linear time approach:

<img src="https://github.com/alivcor/lightforest/raw/master/mergesorted9.png"/>


## Implementation

```java
 public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        if(nums2.length < nums1.length){
            // To ensure that we always run search on the lesser devil
            return findMedianSortedArrays(nums2, nums1);
        }

        int x = nums1.length;
        int y = nums2.length;

        int low = 0;
        int high = x;

        while(low <= high){
            int partitionX = (low + high)/2;
            int partitionY = (x + y + 1)/2 - partitionX;
            
            // INF is added to cover edge cases when either of the 4 parts become empty.
            int maxLeftX = (partitionX == 0) ? Integer.MIN_VALUE : nums1[partitionX-1];
            int minRightX = (partitionX == x) ? Integer.MAX_VALUE : nums1[partitionX];

            int maxLeftY = (partitionY == 0) ? Integer.MIN_VALUE : nums2[partitionY-1];
            int minRightY = (partitionY == y) ? Integer.MAX_VALUE : nums2[partitionY];

            if(maxLeftX <= minRightY && maxLeftY <= minRightX){
                if((x+y)%2 == 0){
                    return (double) (Math.max(maxLeftX, maxLeftY) + Math.min(minRightX, minRightY))/2;
                } else {
                    return (double) (Math.max(maxLeftX, maxLeftY));
                }

            } else if (maxLeftX > minRightY) {
                high = partitionX - 1;
            } else {
                low = partitionX + 1;
            }

        }
        throw new IllegalArgumentException();

    }
```
_This post is partly based on a video by Tushar Roy [here](https://www.youtube.com/watch?v=LPFhl65R7ww). The aim was to provide a concise, visual representation of the same._




Feeling generous ? Help me write more blogs like this :)  

<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>