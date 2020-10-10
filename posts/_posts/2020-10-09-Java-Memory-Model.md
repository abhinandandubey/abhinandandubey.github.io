---
layout: post
title: Java Memory Model
tags: 
cover_url: https://source.unsplash.com/random?chip
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

# Why study the memory model?

It's important to understand the memory model, this is specially true for Java because Java handles its variables in a much different way than non-JVM languages. It even has special keywords like `volatile` which wouldn't make sense without having a proper knowledge of how these variables take shape inside the memory.

# The Java Memory Model

![](../../assets/images/2020-10-09-21-18-48.png)

Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
