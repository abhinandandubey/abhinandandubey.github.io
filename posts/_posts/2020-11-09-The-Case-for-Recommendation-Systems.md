---
layout: post
title: The Case for Recommendation Systems
tags: Machine-Learning 
cover_url: https://source.unsplash.com/random?movie
cover_meta: 
  (c) UNSPLASH
color_scheme: tango
mathjax: true
mathjax: True
---
<style TYPE="text/css">
wrapper { max-width: 800px; }
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>


<style>
blockquote.yellownote {
    border-left: 12px solid #dc0;
    background-color: #ffa;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
blockquote.sidenote {
    border-left: 12px solid #dc0;
    background-color: #ffa;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
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



## Recommendation Systems

Recommendation systems are not a new concept - and they actually find their roots in the concept of critics - which goes back thousands of years, people who would tell us what they thought we should see or not see, where to go for hunting - and as the civilisations emerged - who are the great story tellers or poets, or artists, or actors.

Simply put, they're systems that help people find things when the process of finding the information you need to make choices might be a little bit challenging, because there's too many choices, or just too many alternatives.

Recommenders are heavily about analysis and this evaluation starts with this big dataset of product opinions and perhaps product data, maybe even user demographics and other pieces of information like user ratings or reviews we can pull together for users. We could begin by asking ourselves if user _X_ likes Item _1_ and _2_ and user _X_ likes most of the items liked by user _Y_ - then there's a high chance _Y_ would like Item _1_ - if we have this - those kinds of questions are collaborative questions because they look at what the community of users together do or don't do and the patterns in their behavior that emerge in that context.

## Making Predictions 

We could either make a prediction by looking at the community in general and figuring out patterns, or we could give a prediction using the content itself and how it has been liked by the user himself/herself in the past. The prediction is the estimate of how much we think they're going to like items - usually items that they haven't seen in the past.

There's many cases when we can infer from the user's behavior how much they likely like a particular product. If I'm watching Netflix and I start watching a movie and five minutes into it, I stop watching it, and I never come back, Netflix would probably know I didn't like that movie. If I'm listening to a song on Spotify on the special playlist spotify curates for me weekly - "Discover Weekly" - and I skip within first few seconds of the song, Spotify would learn from this behavior and recommend less songs like the one I skipped.

<img src="https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/2020-11-09-19-06-56.png" style="width: 250px"/>


## User Interaction & Ratings

What's a rating really? I like to see it as an *expression of preference*. Different websites use different rating mechanisms. The e-Commerce giant uses a 5-star rating system, and further feature-specific item ratings. To calculate the overall star rating and percentage breakdown by star, Amazon doesn’t use a simple average. Instead, their system considers things like how recent a review is and if the reviewer bought the item on Amazon. It also analyzes reviews to verify trustworthiness.

<img src="https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/2020-11-09-19-10-20.png" style="width: 250px"/>

Spotify opts for simpler rating system where a song can either be "loved" or be given a "thumbs down". This fits the purpose, because I definitely wouldn't like a music player which asks me to rate on a scale of 1-5 how much I liked a song, every single time I listen to a new song.

Facebook uses likes, and they recently added other ways to capture user ratings which allow a user to react with the emotion they're feeling when they see a "post". This serves two purposes, first that even if facebook doesn't actually use these extra emotions to gauge the kind of ads to display, it still makes the user feel like his/her opinion is valued. Obviously, this feature serves more than that.

From our little study we can be assured that user ratings may or may not be useful, but they definitely help the user feel more engaged with the system, and it makes them feel valued. We can also see how it's not always a good idea to bug user with interactions, and asking for ratings explicitly might be a recipe for a bad user experience.

## Recommender Interfaces

Broadly speaking, recommendation systems can be categorised into three different interfaces - 

- Filtering Interfaces - these take a stream of content like email or news articles and identify the ones that you want.
- Recommendation Interfaces - Top-N Items / Popular Items etc.
- Prediction Interfaces - Evaluation based, predicted ratings.

## Collaborative Filtering

Collaborative filtering emerged as a reaction to the problem that sometimes there's so much content that you don't just want what's on topic, you want the ones that are really good on that topic. Automated Collaborative Filtering, which is the first system that became known as a recommender system started with the GroupLens Project - which is now mostly obsolete except for educational purposes.

## Modeling User Preferences

Modeling user preferences can seem like an easy task, but it's one of the most challenging tasks to begin with. To greater extent, user preferences can be classified into two buckets - 

### Explicit Preferences

If I press the thumbs down button on a song on Spotify or if I downvote an answer on StackOverflow, that's explicit feedback. I'm telling the system that I don't like this. These are mostly ratings, reviews or votes.

### Implicit Preferences

These preferences are often hard to gauge and there are different ways of doing it. Facebook and Instagram do it using *engagement time* - how long you paused scrolling on a post. Netflix would probably do this by how long a movie or show kept me engaged. For eCommerce websites it could be anything from a simple Click, a Purchase or a Follow.

## Cold Start - What to recommend with no data?

This is one of the fundamental problem with recommender systems. If I am a new user on Apple Music, they don't know what to recommend me. How can they curate lists for me? This is generally solved by two ways - just go by the popular vote - you could just be like - Hey here's what most of our users are listening to - maybe you could try this? - the system could also probably take into account the *context* of the user - *A lot of people in New York City are listening to Lady Gaga today and since you happen to be listening from New York City - we think you might be interested in this*.

Another way to approach this is by asking the user to provide his preferences. This could be a short survey which could be a part of new user onboarding. In the case of Apple Music, they could (and they do) ask me what Genres of music I like listening to.




<br/>
Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>
<br/>

