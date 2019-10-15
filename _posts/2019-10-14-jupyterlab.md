---
layout: post
title: 'jupyterlab - cell color'
description: "jupyterlab - cell color"
author: Stephan
categories: [math, code, python]
tags: [math, code, python]
featured_image_thumbnail: assets/images/posts/2019/jupyterlab.png
featured_image: assets/images/posts/2019/jupyterlab.png
featured: false
hidden: false
---

How to change the background color of a single cell in a jupyterlab (jupyter notebook)?

The following is a bit of a hack, but it works to set the color of a coding cell. We will define our main function and place it in the top of our notebook.

```python
from IPython.display import HTML, display

def set_background(color):    
    script = (
        "var cell = this.closest('.jp-CodeCell');"
        "var editor = cell.querySelector('.jp-Editor');"
        "editor.style.background='{}';"
        "this.parentNode.removeChild(this)"
    ).format(color)

    display(HTML('<img src onerror="{}">'.format(script)))
```

To use it we add the following line in the top of the coding cell: `set_background('honeydew')`

To use it as cell magic, we can do:

```python   
from IPython.core.magic import register_cell_magic

@register_cell_magic
def background(color, cell):
    set_background(color)
```

...and to use it, we add the following line in the top of the coding cell: `%%background honeydew`

This is test in jupyterlab version 1.1.4 on a MacBook Pro running macOS Catalina.
