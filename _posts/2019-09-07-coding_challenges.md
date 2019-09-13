---
layout: post
title: 'Coding - Part 1'
description: "Python & ML Challenge"
author: Stephan
categories: [code, neural network]
tags: [machine learning, neural network, python]
featured_image_thumbnail: assets/images/posts/2019/coding_challenge.jpg
featured_image: assets/images/posts/2019/coding_challenge.jpg
featured: false
hidden: false
---

I got the following two part coding challenge, where I needed to write a function and a theory description for an alternative solution using a neural network:


You are given a data set where each point has 3 values - X, Y, C, where X is the x coordinate, Y is the y coordinate and C is a color (coded either as 0 or 1).

### Function

You are asked to write a function that will take three values ($x_i, y_i, r$), where $x_i$, $y_i$ are some influence coordinates and $r$ is the influence radius. The function should return a float value between 0 and 1, which tells you the probability that the given input coordinate $x_i$, $y_i$ has a color 0 based on the various samples radius $r$ around $x_iy_i$.


For example given the following dataset:

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

Examples:
+ If we use $x_i = 2.0$, $y_i = 2.0$ and $ r = 2.0$, the probability should be $1.0$ since there are 3 nodes of color $0$ and 0 nodes of color $1$ in the defined radius.
+ If we use $x_i = 7.0$, $y_i = 7.0$ and $ r = 2.0$, the probability should be $0.0$ since there are 0 nodes of color $0$ and 6 nodes of color $1$ in the defined radius.
+ If we use $x_i = 4.0$, $y_i = 4.0$ and $ r = 4.0$, the probability should be $0.6$ since there are 3 nodes of color $0$ and 2 nodes of color $1$ in the defined radius.

```python
import math

def get_dist(x1, y1, x2, y2):
    return math.hypot(x2 - x1, y2 - y1)


def findProbability(X, Y, C, xpos, ypos, radius):
    color = []
    for k, v in enumerate(X):
        dist = get_dist(X[k], Y[k], xpos, ypos)
        if dist <= radius:
            color.append(C[k])

    result = sum(color)/float(len(color))
    print result



X = [2.0, 1.0, 3.0, 2.0, 0.0, 0.0, 7.0, 6.0, 8.0, 7.0, 7.0, 6.0]
Y = [0.0, 3.0, 1.0, 1.0, 2.0, 2.0, 7.0, 6.0, 8.0, 7.0, 6.0, 8.0]
C = [0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1]
xpos = 2.0
ypos = 2.0
radius = 2.0
findProbability(X, Y, C, xpos, ypos, radius)
```

### Neural Network

The given data set here is small but you could imagine an extremely large data set such as locations on a map, or vertices in a 3D scene. Using a brute force approach still might prove expensive. Neural Networks on the other hand give a quick (?) feed forward look up or answer. How you would design a
neural network to solve this problem? For a specific data set with points having 3 values (x, y, c) you want to design a network that takes three inputs $x_i$, $y_i$, $r$ and output a probability of the color being 0.

Explain what training data you would use, what kind of a network and layers you would build.

...to be continued [here](coding_challenge_part_2)
