---
layout: post
title: Edge Detection & The Canny Edge Detector
tags: 
cover_url: https://apod.nasa.gov/apod/image/1901/TychoSNR_Chandra_960.jpg
cover_meta: 
  Tycho's Supernova Remnant in X-ray | NASA IMAGE OF THE DAY
color_scheme: tango
mathjax: true
mathjax: True
---

<div style="text-align: justify">
<br/>
If you were to attend a Graduate Computer Vision without any experience of Computer Vision at an undergraduate level like me, you would certainly face problems with edge detection and what it really means. They usually start with Canny Edge Detector at any Graduate Course and it makes it hard for many to comprehend how the idea came up in the first place.

<p>

Lets try loading an image into our code. In this example, I'll be taking a photo which I took at the Niagara Falls State Park.</p>

{% highlight c++ %}
    Mat orig_image;
    Mat resized_image;

    try{
        orig_image = imread(IMG_LOC + "niagara.jpg", 1);
    } catch( cv::Exception& e ) {
        const char* err_msg = e.what();
        std::cout << "exception caught: " << err_msg << std::endl;
    }
    resize(orig_image, resized_image, cv::Size(orig_image.cols*0.25, orig_image.rows*0.25));
{% endhighlight %}

If you notice, I also resized it to approximately 25% the image size.  

</div>