---
layout: post
title: 'Branchless Programming'
description: "Branchless Programming"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2020/python_logging.png
featured_image: assets/images/posts/2020/python_logging.png
featured: False
hidden: False
---

What is branchless programming? A branch occurs as soon we are using statements like `if`, `switches`, `conditional operator`, etc. where the CPU has more than one path it can take. If there is for example an `if` statement the CPU prepares (or better guesses) for all possible branches the code might take. By doing that it fills up unnecessarily memory space for ones and after the execution of the given statement the CPU needs to flush the not used path instructions. And that process is slow.

Take the following example:

```python
def Smaller(a, b):
    if a < b:
        return a
    else
        return b
```

Not that there is anything wrong with that and it is a fairly normal function to write. Here is the same function just as branchless:

```python
def Smaller_Branchless(a, b):
    return a * (a < b) + b * (b <= a)
```

In branchless programming we resort to use conditional operators, i.e. `<`. In the very simple example above the conditional parts return either 0 or 1 and therefore the value of either a or b are multiplied by that value.

For example, `a = 7` and `b = 5`, and when we plug in the values, we get:

`return 7 * (7 < 5) + 5 * (5 <= 7)`

which gives us

`return 7 * 0 + 5 * 1`

returning the value `5`.


### Now why is that important?

In "normal" day to day scripting, not at all. But in heavy computational programming, like for gaming or ML, it becomes very quickly an important aspect. Especially when your CPU and memory resources are limited.

That is to say that if we compile our code, the compiler tries its best to do the right thing. But that is a whole different topic.
