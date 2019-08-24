---
layout: post
title: 'Python Array Libraries'
description: "python 3.7.4 array libraries"
author: Stephan
categories: [code, python]
tags: [coding]
featured_image_thumbnail: assets/images/posts/2019/python_ml.jpg
featured_image: assets/images/posts/2019/python_ml.jpg
featured: false
hidden: false
---


In Python an array can be either of type `list` or of type `tuple` with the constraint that it is immutable. One of the key features of a list is that it can be dynamically resized, values can be deleted or inserted at arbitrary locations in the list.

* Instantiate:
    - `A = [3, 5, 8, 13]`
    - `A = [1] + [0] * 10`
    - `A = list(range(10))`
    - `A = [[1, 2, 4], [3, 5, 7, 9], [13]]` for 2D array

* Basic operations:
    - `len(A)`
    - `A.append(42)`
    - `A.remove(2)`
    - `A.insert(3, 28)`

* Check if a value is in an array:
    - `if a in A:`

* Copy:
    - `B = A` _vs._ `B = list(A)`
    - `copy.copy(A)` _vs._ `copy.deepcopy(A)`

* Methods:
    - `min(A)`
    - `max(A)`
    - binary search for sorted lists:
        - `bisect.bisect(A, 6)`
        - `bisect.bisect_left(A, 6)`
        - `bisect.bisect_right(A, 6)`
    - `A.reverse()`(in-place) _vs._ `reversed(A)`(returns an iterator)
    - `A.sort()`(in-place) _vs._ `sorted(A)`(returns a copy),
    - `del A[i]`(deletes the i-th element)
    - `del A[i:j]`(removes the slice)

* Slicing - given an array `A = [1, 6, 3, 4, 5, 2, 7]`:
    - `A[2: 4]` _returns_ `[3, 4]`
    - `A[2:]` _returns_ `[3, 4, 5, 2, 7]`
    - `A[:4]` _returns_ `[1, 6, 3, 4]`
    - `A[:-1]` _returns_ `[1, 6, 3, 4, 5, 2]`
    - `A[-3:]` _returns_ `[5, 2, 7]`
    - `A[-3: -1]` _returns_ `[5, 2]`
    - `A[1: 5: 2]` _returns_ `[6, 4]`
    - `A[5: 1: -2]` _returns_ `[2, 4]`
    - `A[::-1]` _returns_ `[7, 2, 5, 4, 3, 6, 1]`(reverses list)

* List Comprehension: An in-depth overview can be found [here](https://github.com/ssrosa/list_comprehensions_study_group/blob/master/index.ipynb), written by Steven Rosa.
