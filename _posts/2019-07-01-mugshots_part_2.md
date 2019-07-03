---
layout: post
title: 'Exploration into Image Generation (Part 2)'
description: "image-to-image - the paper"
author: Stephan
categories: [code, images, paper]
tags: [machine learning, GAN, pix2pix]
featured_image_thumbnail: assets/images/posts/2019/pix2pix_thumbnail.png
featured_image: assets/images/posts/2019/mugshots.png
featured: false
hidden: false
---

## The Paper(s)

For the `mugshot` project, I wanted to get a better understanding of currently used techniques in regards to Generative Adversarial Network (GAN). Here I will just try to focus for now on the paper [Image-to-Image Translation with Conditional Adversarial Networks](https://phillipi.github.io/pix2pix/) out of the University of California, Berkeley, published in 2017. As well the paper [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/) out of the University of Freiburg, Germany, published in 2015.

## Image-to-Image `(also referred to as pix2pix)`

<div style="text-align:center">
{% include image-caption.html imageurl="/assets/images/posts/2019/pix2pix_example0.png" title="Image-to-Image" caption="Example result of a facades labels→photo, compared to ground truth." %}
</div>

### Introduction

The researchers investigated conditional adversarial networks as a general-purpose solution to image-to-image translation problems. In image processing, computer graphics, and computer vision, many issues can be modeled as “translating” an input image into a corresponding output image.

We can reframe the problem by using some terminology we can find in image processing software like Photoshop, for example, morph, blend, blur, enhance, edge-detection, and many more. Let us look at the example above, we have the left side as our input and want to get the right side as our output image. The center image, called “ground truth,” is the image we are training our network on in conjunction with the input image to generate the output image. Another way to look at it, we create a “map” to put on top of our ground truth image and based on the different colored fields grep (learn) the information found in the ground truth image. That gets used then to create our output image. So where we have the lime green color in the input image, we can find in the ground truth image a balcony. Each color greps a different part from the facade, and our network learns what that is to generate our output image. As simple as creating something using lego. Or copy and paste pieces of information from one picture to create a new image. For this particular example, it is essential to note that the different colors in the input image are basically our labels to define parts in the facade. Whereas in other instances, we use maybe the grayscale image to create a colorized output or use a drawing of edges to create a handbag or a shoe. In short, GANs are generative models that learn a mapping from random noise vector $$z$$ to output image $$y$$, $$G: z → y$$[^1].

### Method

To achieve the results shown here, the researchers use a “U-Net”-based architecture for their generator and a convolutional “PatchGAN” classifier for the discriminator. To achieve that the generator produces an output which is not distinguishable from a “real” images the researchers used a conditional GAN to learn a mapping from image $$x$$ and random noise vector $$z$$, to generate an output image $$y$$, $$G: {x, z} → y$$. As described above, by “linking” input image with ground truth image, we define the condition.

#### Network architectures

The generator and discriminator components of the architecture got adapted from the paper “Unsupervised representation learning with deep convolutional generative adversarial networks”[^2], and both use modules of the convolution-BatchNorm-ReLu[^3].

##### Generator

Many solutions utilize to similar problems an encoder-decoder network in which the input is passed through a series of layers that progressively downsample, until a bottleneck layer, at which point the process is reversed. The drawback here that all the information requires to flow through all layers, including the bottleneck.

The Image-to-Image approach added skip connections to bypass the bottleneck, following the general shape of a “U-Net.” Here the researcher added skip connections between each layer $$i$$ and layer $$n-i$$, where $$n$$ is the total number of layers.

##### Discriminator

As the paper makes the point, it is well known that the L2 loss - and L1 - produce blurry results; nonetheless, they correctly capture the low frequencies in the picture. Which turns the focus on the high-frequency structure of an image, and the researchers designed the discriminator architecture - which they termed a _Patch_GAN - that only penalizes structure at the scale of patches.  Here the discriminator tries to classify if each $$N x N$$ patch in an image is real or not. Effectively, as a Markov random field, and therefore, can be seen as a form of texture/style loss.

##### Optimization

The original GAN[^1] paper describes an approach to optimize a network by alternating between one gradient descent step on $$D$$, then one step on $$G$$. And as it is suggested there as well,  they instead train to maximize $$log D(x, G(x, z))$$.

### Experiments

To test the conditional GAN and its generality, the researchers used a variety of tasks and datasets:

* _Semantic labels↔photo_, trained on the Cityscapes dataset [^4].
* _Architectural labels→photo_, trained on CMP Facades [^9].
* _Map↔aerial photo_, trained on data scraped from Google Maps.
* _BW→color photos_, trained on [^10].
* _Edges→photo_, trained on data from [^11] and [^12]; binary edges generated using the HED edge detector [^13] plus postprocessing.
* _Sketch→photo_: tests edges→photo models on human drawn sketches from [^5].
* _Day→night_, trained on [^8].
* _Thermal→color photos_, trained on data from [^7].
* _Photo with missing pixels→inpainted photo_, trained on Paris StreetView from [^6].

### Conclusion

The conclusion the researchers drew from the results presented in that paper suggests that conditional adversarial networks are a promising approach for many image-to-image translation tasks, especially those involving highly structured graphical outputs. These networks learn a loss adapted to the job and data at hand, which makes them applicable in a wide variety of settings.

And for me reading the paper with the “mugshot” project in mind believe that it is possible - hopefully - to get from a front head shot to a side view. For a more in-depth read, I encourage you to read the paper itself.

Next up is a read up (write up) on U-Net.

--------

##### Footnotes

[^1]: I. Goodfellow, J. Pouget-Abadie, M. Mirza, B. Xu, D. Warde-Farley, S. Ozair, A. Courville, and Y. Bengio. Generative adversarial nets. In NIPS, 2014.
[^2]: A. Radford, L. Metz, and S. Chintala.  Unsupervised representation learning with deep convolutional generative adversarial networks. In ICLR, 2016.
[^3]: S. Ioffe and C. Szegedy.  Batch normalization: Accelerating deep network training by reducing internal covariate shift. In ICML, 2015.
[^4]: M. Cordts, M. Omran, S. Ramos, T. Rehfeld, M. Enzweiler, R. Benenson, U. Franke, S. Roth, and B. Schiele. The cityscapes dataset for semantic urban scene understanding. In CVPR, 2016.
[^5]: M. Eitz, J. Hays, and M. Alexa. How do humans sketch objects? In SIGGRAPH, 2012.
[^6]: C. Doersch, S. Singh, A. Gupta, J. Sivic, and A. Efros. What makes Paris look like Paris?ACM Transactions on Graphics,31(4), 2012.
[^7]: S. Hwang, J. Park, N. Kim, Y. Choi, and I. So Kweon.  Multispectral pedestrian detection: Benchmark dataset and base-line. In CVPR, 2015
[^8]: P.-Y. Laffont, Z. Ren, X. Tao, C. Qian, and J. Hays. Transient attributes for high-level understanding and editing of outdoor scenes. ACM Transactions on Graphics (TOG), 33(4): 149, 2014.
[^9]: R.S. Radim Tylecek. Spatial pattern templates for recognition of objects with a regular structure. In German Conference on Pattern Recognition, 2013.
[^10]: O. Russakovsky, J. Deng, H. Su, J. Krause, S. Satheesh, S. Ma, Z. Huang, A. Karpathy, A. Khosla, M. Bernstein, et al. Imagenet large scale visual recognition challenge.International Journal of Computer Vision, 115(3): 211–252, 2015.
[^11]: J.-Y. Zhu, P. Krahenbuhl, E. Shechtman, and A. A. Efros. Generative visual manipulation on the natural image mani-fold. In ECCV, 2016.
[^12]: A. Yu and K. Grauman. Fine-Grained Visual Comparisons with Local Learning. In CVPR, 2014.
[^13]: S. Xie and Z. Tu. Holistically-nested edge detection. In ICCV, 2015.
