---
layout: post
title: Respite From Constructor Telescoping
tags: 
cover_url: https://github.com/alivcor/lightforest/raw/master/arch.jpg
cover_meta: 
  Wall Art (c) AD Photography
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


## Telescoping

Lets have a look at the constructor below

```java
public class Car {
  int modelYear;
  String modelName;

  public Car(int year, String name, String color, String manufacturer, double sellingPrice, String carType, Boolean isLuxury, Boolean isImported, String countryOfManufacture) {
    modelYear = year;
    modelName = name;
    ...
  }
}
```

It just looks ugly. And if you were to look at this code, you could figure out it wasn't the work of an Amateur programmer. This is one of those things which make you wonder _why doesn't my code look clean and professional_ (Feel me?)

Believe it or not, there's a term for it - it's called *Constructor Telescoping*. When you load your constructor with a shit ton of parameters, it makes the code look ugly. I, myself have done it many times, and now that I know there's something I can do to avoid it - I am not going down this ugly lane anymore.

### Builder Pattern

