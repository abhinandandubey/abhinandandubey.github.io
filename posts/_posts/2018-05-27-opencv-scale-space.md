---
layout: post
title: The Scale Space
tags: OpenCV Computer-Vision Scale-Space Convolution
cover_url: https://github.com/alivcor/lightforest/raw/master/IMG_2252.JPG
cover_meta: >
  Take My Breath Away, Danh Vo. Copyright © AD Photography
color_scheme: tango
mathjax: true
mathjax: True
---

## What is Scale Space ?

Before we jump straight into what a scale-space is, it is important to know what was the need of its existence in the first place, as with any other concept. This post is a part of a larger tutorial on SIFT, and an effort to implement it from scratch in C++ with as little help from OpenCV as possible.

### SIFT & The need of scale-space

SIFT is one of the major feature descriptors in computer vision. Even with present-day advancements in Neural Networks, SIFT is still being used in some of the papers published in OpenCV, and forms the basics of Computer Vision. 

#### What can I do with SIFT Features

Anything you want! Track objects, find similarities, detect and identify objects - what not! You can even use SIFT features as an input to your machine learning model and do pretty much anything. Think of SIFT as a very generic way of representing an object in real, physical world, as a collection of numbers.



 