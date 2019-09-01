---
layout: post
title: 'Lambda'
description: "python lambda"
author: Stephan
categories: [code, python]
tags: [coding]
featured_image_thumbnail: assets/images/posts/2019/python_ml.jpg
featured_image: assets/images/posts/2019/python_ml.jpg
featured: false
hidden: false
---

Python supports the creation of anonymous functions called **lambda**. It is often used in conjunction with functions like **filter()**, **map()** and **reduce()**.

The general form of **lambda**'s are:

```python
lambda arg1, arg2, ...argN : expression using arguments
```

<!-- The following example shows the difference between a normal function definition, *multiply* and a lambda function, *lambd*: -->

Consider the following example of the function **multiply()**:

```python
def multiply(x, y):
    return x * y
```

Because of the simplicity of the function we can convert it to a lambda function.

```python
lambd = lambda x, y: x * y
```

To note here is, because **lambda**'s are also known as "throwaway functions", it is ok to use `_` as its "name".
