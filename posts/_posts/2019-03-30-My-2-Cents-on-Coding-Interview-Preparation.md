---
layout: post
title: My Two Cents on Coding Interview Preparation
tags: 
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_2716.JPG
cover_meta: The Majestic NYC Skyline in Summer (c) AD Photography
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


Coding interviews can be hard - and with emerging hiring techniques in the bay-area companies like _bar-raising_ rounds, it might indeed sound brutal. To me personally, I had a moral blocker when I was faced with the need to solve a never ending list of Leetcode problems - most of which sounded like puzzles. I always used to think that you should be a _good_ programmer - not a robot who knows how to solve puzzles!

My first exposure to these coding interviews was in Fall of 2016 - when I was hunting for an internship for the summer of 2017. I had no prior experience - neither work, nor coding interviews. I was lucky enough to get a few calls from companies in Silicon Valley and a few startups in NYC, thanks to the good reputation Stony Brook has amongst the top CS programs. 

But that wasn't enough. Cracking the GRE, getting a good GPA in the undergrad, toiling through the not-so-easy Graduate Courses, and then the tech industry expects you to have solved another 800 or so puzzles? Bleh.

Well, time passed and I mostly ignored coding interview practice. I was lucky enough to get a job offer from an investment bank in NYC with some basic Data Structures and Algorithms knowledge that I had gained as a part of my undergrad, and some naïve interview preparation.

A few weeks into my job, and I start getting calls - first of them being Apple. And out of nowhere, I find myself barely being able to solve the Two Sum problem. Needless to say, I could only last a couple of phone screens. If you are new to this, let me tell you - the big N - or as they call them "FAANG" on the forumsrequire you to go through a coding assessment, which is followed by a couple of phone screens - a mix of coding plus some background questions - , after which they call you on-site if they feel you're worth it - and then you basically go through anywhere between 4 to 7 rounds - a mix of one-on-one or two-on-one before you finally get an offer. All of these interviews have a 50-100% coding part where they ask a bunch of questions from [HackerRank](https://www.hackerrank.com) and the most celebrated [Leetcode](https://www.leetcode.com). The quality of questions asked and the competency expected varies from brand to brand. If you are talking the likes of Google or Netflix - you're not getting anywhere without having a perfect score in all rounds which have a mix of "LC Hard" and "LC Medium" questions. 

Earlier this year, after the Apple debacle, I was approached by an Amazon recruiter, and was lucky enough to be able to qualify the coding assessment and the preliminary rounds - and was called onsite at the Seattle HQ for a position in New York City. I had less than a month to prepare, and I was looking at a list of ~400 questions for Amazon on Leetcode.

It was hard, it was rough. The first few days were really pathetic and de-moralizing given that it had been a year since I had graduated and I had never before seriously taken up these coding problems. Coming back from work, already tired and then solving these problems, then realizing I couldn't do better than brute-force was such a slap on my face. But I kept going - I identify as a lazy but otherwise hard-working person, but when I am into it - I give my best. Needless to say - the high reward of a potential SDE position at FAANG kept my enthusiasm alive all the time. More than that, I also had some peer-pressure. Everyone at my grad school was making more bucks than me - and while I don't identify as someone who constantly whines looking at my neighbours - in fact, if you look at my instagram for 2018, you'd realize I was living a life!; a feeling was eating me from the inside. 

It was just this sinking feeling that there were opportunities at my door, and I was acting like a douchebag by just being completely oblivious and staying in my comfort zone. 

In the little hours I had, I managed to solve near about 200 problems, although most of them were Easy-Medium. And in doing so, I learned a lot of lessons. I couldn't find these lessons anywhere on the internet and so at the end of it, I decided to write a blog post on it. 

In the first part, I'll help you overcome your motivation of not doing it, and then, I'll give you some **very helpful** tips that will help you get the most out of it.

# Why should I bother doing LC

## 1. It's not about solving puzzles

As I previously mentioned, I just found it stupid to be judged by a couple of coding puzzles. And to be honest, my belief has largely remained unchanged. It doesn't make you the smartest person on the earth. But it _does_ make you a little smarter, and it _does_ make you a better programmer. I learned so much more about Python - my go-to programming language than ever before. I learned so many ways of looking at problems, and gained a better perspective of what I had studied in Algorithms class. In fact I wish this was a part of the curriculum at school - not the whole thing but at least a part of it. 

And not to mention, *it made some coding interviews laughably easy*. It _does_ help a lot. Once you've solved enough of these tricky problems, your mind magically develops this "algo mind". 

## 2. It's hard because it's not "natural"

Your brain doesn't work on algorithms, instead, it works on probabilities and years of training and evolution. It's essentially a very complex neural network. So when I am looking at the Two Sum problem, or the Rotting Oranges problem, I'm not maintaining a stupid hashmap, or unconsiously running a BFS at the back of my mind without me actually knowing about it. Rather my brain is just running through a lot of permutations really really quick and running a hit-and-trial with some heuristics which activate neurons which get me an answer which might or might not be correct. 

But when coding, we're actually finding a deterministic solution to the problem, a well structured and optimised list of steps which can get me the answers instead of rummaging through a bunch of hit and trials. That makes it hard. But there's a good news too - your brain is capable of learning how to come up with these steps too - just like you were never taught how to board a flight but you actually figured it out naturally, your brain will, after a while get better at figuring out solutions to compile an algorithm to these problems. It's just a matter of time.


## 3. It gives you a lift

Once you've done close to 300 questions, you'll have an upper hand at these dreaded interviews. It'd only be a matter of time that you get through one of these. It not only takes away a lot of stress, it makaes these interviews laughably easy. You can focus on other parts of your profile like projects, system design, etc. 

If the above three don't motivate you enough, I don't know what can. I'd say at least give it a try. After solving 30 problems or so, you'll actually start enjoying it!

<hr/> 
Now comes the question of **how do I make the most out of it?** This is **very important** because it's possible that you do all these questions and still fail at the interviews, it still is - and while it doesn't necessarily mean that you have low IQ, it's often a mix of reasons - the hiring process is optimised for low false positives, which means from a company's side of things, *it's fine to reject a few that were otherwise good and might in fact be better than the ones that were finally hired*. The goal is to NOT have someone who wasn't what they needed. It doesn't mean that if you are what they need, you'll get through. But more often than not, it might be that you had other reasons - and maybe one of them is that you didn't do this coding exercise just quite the way it was meant to be done. This is where I think this will help you out.

# Solve easy problems first
Patience and Persistence are the key to success. The Taj Mahal wasn't built in a day. It is very easy to get tempted to solve HARD problems and just "prove yourself" - but believe me - that doesn't help at all. Do the easy ones first, this will do two things - First, it will warm you up for the heavy lifting. Second, it will boost your confidence so that you gradually ascend and don't give up in middle. Third, it will brush up your basics that will prove to be super helpful for Medium and Hard problems.

# Lists are your friends
Yes they are - and I think this is why I gave up doing LC in the past. I just aimed at starting serially from 1 towards 800. But that isn't humanly possible. It just isn't. This time, instead, I bought the Premium subscription for a year - it pays off many times its value so that's a no-brainer -  and started with the Top 100 Amazon List. I used to solve around 4 to 8 problems a day and increased them to 10 - 15 on the weekends. Once I was done with the Top 100 list, I started with Medium Top 250 and so on. It helped me maintain a to do list, and also geared up my confidence knowing that I'm already done with a certain percentage of highly probable questions already.

# Do it the Feynman Way
In the early 1960s, Richard Feynman gave a series of undergraduate lectures that were collected into a book called the Feynman Lectures on Physics. Feynman was well known for simple explanations of scientific concepts that result a in deeper understanding of the subject matter. A note at the top of his blackboard read:

> What I cannot create, I do not understand.

This is SUPER IMPORTANT. If you can't explain it to someone else - believe it or not - you don't understand it, and you'll most likely fail to remember it in the stressful setting of an interview. When you solve a problem, work your way through the problem yourself on a whiteboard. I used an incredibly useful app called [GoodNotes](https://www.goodnotes.com/) on my iPad - it is $7.99 on the [AppStore](https://itunes.apple.com/us/app/goodnotes-5/id1444383602?ls=1&mt=8) but worth a thousand times that.

I created a notebook on the App and by the time I had completed my preparation, it had a whopping 200 pages of scribbles, pseudo-codes, drawings - and some doodles!

# Beware of Hydroplaning

Who says Hydroplaning only happens on wet streets? It can happen when you're LCing too. What I mean by hydroplaning here is that, when you're looking at the solution, or a Solution Code, it is **very easy** to skip small details which make up the solution. For example, maybe that *for loop* is actually running backwards. Maybe the loop is stopping one index before than it normally does. Things like that only come to picture when you implement it yourself. I guarantee you that Hydroplaning will happen. It's unavoidable, it's just how our brains function, they strip off the intricate details to focus on the big picture. So when you're reading a solution code, make sure to implement it yourselves, even if *you think you get it*. This might take 5 more minutes of your time now, but it will save you a whole lot of embarrasement in an interview when you're either confused why it isn't working here when you're doing just that way it was done in *that solution*!

# Your brain is an LRU+LFU Cache

If you haven't solved them already, [LRU Cache Design](https://leetcode.com/problems/lru-cache/) and [LFU Cache Design](https://leetcode.com/problems/lfu-cache/) are pretty famous problems and you're very likely to face either one of them in an interview. But this problem is exactly how your brain works. It remembers the information which is most frequently and most recently used. So it is common for your brain to cut out the pieces of information that are either not accessed frequently, or have been not accessed since long. 

Growing up in a pretty religious home, my father enchants the all 43 verses in the Hindu holy devotional hymn [Hanuman Chalisa](https://en.wikipedia.org/wiki/Hanuman_Chalisa) - every morning. He doesn't miss it a single day. So that meant we all had to hear him enchant it every day - all through my childhood. And even though I never paid attention, I somehow had learned all the 43 verses which I remember to this day - word to word. 

This is not to brag about myself, but rather to show you how your brain functions. Follow this simple rule and you'll magically remember everything you did, because lets face it, [you can't come up with stupid mnemonics for everything Mr. Lieuw](https://www.youtube.com/watch?v=JsC9ZHi79jo) and moreover it's just plain rote-learning if you're doing mnemonics. The rule is to just look at or go through the thing after $x+4$, $x+12$, $x+24$, $x+72$.., where x updates everytime to its new values. Multiply by 3, multiply by 2, multiply by 3, multiply by 2. This makes your brain think that this piece of information is being accessed multiple times, in longer periods of time so it makes sense to retain it in your Long Term Memory. If you keep doing that, you'll be surprised to see how you're able to magically recall all of it without grinding it all at once.

# Intuitive Solutions Last

> Wow that one line solution is so cool. It does 4 bitwise operations and uses list comprehensions on multiple lists and zips them together all in one line

Well, good for the guy who wrote it. But is it for you? *It probably isn't*. Even if you somehow embrace it's beauty right now, you'll regret about forgetting it later. When you're searching for an elegant solution in the "Discuss" tab or somehwere online, try to go with the solution that's elegant but not trimmed short or does some super smart operations. Because even if you learn it, you'll have to explain how you came up with it in the short duration of the interview. Else, it will become pretty obvious that you had seen the question before - which I don't know is good or bad.

# This is easy - but is it In Place?

There might be questions like [https://leetcode.com/problems/move-zeroes/](this) which explcitly ask you modify things in place, and while you could get away with an AC just by doing some copying, it's not good for your interview preparation. You should look at how to do things in place. Pay serious attention to this as interviewers often like to put constraints as follow-up questions - _do it in-place_, do it _iteratively_, _do it with O(1) space_ ...

# Let code explain you how it's working

There are certain problems which do not have detailed explanations, and you might find yourselves in a situation where all you can find online is code, or maybe the explanation is just too complex in itself - maybe it's a [brain-dump](https://whatis.techtarget.com/definition/brain-dump) - not a well-structured article. In such cases, let the code work for you. What I mean by that is [this](https://leetcode.com/problems/sliding-window-maximum/discuss/237611/simplest-on-python-solution-with-explanation/256107). This is what I used to do when I was stuck. It helped - A LOT!

# Some problems are simply ANNOYING

There are some problems like [this](https://leetcode.com/problems/count-and-say/), [this](https://leetcode.com/problems/read-n-characters-given-read4/) and [this](https://leetcode.com/problems/string-compression/), which are indeed annoying. You might even be unlucky to face them in an interview, so it's best to duck it up and just solve them. These questions can usually be characterized by their high like/dislike ratio, and this is a testament to the fact that you're not the only one to find them annoying!

# Let it rust - but don’t let it die

Earlier in this post, I gave you a trick to retain the information and strategies you use to solve problems. This it is fine to wait before you look back and revisit a problem, but don't let that wait be so long that you actually forget what approach you took. When I was solving all of these problems, in the last week of my interview, I had to revise a whole lot of problems, and I did have some trouble recalling the solution even after looking at it. It's best to at least give a cursory look at the problems you have done every weekend or so.

# Details make all the difference

One of the things that make Leetcoding a lot fun is beating what's out there. And just a small tip here. If you are new to competitive programming, it makes a huge difference whether or not your code is writing to `stdout` or you are initializing or using variables that are not really required. See for yourselves:


![Raw Code](https://github.com/alivcor/lightforest/raw/master/LEETCODE1.png)
![Optimised](https://github.com/alivcor/lightforest/raw/master/LEETCODE2.png)

# Start Timing 

Once you think you're able to at least solve the problems, or roughly sketch an approach, you should start timing yourselves. A great way to do so is using an add-on on your Titlebar. I used [Be Focused](https://itunes.apple.com/au/app/be-focused-focus-timer/id973134470?mt=12) - it's Free but also has a Pro version without ads and some other features.

# BUD, FUD, BURT, FURT 

None of the methods of solving a problem are going to work if you haven't practised enough. I find these approaches incredibly stupid, and did not expect something of this sort in [Cracking The Coding Interview](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850), which is otherwise a good book for coding interview preparation. Like it or not, the level of these coding interview has reached its peak - or maybe not - but it's just not possible to crack these without having faced similar problems before. If you belive the contrary, you're naturally smart - no offense, but there is another 99% of population which can only make their way through by old-fashioned hard work. I am one of them.


# The Stone-Age Algorithms

It is highly probable that your interviewer might just ask you some of the very basic programming questions which you may or may not have studied in CS 101. These algorithms must be on top of your head all the time.

1. Euclid's Algorithm
2. Fibonacci Series
3. Binary Search
4. Insert Sort, Binary Insertion Sort
5. LinkedList Reversal

# Money Milking is still good here

All through my preparation, it saddened me to know that some people who are already employees at FAANG or one of these Big N companies have started making money out of this. Countless examples - [InterviewAccelerator](https://www.interviewaccelerator.com/), [interviewkickstart](https://www.interviewkickstart.com), [Careercup Resume](http://www.careercup.com/resume), and a full fledged bootcamp - [outco](https://www.outco.io) - _which is still a little okay as they're an independent entity_, 

This is incredibly sad for two reasons

1. **It's unethical.** You are basically the teacher in school who also holds up evening tuition classes charging students for "an edge above others". Get the analogy? Don't? 

2. **It's immoral** You went through a lot of pain doesn't mean you make it more painful. Instead, maybe write a blog post to help others, or volunteer for teaching college students coming from low-income or community colleges so that they can also dream of working for one of these. If you really want to make money - maybe write a book, but don't make money out of someone who is already going through it!

All these programmes follow the same money making strategy - plans where they take a cut out of your first year's pay or pay thousands of dollars upfront. 

I'd advise you to **stay FAR away from these programs - you CAN do it on your own.**

# 


