---
layout: post
title: 'Coding Challenge - Part 2'
description: "Python & ML Challenge"
author: Stephan
categories: [code]
tags: [machine learning, python]
featured_image_thumbnail: assets/images/posts/2019/coding_challenge.jpg
featured_image: assets/images/posts/2019/coding_challenge.jpg
featured: false
hidden: false
---

In the previous post I described the two part coding challenge I was given, where I needed to write a function and a theory description for an alternative solution using a neural network:

You are given a data set where each point has 3 values - X, Y, C, where X is the x coordinate, Y is the y coordinate and C is a color (coded either as 0 or 1). For example given the following dataset:

$$
\begin{array}{lcr}
 2.0 &  0.0 &  0 \\
 1.0 &  3.0 &  0 \\
 3.0 &  1.0 &  0 \\
 2.0 &  1.0 &  0 \\
 0.0 &  2.0 &  0 \\
 0.0 &  2.0 &  0 \\
 7.0 &  7.0 &  1 \\
 6.0 &  6.0 &  1 \\
 8.0 &  8.0 &  1 \\
 7.0 &  7.0 &  1 \\
 7.0 &  6.0 &  1 \\
 6.0 &  8.0 &  1 \\
\end{array}
$$

### Problem

The given data set here is small but you could imagine an extremely large data set such as locations on a map, or vertices in a 3D scene. Using a brute force approach still might prove expensive. Neural Networks on the other hand give a quick (?) feed forward look up or answer. How you would design a neural network to solve this problem? For a specific data set with points having 3 values (x, y, c) you want to design a network that takes three inputs $x_i$, $y_i$, $r$ and outputs a __probability of the color being 0__.

Explain what training data you would use, what kind of a network and layers you would build.


### Neural Network

Let's first consider how we, as humans would approach it. If we were given a 3D scene to look at, then it would be a visual problem and we could _solve it_ right away. On the other hand, if we have an array of numbers - as we have above - and we don't know what theses numbers mean and had to produce some number as an answer, then it would be most likely impossible.

[Tensorflow](https://www.tensorflow.org/tutorials/) is excellent in image recognition and therefore, a good starting point for the given problem. We can start with the well known MNIST neural network and see what it would take to tweak it for our use case.

Let's consider our input data:
+ Given a 3D scene, we want to convert our input data to [voxels](https://en.wikipedia.org/wiki/Voxel) in such a way that each training data is a 3D image of size `[n, n, n]`, where `n` is the resolution.
+ Initialize our 3D matrix with zeros, i.e. $np.zeros((3, 3, 3))$.
+ For each data point, we are changing the corresponding voxel to 1.

Since we have a 3D scene, we will have a lot of data points, and our tensor will be of size $[batch, n, n, n]$.

We do the same for our output.

We will start with a simple neural network using maybe 2 or 3 convolution layers (i.e. [tf.keras.layers.Conv3D](https://www.tensorflow.org/versions/r2.0/api_docs/python/tf/keras/layers/Conv3D)).

We will use backpropagation to train our output layer to predict our expected output, which will return a probability graph of where it is in 3D space. With that, we can filter our result for the highest probability value and get the coordinates, which should give us all colors in the given radius and therefore a way to calculate the probability of the color being 0 of a given point in space.

That said, I might stay corrected.
