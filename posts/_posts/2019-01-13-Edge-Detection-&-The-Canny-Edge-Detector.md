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
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
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
The thing that probably bites people here is - as I call it - <i>The Discretization of Continuous Functions</i>. Recall from high school calculus that a unary function is said to be differentiable at $x$ if <i>(NOT "iff")</i> the following derivative exists:

$$f'(x) = \frac{d}{dx}f = \lim _{h\to 0}{\frac {f(x+h)-f(x)}{h}}  $$

On paper, sure, you can have infinitesimal "$h$" - but in the world of computers, where everything is discrete, well you need to have some sort of <i>"step"</i> to be able to define a differential properly.

Talking of images as a function of $x$ and $y$, we are further limited by pixel-level accuracy and can only set $h = 1$. But for symmetry, we could possibly divide this "step" of 1 write the above equation as

$$I_x(x,y) = \frac{I(x+1, y) - I(x-1, y)}{2*1}$$


Notice that we now divide by $2*1$ because we take a step of 2 for symmetry. The resulting matrix representation of the kernel is as follows:

$$ \begin{bmatrix}
    0 & 0 & 0 \\
    -1 & 0 & 1 \\
    0 & 0  & 0
\end{bmatrix} $$

<h3>But wait, how will the differential help with the edges?</h3>
Short answer - If your image was really a 2D function of $x$ and $y$, edges are essentially peaks 

Long Answer - Lets start with a rather philosophical question - Stolen from Stanford CS Webpage, this is a famous "Origin of Edges" slide found in almost all Graduate Computer Vision courses. Observe how there are so many ways one could define an edge.
<img src="https://cs.stanford.edu/people/eroberts/courses/soco/projects/1997-98/computer-vision/images/bottle.jpg"/>

So what are edges, really? Probably the best way to put this is to say - edges are reduced set of pixels that define an image for you or to say - are enough for a human to make sense of it.

</div>

_Suggested Reading : Sec 2.3.3 Basic Edge Detectors, Reinhard Klette - Concise Computer Vision p.62-64_