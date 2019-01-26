---
layout: post
title: Implementing 2D Convolution From Ground Up
tags: 
cover_url: https://apod.nasa.gov/apod/image/1901/Selfie_InSight_1080.jpg
cover_meta: 
  InSight Lander Takes Selfie on Mars | NASA IMAGE OF THE DAY
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


## The Directions

The first thing that should be clear in your mind when you're even thinking of working with OpenCV or any other image processing library for that matter is the directions

