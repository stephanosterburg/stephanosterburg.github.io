---
layout: post
title: 'Hackerrank - Minimum Absolute Difference in an Array'
description: "Hackerrank Coding Challenge"
author: Stephan
categories: [code, python]
tags: [coding challenge, python]
featured_image_thumbnail: assets/images/posts/2019/coding_challenge.jpg
featured_image: assets/images/posts/2019/coding_challenge.jpg
featured: false
hidden: false
---

Given an array of integers, find and print the minimum absolute difference between any two elements in the array. For example, given the array `arr = [-2, 3, 4]` we can create `3` pairs of numbers: `[-2, 2]`, `[-2, 4]` and `[2, 4]`. The absolute differences for these pairs are `|(-2) - 2| = 4`, `|(-2) - 4| = 6` and `|2 - 4| = 2`. The minimum absolute difference is `2`.

You can find the challenge [here](https://www.hackerrank.com/challenges/minimum-absolute-difference-in-an-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms).

### Approach

The most direct solution would be to calculate the absolute difference between each element with the elements after it, but the time complexity will be then `O(n²)`. Assuming we are allowed to use quicksort to sort the list, then we can compare each neighbor pair and the new time complexity will be `O(n log n)`.

### Python 3

```python
def minimumAbsoluteDifference(arr):
    # sort array first
    arr.sort()

    # initiate first difference between the first two elements in list
    min_diff = abs(arr[0] - arr[1])

    # iterate over list and compare abs values of neighbors
    for i in range(len(arr)-1):
        diff = abs(arr[i] - arr[i+1])
        if diff < min_diff:
            min_diff = diff

    return min_diff


n = 5
arr = [1, -3, 71, 68, 17]
print(minimumAbsoluteDifference(arr))  # 3
```
