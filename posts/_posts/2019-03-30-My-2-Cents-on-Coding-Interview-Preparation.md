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

Coding interviews can be hard - and with emerging hiring techniques in the bay-area companies like _bar-raising_ rounds, it might indeed sound brutal. To me personally, I had a moral blocker when I was faced with the need to solve a never ending list of Leetcode problems - most of which sounded like puzzles. I always used to think that you should be a _good_ programmer - not a robot who knows how to solve puzzles!

My first exposure to these coding interviews was in Fall of 2016 - when I was hunting for an internship for the summer of 2017. I had no prior experience - neither work, nor coding interviews. I was lucky enough to get a few calls from companies in Silicon Valley and a few startups in NYC, thanks to the good reputation Stony Brook has amongst the top CS programs. 

But that wasn't enough. Cracking the GRE, getting a good GPA in the undergrad, toiling through the not-so-easy Graduate Courses, and then the tech industry expects you to have solved another 800 or so puzzles? Bleh.

Well, time passed and I mostly ignored coding interview practice. I was lucky enough to get a job offer from an investment bank in NYC with some basic Data Structures and Algorithms knowledge that I had gained as a part of my undergrad, and some naïve interview preparation.

A few weeks into my job, and I start getting calls - first of them being Apple. And out of nowhere, I find myself barely being able to solve the Two Sum problem. Needless to say, I could only last a couple of phone screens. If you are new to this, let me tell you - the big N, (or as they call them "FAANG" on the forums)require you to go through a coding assessment, which is followed by a couple of phone screens (a mix of coding plus some background questions), after which they call you on-site if they feel you're worth it - and then you basically go through anywhere between 4 to 7 rounds - a mix of one-on-one or two-on-one before you finally get an offer. All of these interviews have a 50-100% coding part where they ask a bunch of questions from [HackerRank](https://www.hackerrank.com) and the most celebrated [Leetcode](https://www.leetcode.com). The quality of questions asked and the competency expected varies from brand to brand. If you are talking the likes of Google or Netflix - you're not getting anywhere without having a perfect score in all rounds which have a mix of "LC Hard" and "LC Medium" questions. 

Earlier this year, after the Apple debacle, I was approached by an Amazon recruiter, and was lucky enough to be able to qualify the coding assessment and the preliminary rounds - and was called onsite at the Seattle HQ for a position in New York City. I had less than a month to prepare, and I was looking at a list of ~400 questions for Amazon on Leetcode.

It was hard, it was rough. The first few days were really pathetic and de-moralizing given that it had been a year since I had graduated and I had never before seriously taken up these coding problems. Coming back from work, already tired and then solving these problems, then realizing I couldn't do better than brute-force was such a slap on my face. But I kept going - I identify as a lazy but otherwise hard-working person, but when I am into it - I give my best. Needless to say - the high reward of a potential SDE position at FAANG kept my enthusiasm alive all the time. More than that, I also had some peer-pressure. Everyone at my grad school was making more bucks than me - and while I don't identify as someone who constantly whines looking at my neighbours (in fact, if you look at my instagram for 2018, you'd realize I was living a life!); a feeling was eating me from the inside. 

It was just this sinking feeling that there were opportunities at my door, and I was acting like a douchebag by just being completely oblivious and staying in my comfort zone. 

In the little hours I had, I managed to solve near about 200 problems, although most of them were Easy-Medium. And in doing so, I learned a lot of lessons. I couldn't find these lessons anywhere on the internet and so at the end of it, I decided to write a blog post on it. 

In the first part, I'll help you overcome your motivation of not doing it, and then, I'll give you some *very helpful* tips that will help you get the most out of it.

# Why should I bother doing LC

## 1. It's not about solving puzzles

As I previously mentioned, I just found it stupid to be judged by a couple of coding puzzles. And to be honest, my belief has largely remained unchanged. It doesn't make you the smartest person on the earth. But it _does_ make you a little smarter, and it _does_ make you a better programmer. I learned so much more about Python - my go-to programming language than ever before. I learned so many ways of looking at problems, and gained a better perspective of what I had studied in Algorithms class. In fact I wish this was a part of the curriculum at school - not the whole thing but at least a part of it. 

And not to mention, *it made some coding interviews laughably easy*. It _does_ help a lot. Once you've solved enough of these tricky problems, your mind magically develops this "algo mind". 

## 2. It's hard because it's not "natural"

Your brain doesn't work on algorithms, instead, it works on probabilities and years of training and evolution. It's essentially a very complex neural network. So when I am looking at the Two Sum problem, or the Rotting Oranges problem, I'm not maintaining a stupid hashmap, or unconsiously running a BFS at the back of my mind without me actually knowing about it. Rather my brain is just running through a lot of permutations really really quick and running a hit-and-trial with some heuristics which activate neurons which get me an answer which might or might not be correct. 

But when coding, we're actually finding a deterministic solution to the problem, a well structured and optimised list of steps which can get me the answers instead of rummaging through a bunch of hit and trials. That makes it hard. But there's a good news too - your brain is capable of learning how to come up with these steps too - just like you were never taught how to board a flight but you actually figured it out naturally, your brain will, after a while get better at figuring out solutions to compile an algorithm to these problems. It's just a matter of time.


## 3. It gives you a lift

Once you've done close to 300 questions, you'll have a upper hand at these dreaded interviews. It'd only be a matter of time that you get through one of these. It not only takes away a lot of stress, it makaes these interviews laughably easy. You can focus on other parts of your profile like projects, system design, etc. 

If the above three don't motivate you enough, I don't know what can. I'd say at least give it a try. After solving 30 problems or so, you'll actually start enjoying it!

Now comes the question of how do I make the most out of it? This is *very important* because it's possible that you do all these questions and still fail at the interviews, it still is - and while it doesn't necessarily mean that you have low IQ, it's often a mix of reasons - the hiring is optimised for low false positives, which means from a company's side of things, *it's fine to reject a few that were otherwise good and might in fact be better than the ones that were finally hired*. The goal is to NOT have someone who wasn't what they needed. It doesn't mean that if you are what they need, you'll get through. But more often than not, it might be that you had other reasons - and maybe one of them is that you didn't do this coding exercise just quite the way it was meant to be done. This is where I think this will help you out.

# Solve easy problems first
Patience and Persistence are the key to success. The Taj Mahal wasn't built in a day. It is very easy to get tempted to solve HARD problems and just "prove yourself" - but believe me - that doesn't help at all. Do the easy ones first, this will do two things - First, it will warm you up for the heavy lifting. Second, it will boost your confidence so that you gradually ascend and don't give up in middle. Third, it will brush up your basics that will prove to be super helpful for Medium and Hard problems.

# Lists are your friends
Yes they are - and I think this is why I gave up doing LC in past. I just aimed at starting serially from 1 towards 800. But that isn't humanly possible. It just isn't. Instead, I bought the Premium subscription for the year (it pays of many times its value so that's a no-brainer) and started with the Top 100 Amazon List. I used to solve around 4 to 8 problems a day and increased them to 10 - 15 on the weekends. Once I was done with a list, I started with Medium Top 250 and so on. It helped me maintain a to do list, and also geared up my confidence knowing that I'm already done with a certain percentage of highly probable questions already.

# Do it the Feynman Way
In the early 1960s, Richard Feynman gave a series of undergraduate lectures that were collected into a book called the Feynman Lectures on Physics. Feynman was well known for simple explanations of scientific concepts that result a in deeper understanding of the subject matter. A note at the top of his blackboard read:

> What I cannot create, I do not understand.

This is SUPER IMPORTANT. If you can't explain it to someone else - believe it or not - you don't understand it, and you'll most likely fail to remember it in the stressful setting of an interview. When you solve a problem, work your way through the problem yourself on a whiteboard. I used an incredibly useful app called [GoodNotes](https://www.goodnotes.com/) on my iPad - it is $7.99 on the [AppStore](https://itunes.apple.com/us/app/goodnotes-5/id1444383602?ls=1&mt=8) but worth a thousand times that.

I created a notebook on the App and by the time I had completed my preparation, it had a whopping 200 pages of scribbles, pseudo-codes, drawings (and some doodles!).

# Beware of Hydroplaning


