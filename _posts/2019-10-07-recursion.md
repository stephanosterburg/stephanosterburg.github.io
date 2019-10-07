---
layout: post
title: 'Recursion - Super Power'
description: "Recursion - Super Power"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2019/coding_challenge.jpg
featured_image: assets/images/posts/2019/coding_challenge.jpg
featured: false
hidden: false
---

Recursion can be tricky to grasp. To demonstrate its power we are using the famous "Tower of Hanoi" problem. The "Tower of Hanoi" is a mathematical puzzle which consists of three towers (pegs) and more than one rings is as depicted in the image below:
  
![Tower of Hanoi]({{ site.baseurl }}/assets/images/posts/2019/tower_of_hanoi.jpg)  

These rings are of different sizes and stacked upon in an ascending order, i.e. the smaller one sits over the larger one. There are other variations of the puzzle where the number of disks increase, but the tower count remains the same.

### Rules
The mission is to move all the disks to some another tower without violating the sequence of arrangement. A few rules to be followed for "Tower of Hanoi" are −

    + Only one disk can be moved among the towers at any given time.
    + Only the "top" disk can be removed.
    + No large disk can sit over a small disk.

Here is a recursive algorithm for "Tower of Hanoi" written in python: 

```python
def move():
    print('Move disc from {} to {}'.format(f, t))

def hanoi(n, f, h, t):
    """
    n = number of discs
    f = 'from' position
    h = 'helper' postion
    t = 'target' postion 
    """

    if n == 0:
        pass
    else:
        hanoi(n-1, f, t, h)
        move(f, t)
        hanoi(n-1, h, f, t)

hanoi(7, 'A', 'B', 'C')

Move disc from A to B
Move disc from A to C
Move disc from B to C
Move disc from A to B
Move disc from C to A
Move disc from C to B
Move disc from A to B
Move disc from A to C
Move disc from B to C
Move disc from B to A
Move disc from C to A
Move disc from B to C
Move disc from A to B
Move disc from A to C
Move disc from B to C
```