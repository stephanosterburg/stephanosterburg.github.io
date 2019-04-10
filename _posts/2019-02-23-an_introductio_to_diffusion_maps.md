---
layout: post
title: A short introduction to Diffusion Maps
description: "An interpretation of the paper"
author: stephan
categories: [data, code, diffusion maps, papers]
image: assets/images/diffusion_map.png
featured: false
---

The [paper](https://inside.mines.edu/~whereman/talks/delaPorte-Herbst-Hereman-vanderWalt-DiffusionMaps-PRASA2008.pdf) published by J. de la Porte[^4], B. M. Herbst[^4], W. Hereman[^5] and S. J. van der Walt[^4] in November 2008 describes the mathematical technique for dealing with reducing dimensionality in data. In that paper, the authors give an overview of three well-known dimensionality reduction techniques and introduce diffusion maps and its functionality, process and how it compares with the other methods.

[^4]: Applied Mathematics Division, Department of Mathematical Sciences, University of Stellenbosch, South Africa
[^5]: Colorado School of Mines, United States of America


### Dimensionality Reduction

We are limited in visualising information beyond the third dimension. For simplicity reasons, imagine a black and white image (grey scale) of 100 by 100 pixels, where each pixel represents a variable. The dimensionality would add up to be 10,000. A **human** would not have any issues with reading the following digits in the picture below, but a **computer** sees them as a data point in an _nm_-dimensional coordinate space.


![Dimensionality Reduction]({{ site.baseurl }}/assets/images/Two-images-of-the-same-digit-at-different-rotation-angles.png)


Dimensionality reduction is about converting data of very high dimensionality into data of lower dimensionality while preserving the original information. We can achieve these if one would be able to measure the distance between data points on the manifold itself instead of in Euclidean space by taking into account the data global space.

We can use the analogy of zipping text files, compressing large text data into smaller equivalent data. With the minor but significant difference that in dimensionality reduction we will lose some information. The goal here is to not lose too many of the essential features in the data.


### Popular Techniques

The most popular reduction methods are

- Principle Component Analysis (PCA)[^6], a linear dimensionality reduction technique using Singular Value Decomposition of the data to project it to a lower dimensional space.[^7]
- Multi-dimensional Scaling (MDS)[^8] seeks a low-dimensional representation of the data in which the distances respect well the distances in the original high-dimensional space.[^9]
- Isometric Feature Map (Isomap)[^10], a non-linear dimensionality reduction through Isometric Mapping.[^11]

[^6]: I.T. Jolliffe. Principal component analysis. Springer Verlag New York, 1986.
[^7]: <https://scikit-learn.org/stable/modules/decomposition.html#pca>
[^8]: W.S. Torgerson. Multidimensional scaling: I. Theory and method. Psychometrika, 17(4):401–419, 1952.
[^9]: <https://scikit-learn.org/stable/modules/manifold.html#multidimensional-scaling>
[^10]: Joshua B. Tenenbaum, Vin de Silva, and John C. Langford. A Global Geometric Framework for Nonlinear Dimensionality Reduction. Science, 290(5500):2319–2323, 2000.
[^11]: <https://scikit-learn.org/stable/modules/manifold.html#isomap>


### Diffusion Maps

Diffusion maps are a non-linear technique. Similar to taking a random walk on our data, where we jump between data points in feature space (see image below), we  are analysing the connectivity of the data. The goal is to reveal the geometric structure of our data at different scales, defining a "time-dependent" diffusion metrics.


![Diffusion Maps]({{ site.baseurl }}/assets/images/A-random-walk-on-a-data-set-Each-jump-has-a-probability-associated-with-it-The-green.png)

By measuring the distances between data points (i.e. Euclidean space), we can define neighbourhoods. In order to achieve it, we define a threshold value in the way of a function (kernel) to obtain the similarity within the geometric structure, which allows us to preserve the underlying information of the given data and be able to connect each neighbourhood.

The basic diffusion mapping algorithm can be defined as follows:


INPUT: High dimensional data set

1. Define a kernel, $$k(x, y)$$ and create a kernel matrix, $$K$$, such that $$K_{i,j} = k(X_i, X_j)$$.
2. Create the diffusion matrix by normalising the rows of the kernel matrix
3. Calculate the eigenvectors of the diffusion matrix.
4. Map to the $$d$$-dimensional diffusion space at time $$t$$, using the $$d$$ dominant eigenvectors and -values

OUTPUT: Lower dimensional data set

### Comparison

The authors also investigated the performance of the different algorithms using a data set with an underlying C-shape for the change of the parameters. As for PCA, once the data is projected onto the two primary axes of variation the ordering of the data along the non-linear C-shape is lost and fails to detect a single parameter. MDS makes a better job in preserving data points in Euclidean space but due to the non-linear structure of the data, MDS cannot preserve the underlying clusters.

### Conclusion

Diffusion mapping is more robust to noise perturbation and is the only technique that allows geometric analysis at different scales.

---

##### Footnotes
