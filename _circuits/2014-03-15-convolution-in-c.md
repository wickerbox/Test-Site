---
layout: circuits
title: Image Convolution in C
image: /img/thumbs/convolution2.png
description: Extensible convolution framework with examples.
---

<h5>Introduction</h5>

This project performs pixel-by-pixel convolution on a 2-dimensional image of height and width less than 900 pixels. I wanted to really grok convolution and I wanted a program I could run on the command line with a script so I could compare different kernel matrices. 

Apart from the convolution program itself, I wrote a small helper program called GetPixels that uses OpenCV to turn an image into a text file of grayscale pixel values and vice versa. 

This project follows from my previous <a href="#">SystemVerilog</a> implementation but I lack access to a decent SysV simulator and felt C would work just fine. 

The code is available at <a href="http://github.com/wicker/image-processing/">my GitHub repository</a>.

  <img src="https://jenner.smugmug.com/Convolution-and-GetPixels/n-3qNVXF/i-7RBP58r/0/O/i-7RBP58r.png">

<h5>GetPixels</h5>

First of all, the convolution program takes a text file of 8-bit grayscale pixel values with values between 0 (black) and 255 (white). GetPixels is a small OpenCV-based tool to turn a gray or color image into a text file of pixel values and back again. Check out <a href="/2014/03/getpixels/">the full GetPixels instructions here</a>.

Basically, GetPixels takes an input PNG and outputs a text file with the grayscale value of the pixels separated by a space, like this:

164 165 165 165 165 164 165 165 165 164 165 166

At the end, once you've performed convolution with your kernel, GetPixels can take the modified input text file with grayscale pixel values separated by spaces and returns an output image.

Usage: <code>./GetPixels.o operation input-file output-file width height</code>

{% highlight bash %}
<operation> is 1 for image -> text file or 2 for text file -> image
<image file> is the relative path name to an image file
<pixel file> is the relative path name to a text file of pixels
<width> is the width of the image in pixels
<height> is the height of the image in pixels
{% endhighlight %}

To get a text file of the pixels of a 800x600 mountain.png, try this:

./GetPixels.o 1 mountain.png pixels.txt 800 600

To reconstruct an image from that pixel file, try this:

./GetPixels.o 2 mountainReconstructed.png pixels.txt 240 320

Note: if you don't change the pixel values by convolution or other way, then mountain.png and mountainReconstructed.png should be identical. It's a good way to check that the program works.

<h5>Convolution</h5>

The convolution program works by shoving an image's pixel values into an array and iterating over them with a convolution kernel matrix. The program takes arguments to specify input and output file names along with the number of kernels (1 or 2), kernel size (odd numbers between 1 and 17), and kernel coefficients (integers).

Usage:  ./main.o <input.txt> <width> <height> <op> <kernel> <norm> <output.txt>

<code>input.txt</code> is the text file containing grayscale image pixel values.  

<code>width</code> in pixels, same as number of columns in image.  

<code>height</code> in pixels, same as number of rows in image.  

<code>op</code> is (1) a horizontal 1d, (2) a vertical 1d, or (3) custom kernel to be entered by the user.  

<code>kernel</code> is the N in the NxN kernel matrix that must be an odd number between 1 and 17.  

<code>norm</code> is (0) no or (1) yes to normalize (divide by number of coefficients) the image.  

<code>output.txt</code> is the text file to store resulting grayscale image pixel values.  

To perform a custom 2D operation on a 240x320 image with a 5x5 filter kernel that will be normalized:

<code>./main.o input.txt 240 320 3 5 1 output.txt</code>

To perform a 1D horizontal operation on the same 240x320 image with a 3x3 filter kernel that will not be normalized:

<code>./main.o input.txt 240 320 1 3 0 output.txt</code>

<h5>Scripting Example</h5>

I put the following in a script to run a bunch of kernels all in a row. I used the same input image (brokentop.txt) but saved the outputs to their own files so I could reconstruct them with GetPixels at the end. The image is a portrait size of 240x320. I chose to do a single custom 3x3 operation and none of them are to be normalized.

First, I turned the brokentop.png image into a text file.

<code>./GetPixels.o 1 brokentop.png brokentop.txt 240 320</code>

Then I performed the operations, creating six output text files with the results.

<code>./main.o brokentop.txt 240 320 3 3 0 output-horz.txt</code>

<code>./main.o brokentop.txt 240 320 3 3 0 output-sobel-h.txt </code> 

<code>./main.o brokentop.txt 240 320 3 3 0 output-vert.txt</code> 

<code>./main.o brokentop.txt 240 320 3 3 0 output-sobel-v.txt</code> 

<code>./main.o brokentop.txt 240 320 3 3 0 output-45deg.txt</code> 

<code>./main.o brokentop.txt 240 320 3 3 0 output-135deg.txt</code> 


Finally, I reconstructed images from the output files so I could look at them in an image viewer.

<code>./GetPixels.o 2 output-horz.png output-horz.txt 240 320</code>


<code>./GetPixels.o 2 output-sobel-h.png output-sobel-h.txt 240 320</code>

<code>./GetPixels.o 2 output-vert.png output-vert.txt 240 320</code>

<code>./GetPixels.o 2 output-sobel-v.png output-sobel-v.txt 240 320</code>

<code>./GetPixels.o 2 output-45deg.png output-45deg.txt 240 320</code>

<code>./GetPixels.o 2 output-135deg.png output-135deg.txt 240 320</code>


For each operation, the program prompted me to enter the coefficients from left to right by hand but I have a couple options for how to enter them. Using the kernel for a Sobel horizontal operator, I can enter the coefficients all on one line like this,

<code> +1 +2 +1 0 0 0 -1 -2 -1</code> 

or I can enter them by lines like this,

<code>+1 +2 +1</code>

<code>0 0 0 </code>

<code>-1 -2 -1</code>

which can be easier to read. Finally, I can enter them all like this,

<code>+1</code> 

<code>+2  </code> 

<code>+1</code> 

<code>0</code> 

<code>0</code> 

<code>0</code> 

<code>-1</code> 

<code>-2</code> 

<code>-1</code>


so it really doesn't matter. I ended up writing them in the second form in a text file and copying/pasting. That way the test is built before I start and the results come out without needing editing. 

The image on the left is the original brokentop.png. The image on the right is what it's turned into by GetPixels when the text file brokentop.txt is created. 

<div class="convolution">Original image

<img src="/img/project/convolution/results/brokentop.png">
</div>

<div class="convolution">GetPixels result

<img src="/img/project/convolution/results/results-delta.png">
</div>
<div style="clear:both;"></div>

The script lets you quickly do things like compare the six operations above when normalized (flag set to '1') and not normalized (flag set to '0'). Here are the results of the convolution operations when they are not normalized. They're still very high noise, because the only filtering was high pass, which is effectively trying to find every edge in the image. 

<div class="convolution">Horizontal Filter
-1 -1 -1 
2 2 2
-1 -1 -1
<img src="/img/project/convolution/not-normed/output-horz.png">
</div>
<div class="convolution">Sobel Horizontal Filter
-1 -2 -1
0 0 0 
1 2 1
<img src="/img/project/convolution/not-normed/output-sobel-h.png">
</div>
<div style="clear:both;"></div>
<div class="convolution">Vertical Filter
-1 2 -1
-1 2 -1
-1 2 -1
<img src="/img/project/convolution/not-normed/output-vert.png">
</div>

<div class="convolution">Sobel Vertical Filter
-1 0 1 
-2 0 2
-1 0 1
<img src="/img/project/convolution/not-normed/output-sobel-v.png">
</div>
<div style="clear:both;"></div>
<div class="convolution">45 Degrees
-1 -1 2
-1 2 -1
2 -1 -1
<img src="/img/project/convolution/not-normed/output-45deg.png">
</div>

<div class="convolution">135 Degrees
2 -1 -1
-1 2 -1
-1 -1 2
<img src="/img/project/convolution/not-normed/output-135deg.png">
</div>
<div style="clear:both;"></div>

Normalizing, on the other hand, provides a low pass effect and helps to smooth out the resulting image. These images were normalized after the filter. I ran the script in a fresh directory but passed a '1' to the <code><norm></code> flag instead of a 0. You can tell these are much more useful images. 

<div class="convolution">Horizontal Filter
-1 -1 -1 
2 2 2
-1 -1 -1
<img src="/img/project/convolution/normed/output-horz.png">
</div>
<div class="convolution">Sobel Horizontal Filter
-1 -2 -1
0 0 0 
1 2 1
<img src="/img/project/convolution/normed/output-sobel-h.png">
</div>
<div style="clear:both;"></div>
<div class="convolution">Vertical Filter
-1 2 -1
-1 2 -1
-1 2 -1
<img src="/img/project/convolution/normed/output-vert.png">
</div>

<div class="convolution">Sobel Vertical Filter
-1 0 1 
-2 0 2
-1 0 1
<img src="/img/project/convolution/normed/output-sobel-v.png">
</div>
<div style="clear:both;"></div>
<div class="convolution">45 Degrees
-1 -1 2
-1 2 -1
2 -1 -1
<img src="/img/project/convolution/normed/output-45deg.png">
</div>

<div class="convolution">135 Degrees
2 -1 -1
-1 2 -1
-1 -1 2
<img src="/img/project/convolution/normed/output-135deg.png">
</div>
<div style="clear:both;"></div>

<h5>How Convolution Works</h5>

Pixel by pixel, the kernel matrix is placed with the pixel of interest in the center. Each coefficient in the matrix is multiplied by the neighboring pixel it covers. The values are then added up and may or may not normalized. 

Choose the kernel matrix wisely! Kernel matrices are the discrete version of a continuous filtering function and you can develop your own or adapt the common ones. 

In edge cases where the some of the coefficients in the kernel matrix fall off the edge of the image and don't have corresponding pixels to multiply against, this program tracks how many coefficients weren't used and only adds and/or calculates an average for the ones which were. There is no wrapping, mirroring, or other border tricks. The pixels are just ignored.

The program also checks if a value is greater than 255 or less than zero, and bounds the value accordingly. 

<h5>Delta Function</h5>

To produce a control image for each set run, use a delta function. The delta function multiplies all the other pixels by zero so the only value to be output is the pixel itself. Obviously, a delta function should not be normalized.

<div class="convolution">Original image
&nbsp;
&nbsp;
&nbsp;
<img src="/img/project/convolution/results/brokentop.png">
</div>

<div class="convolution">Delta function
0 0 0
0 1 0 
0 0 0
<img src="/img/project/convolution/results/results-delta.png">
</div>
<div style="clear:both;"></div>

<h5>Common Kernels - Rabbit Example</h5>

This is a set of examples showing high and low pass kernels from <a href="http://en.wikipedia.org/wiki/Kernel_%28image_processing%29">the Wikipedia image processing kernel page</a> on a Wikipedia Creative Commons-licensed rabbit image.

The delta image is the original image in grayscale. 

<div class="convolution">Delta
0 0 0
0 1 0 
0 0 0
<img src="/img/project/convolution/results/rabbit-kernel-delta.png"></div>

<h5>Rabbit Example - Low Pass Filters</h5>

Low pass filters like a blur are generally normalized. The most common is a Gaussian distribution, which these are not. To get more of a blur, just enlarge the kernel.  

<div class="convolution">Blur #1 (Normalized)
1 2 1 
2 4 2
1 2 1
<img src="/img/project/convolution/results/output-blur1.png"></div>

<div class="convolution">Blur #2 (Normalized)
1 1 1 
1 1 1 
1 1 1 
<img src="/img/project/convolution/results/output-blur2.png"></div>

<h5>Rabbit Example - High Pass Filters</h5>

High Pass filters are used for edge enhancement, edge detection, embossing, sharpening, or any operation where you want to bring attention to the difference between two pixels. The sharpen operation below is not normalized; the others are.

<div class="convolution">Sharpen
0 -1 0 
-1 5 -1
0 -1 0
<img src="/img/project/convolution/results/sharpen.png"></div>

<div class="convolution">Edge #1
1 0 -1
0 0 0 
-1 0 1
<img src="/img/project/convolution/results/rabbit-kernel-edge1.png"></div>

<div class="convolution">Edge #2
0 1 0 
1 -4 1 
0 1 0
<img src="/img/project/convolution/results/rabbit-kernel-edge2.png"></div>

<div class="convolution">Edge #3
-1 -1 -1 
-1 8 -1 
-1 -1 -1
<img src="/img/project/convolution/results/rabbit-kernel-edge3.png"></div>

<h5>Kernel Sum (Brightness)</h5>

There are three things - luminance (brightness), hue, and saturation. We're working with grayscale values so we ignore hue and saturation. We're left with an 8-bit brightness value. That means that if the sum of our kernel is greater than one, we're going to get pixel values at greater than we started with, which is a brighter image. If the kernel is less than one, we'll get a darker image.

<div class="convolution">Edge (Sum = -2)
0 1 0 
1 -6 1 
0 1 0
<img src="/img/project/convolution/laplace-pos/output-sum-m2.png"></div>

<div class="convolution">Edge (Sum = -1)
0 1 0 
1 -5 1 
0 1 0
<img src="/img/project/convolution/laplace-pos/output-sum-m1.png"></div>

<div class="convolution">Edge (Sum = 0)
0 1 0 
1 -4 1 
0 1 0
<img src="/img/project/convolution/laplace-pos/output-sum-0.png"></div>

<div class="convolution">Edge (Sum = 1)
0 1 0 
1 -3 1 
0 1 0
<img src="/img/project/convolution/laplace-pos/output-sum-p1.png"></div>

<div class="convolution">Edge (Sum = 2)
0 1 0 
1 -2 1 
0 1 0
<img src="/img/project/convolution/laplace-pos/output-sum-p2.png"></div>

<div class="convolution">Edge (Sum = 3)
0 1 0 
1 -1 1  
0 1 0
<img src="/img/project/convolution/laplace-pos/output-sum-p3.png"></div>

<h5>1D Examples</h5>

These two images just have the 1-dimensional horizontal matrix specified. The left image is a blur and the right is a 3x3 edge.

<code>./GetPixels.o 1 brokentop.png brokentop.txt</code>
<code>./main.o brokentop.txt 240 320 1 9 1 1d-blur.txt</code>
<code>./main.o brokentop.txt 240 320 1 3 1 1d-edge.txt</code>
<code>./GetPixels.o 2 1d-blur.png 1d-blur.txt 240 320</code>
<code>./GetPixels.o 2 1d-edge.png 1d-edge.txt 240 320</code>

<div class="convolution">9x9 Blur (Normalized)
1 1 1 1 1 1 1 1 1 
<img src="/img/project/convolution/results/1d-blur.png"></div>
<div class="convolution">3x3 Edge (Normalized)
-1 0 1
<img src="/img/project/convolution/results/1d-edge.png"></div>

