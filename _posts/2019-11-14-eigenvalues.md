---
layout: post
title: 'Eigenvalues'
description: "Notes from the Mathematics for Machine Learning course by Imperial College London"
author: Stephan
categories: [code, math]
tags: [machine learning, mathematical]
featured_image_thumbnail: assets/images/posts/2019/eigenvalues.jpg
featured_image: assets/images/posts/2019/eigenvalues.jpg
featured: false
hidden: false
---


The word "eigen" can be translated from German, meaning characteristic. When we are describing an eigenproblem, we are talking about finding the characteristic properties of a vector, for example. However, characteristic of what? For basic 2D _eigen_-problems, we take a transform, and we look for the vectors which are still on the same span after being transformed before we calculate by how much the length of the vector has changed. Which describes basically what eigenvectors and their corresponding eigenvalues are.

$$ Ax = \lambda{x} $$

Here, $x$ describes the eigenvector of the matrix $A$ and the number $\lambda$ describes the eigenvalue of $A$.

```python
import numpy as np

M = np.array([[1, 2], [3, 4]])

eigenvalues, eigenvectors = np.linalg.eig(M)
```
