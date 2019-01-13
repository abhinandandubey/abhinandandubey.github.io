---
layout: post
title: Edge Detection & The Canny Edge Detector
tags: ComputerVision OpenCV EdgeDetection
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

<h2>The Math</h2>

The thing that probably bites people here is _ The Discretization of Continuous Functions_ as I like to call it. Recall from high school calculus that a unary function is said to be differentiable at $x$ if _(NOT "iff")_ the following derivative exists:

$$f'(x) = \frac{d}{dx}f = \lim _{h\to 0}{\frac {f(x+h)-f(x)}{h}}  $$

On paper, sure, you can have infinitesimal " $h$ " - but in the world of computers, where everything is discrete, well you need to have some sort of _"step"_ to be able to define a differential properly.

Talking of images as a function of $x$ and $y$, we are further limited by pixel-level accuracy and can only set $h = 1$. But for symmetry, we could possibly divide this "step" of 1 write the above equation as

$$I_x(x,y) = \frac{I(x+1, y) - I(x-1, y)}{2*1}$$


Notice that we now divide by $2*1$ because we take a step of 2 for symmetry. The resulting matrix representation of the kernel is as follows:

$$ \begin{bmatrix}
    0 & 0 & 0 \\
    -1 & 0 & 1 \\
    0 & 0  & 0
\end{bmatrix} $$

</div>

_Suggested Reading : Sec 2.3.3 Basic Edge Detectors, Reinhard Klette - Concise Computer Vision p.62-64_