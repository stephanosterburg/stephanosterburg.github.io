---
layout: post
title: 'The Hessian'
description: "Mathematics notes #3"
author: Stephan
categories: [math]
tags: [mathematical]
featured_image_thumbnail: assets/images/posts/2019/Hessian_Surface.jpg
featured_image: assets/images/posts/2019/Hessian_Surface.jpg
featured: false
hidden: false
---


The Hessian is additional concept which relates to multivariate systems. In many ways, the Hessian can be thought of as a simple extension of the Jacobian vector. For the Jacobian, we collected together all of the first order derivatives of a function into a vector. Now, we're going to collect all of the second order derivatives together into a matrix, which for a function of n variables, would look like this.


<div style="text-align:center">
{% include image-caption.html imageurl="/assets/images/posts/2019/hessian_matrix.png" title="Tensor" %}
</div>

Hessian matrix will be a n by n square matrix, where n is the number of variables in our function f.

Here is a quick example, where we find the Jacobian first to make our life easier and then differentiate its terms again to find the Hessian.  

First we are going to build the Jacobian, which is going to be denoted as $J$. We first differentiate with respect to x to get $2xyz$, differentiate with respect to y, which is going to get us $x^z$, and finally differentiate with respect to z to get $x^y$.

Using the Jacobian, we can then differentiate again with respect to each of the variables, which will then give us our Hessian matrix.

$$ f(x, y, z) = x^2yz \\\
   J = [2xyz, x^2z, x62y] \\\
   H = \begin{bmatrix} 2yz & 2xz & 2xy \\
                       2xz & 0 & x^2 \\
                       2xy & x^2 & 0 \end{bmatrix} $$
