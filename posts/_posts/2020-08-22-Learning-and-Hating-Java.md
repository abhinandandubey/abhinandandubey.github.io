---
layout: post
title: Learning and Hating Java
tags: 
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_3949.jpg
cover_meta: 
  Traveler (c) AD Photography
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

I am trying to keep an open mind and become a good Java Developer, appreciating OOP Concepts, SOLID Design Patterns and all the popular frameworks.

I do have to say Design Patterns are something really cool and every developer should try to learn them. Spring? Not a big fan so far. 

I consider myself a good Python developer and facing harsh realities like there's no alternative to `defaultdict` in Java is definitely not very conducive to my learning experience.

Just about now, I was trying to sort an array of strings and Java literally gave me so much pain. In python, I could have been done with a single liner `words.sort(key = lambda k: len(k))` Boom. Java - it's like preparing for a war !

```java
Arrays.sort(words, (String w1, String w2) -> Integer.compare(
    w1.length(), w2.length()
    )); 
```

As a developer, I can't see any good reason why do I have to introduce two variables when I can do my comparison with just one. More specifically, why can't this work:

```java
Arrays.sort(words, (String w) -> w.length()); 
```

Okay, I made my peace with it. But then, I was trying to print it, and realised Java won't stop there.

```java
System.out.println(words);
// prints : [Ljava.lang.String;@736e9adb WTF
```

I literally have to convert it. I do understand it's not a list, and arrays are old and they don't have those toString methods defined or whatever.

If I had to create a `Map` with word length as key and a list of words as values. Python is such a pleasant breeze:

```python
c = collections.defaultdict(list)
for word in words:
    c[len(word)].append(word)
print(c)
```
Java is probably a thunderstorm:

```java
Map<Integer, List<String>> store = new TreeMap();
for (String word : words) {
    if(store.containsKey(word.length())){
        store.get(word.length()).add(word);
    } else {
        store.put(word.length(), new ArrayList<String>(Arrays.asList(word)));
    }
}
System.out.println(store);
// prints : {1=[a, b], 2=[ba], 3=[bca, bda], 4=[bdca]} 
```        

Whatever happened to `Maps.toString(store)`. Uniformity probably was on vacation when Java was being developed.



