---
layout: circuits
title: GetPixels
image: /img/thumbs/getpixels.png
description: Get pixel values out of an image. 
---

<img src="https://jenner.smugmug.com/Convolution-and-GetPixels/n-3qNVXF/i-hZRz4Q6/0/O/i-hZRz4Q6.png">

GetPixels converts an image to a text file of grayscale pixel values and reconstructs the image. I wrote it so I could manipulate the value of an image's pixels using convolution kernels and easily look at the results. 

Find the code at <a href="http://github.com/wicker/getpixels">GitHub</a>.

GetPixels is open source software licensed under the <a href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike International</a> license. That means you can share and adapt the work for any purpose as long as you give me credit and share your work alike. 

<h3>Usage</h3>

<code>./GetPixels.o <op> <image.png> <pixels.txt> <width> <height></code>

<code>op</code> is the desired operation. Use '1' for image->pixels and '2' for pixels->image.

<code>image.png</code> is the relative file path to the image and <code>pixels.txt</code> is the text file. 

<code>height</code> and <code>width</code> are the values in pixels of the image.

<h3>Build</h3>

I used OpenCV 2.4.8 and g++ to compile on Linux Mint 15 (Olivia). 

<h3>Failure Cases</h3>

The program will alert you if the height or width is less than 1, if the input image or input pixel file don't exist, or if the operation chosen is not 1 or 2. 

Note: The output files will be created and overwrite any existing files without prompting! 

If you give the program a portrait of dimensions 240x320 but you tell the program it's a landscape of dimensions 320x240, you'll get a broken reconstruction: 

<img src="https://jenner.smugmug.com/Convolution-and-GetPixels/n-3qNVXF/i-3XLkjwM/0/O/i-3XLkjwM.png">



