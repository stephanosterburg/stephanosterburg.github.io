---
layout: post
title: 'Hash Tables: Ice Cream Parlor'
description: "Hash Tables: Ice Cream Parlor"
author: Stephan
categories: [hackerrank, code, python]
tags: [hackerrank, code, python]
featured_image_thumbnail: assets/images/posts/2019/ice_cream_parlor.jpg
featured_image: assets/images/posts/2019/ice_cream_parlor.jpg
featured: false
hidden: false
---

Each time Sunny and Johnny take a trip to the Ice Cream Parlor, they pool their money to buy ice cream. On any given day, the parlor offers a line of flavors. Each flavor has a cost associated with it.

The task is to print two space-separated integers denoting the respective indices for the two distinct flavors they choose to purchase in ascending order.

Have a look at the problem in detail [here](https://www.hackerrank.com/challenges/ctci-ice-cream-parlor/problem).

### Thinking

Our task is to find two numbers in a list that adds up to a specific monetary value. We will create a hash table of cost. We will do two tasks at the same time: while enumerating the list, we will be checking whether a match already exists by checking our hash table, and at the same time, adding the new cost into the hash table if we cannot find a match.

```python
def whatFlavors(cost: list, money: int):
    cost_dict = {}
    for k, v in enumerate(cost):
        if money - v in cost_dict:
            print(str(cost_dict[money - v] + 1) + ' ' + str(k + 1))
        else:
            cost_dict[v] = k
```
