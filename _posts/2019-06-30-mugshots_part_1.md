---
layout: post
title: 'Exploration into Image Generation (Part 1)'
description: "Can I use Generative Adversarial Networks (GAN) to generate a head shot from its side (left or right) based on a given picture showing a head from the front?"
author: Stephan
categories: [code, images]
tags: [machine learning, GAN, pix2pix]
featured_image_thumbnail: assets/images/posts/2019/350px-Bertillon_selfportrait_mugshot.jpg
featured_image: assets/images/posts/2019/mugshots.png
featured: true
hidden: true
---

## The Idea (a brief introduction)


No, this is not my original idea. The credit goes to [Adam Chin](http://adamchinstuff.com/), an ex-co-worker and friend.

Given the dataset of [mug shots](https://www.nist.gov/srd/nist-special-database-18) from the National Institute of Standards and Technology (NIST), we have the usual front and side headshots. The question arises what if there is one of the pictures missing? Can we use machine learning,  especially Generative Adversarial Networks (GAN), to fill in the missing image? Can we train a GAN model in such a way that it can generate an image showing the head from the side given a picture showing the head from the front or vice versa?

The primary literature I am going to use in my exploration is
* [noise2noise](https://arxiv.org/abs/1803.04189)
* [U-Net](https://arxiv.org/abs/1505.04597)
* [pix2pix](https://arxiv.org/abs/1611.07004)

There is also a large body of blog posts covering that topic. Jonathan Hui, for example, has a whole series covering [GAN — GAN Series \(from the beginning to the end\)](https://medium.com/@jonathan_hui/gan-gan-series-2d279f906e7b), which I found very helpful.

In a conversation with Adam a while back, he told me that he saw the talk by [Jaakko Lehtinen from NVIDIA](https://on-demand.gputechconf.com/siggraph/2018/video/sig1814-3-jaakko-lehtinen-deep-adaptive-sampling-count-rendering.html) at SIGGRAPH 2018 in Los Angeles, CA talking about their work in Machine Learning and [Noise2Noise: Learning Image Restoration without Clean Data](https://arxiv.org/abs/1803.04189). And in his words:

> ...it kind of blew me away.

The paper [Noise2Noise: Learning Image Restoration without Clean Data](https://arxiv.org/abs/1803.04189) was initially presented at [ICML](https://icml.cc/) and made multiple appearances in SIGGRAPH 2018 talks.

The intro to the paper states
> We apply basic statistical reasoning to signal reconstruction by machine learning -- learning to map corrupted observations to clean signals -- with a simple and powerful conclusion: it is possible to learn to restore images by only looking at corrupted examples, at performance at and sometimes exceeding training using clean data, without explicit image priors or likelihood models of the corruption. In practice, we show that a single model learns photographic noise removal, denoising synthetic Monte Carlo images, and reconstruction of undersampled MRI scans -- all corrupted by different processes -- based on noisy data only.

So what are the next steps in my exploration?
1. Read the papers listed above, mainly the paper [pix2pix](https://arxiv.org/abs/1611.07004)
2. [TensorFlow](https://www.tensorflow.org/beta/tutorials/generative/pix2pix) has an excellent collection of material for the upcoming version 2.0 (currently in beta) you can experiment with.
3. Apply it to the mug shot dataset.

The initial work I have done so far over the past few days included mainly working through the online tutorial about image generation over at TensorFlow. Using that as my starting point, I tried also to implement the architecture shown in the appendix of the Noise2Noise paper. But I guess I am getting ahead of myself here, lets first cover the papers listed above (at least the pix2pix paper), which I should read first, not just skim over it.