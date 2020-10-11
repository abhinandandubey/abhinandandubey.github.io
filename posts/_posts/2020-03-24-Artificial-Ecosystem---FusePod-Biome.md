---
layout: post
title: Artificial Ecosystem - FusePod Biome
tags: Project Biology
cover_url: https://github.com/alivcor/lightforest/raw/master/biome.jpg
cover_meta: 
  Ecosystem (c) AD Photography
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

### Thought of an Artificial Ecosystem

Lately, maybe because of the Coronavirus scare, and me binge-watching Westworld on HBO, I've been thinking of making my own Artificial Ecosystem - where life starts at the begining, and there are maps or rather nodes spread across a space. There's a lot of ambiguity on how it would look like, and it definitely makes sense to do some modeling.

I started my research with ideas of modelling my problem, and that started with the idea of a "CELL". But where does it end (how _atomic_ can the model be, or how much _sophistication_ can be expected of it?), and how do cells differ - are human cells same as bacterial cells? Are viruses a type of cell? 

#### Time to research

I started labelling how exactly the world would look like - maybe there will be some cells, the process which makes them evolve into organisms? It does make sense to outline a hierarchy out here.

![LifeLake](https://github.com/alivcor/lightforest/raw/master/lifelake_1.png)

Lets assume we start with a very basic lake with just cells, but how do these cells differ - what are they composed of? So I spent hours trying to learn how cells came up in the first place. 

![LifeLake](https://github.com/alivcor/lightforest/raw/master/lifelake_2.png)

![LifeLake](https://github.com/alivcor/lightforest/raw/master/lifelake_3.png)

It's actually quite interesting to read about the RNA World Theory - and that everything evolved from a huge bowl of soup - containing a variety of elements and the RNA chains. RNA themselves are comprised of Nucleotides - the bonding particles which form the RNA.


![LifeLake](https://github.com/alivcor/lightforest/raw/master/lifelake_4.png)

The Nucleotides (`A`, `T`, `C`, `G`) themselves have two parts - the "bonding part" which bonds with every other nucleotide, and a "pairing part" - which only pairs or bonds with a specific nucleotide. For instance, `A` bonds only with `U` whereas `G` bonds only with `C`.

This way, nucleotides together form chains of different lengths and permutations, which can again pair up with different individual nucleotides. This process is known as _base-pairing_

![LifeLake](https://github.com/alivcor/lightforest/raw/master/lifelake_5.png)

Oh, by the way, these chains are called the RNA, a short for Ribonucleic Acid. The RNA pairs up with nucleotides and generates a complimentary RNA. This complimentary RNA binds with another compliment, making a copy of the original RNA. 

![LifeLake](https://github.com/alivcor/lightforest/raw/master/lifelake_6.png)

But just as nature intended, these copies are not always exact - sometimes mutations occur, thus giving rise to different RNAs.

Moreover, some RNAs self-pair, and they effectively become a machine, which could glue or break two different chains or RNAs. Looking at this whole process from a soup level, the RNAs which can pair up more easily are more likely to generate more copies, and thus survive more. This is where the line between chemistry, and life (or biology) blurs. If an RNA is "trying to survive" should we consider it a life-form?




