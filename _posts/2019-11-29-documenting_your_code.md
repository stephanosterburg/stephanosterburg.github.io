---
layout: post
title: 'Documenting Your Code'
description: "Documenting Your Code"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2019/document_code.jpg
featured_image: assets/images/posts/2019/document_code.jpg
featured: false
hidden: false
---

There is an official specification on how you should format your docstrings, called [PEP 0257](https://www.python.org/dev/peps/pep-0257/). Many people don’t strictly follow this format and use a format that is supported by documentation generation tools like `Doxygen`, `Epydoc`, and `Sphinx`. Formats include (taken from [StackOverflow](http://stackoverflow.com/questions/3898572/what-is-the-standard-python-docstring-format)):

### Formats

Python docstrings can be written following several formats, as the other posts showed. However, the default Sphinx docstring format was not mentioned and is based on reStructuredText (reST). You can get some information about the main formats in this [blog post](http://daouzli.com/blog/docstring.html).

__Note that the `reST` is recommended by the [PEP 287](https://www.python.org/dev/peps/pep-0287).__

---

The following are the main used formats for docstrings.

### - Epytext

Historically a `_javadoc_` like style was prevalent, so it was taken as a base for [Epydoc](http://epydoc.sourceforge.net/) (with the called Epytext format) to generate documentation.

Example:
```python
"""
JavaDoc Style

@param param1: this is the first param
@param param2: this is a second param
@return: this is a description of what is returned
@raise keyError: raises an exception
"""
```

### - reST

Nowadays, the probably more common format is the _reStructuredText_ (reST) format that is used by [Sphinx](http://sphinx-doc.org/) to generate documentation. Note: it is used by default in JetBrains PyCharm (type triple quotes after defining a method and hit enter). It is also used by default as the output format in [Pyment](https://github.com/dadadel/pyment).

Example:

```python
"""
reST Style

:param param1: this is the first param
:param param2: this is a second param
:returns: this is a description of what is returned
:raises keyError: raises an exception
"""
```

### - Google

Google has its own [format](https://github.com/google/styleguide/blob/gh-pages/pyguide.md#38-comments-and-docstrings) that is often used. It also can be interpreted by Sphinx (i.e., using [Napoleon plugin](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/)).

Example:

```python
"""
Google Style

Args:
    param1: This is the first param.
    param2: This is a second param.

Returns:
    This is a description of what is returned.

Raises:
    KeyError: Raises an exception.
"""
```

Even [more examples](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html#example-google)

### - Numpydoc

Note that Numpy recommends following their own [numpydoc](https://numpydoc.readthedocs.io/en/latest/) based on Google format and usable by Sphinx.

```python
"""
My numpydoc description of a kind
of very exhaustive numpydoc format docstring.

Parameters
----------
first: array_like
    the 1st param name `first`
second :
    the 2nd param
third : {'value', 'other'}, optional
    the 3rd param, by default 'value'

Returns
-------
string
    a value in a string

Raises
------
KeyError
    when a key error
OtherError
    when the other error
"""
```

### Converting/Generating

It is possible to use a tool like [Pyment](https://github.com/dadadel/pyment) to generate docstrings to a Python project not yet documented automatically, or to convert existing docstrings (can be mixing several formats) from a format to the other one.

Note: The examples are taken from the [Pyment documentation](https://github.com/dadadel/pyment/blob/master/README.rst)
