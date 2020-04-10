---
layout: post
title: 'Convert a List to a String'
description: "Convert a list to a string in Python"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2020/python_lego.jpg
featured_image: assets/images/posts/2020/python_lego.jpg
featured: False
hidden: False
---

During the last project I worked on, I run into the fun issue of converting Python Lists to String. The following post presents what I have learned and displays the most efficient methods to do so.

Python provides a magical join() method that takes a sequence and converts it to a string. The list can contain any of the following object types: Strings, Characters, Numbers.

While programming, you may face many scenarios where list to string conversion is required. Let's now see how we can use the join() method in such different cases.

## How to Convert Python List to String
As we've told above that Python Join() function can easily convert a list to string, so let's first check out its syntax.

### Python Join() Syntax
The syntax of join() is as follows:

```python
string_token.join( iterable )

Parameters:
iterable => It could be a list of strings, characters, and numbers
string_token => It is also a string such as space'' or comma "," etcetera
```

The above method joins all the elements present in the iterable separated by the string_token. Next, we explore examples using the join() function to convert the list to a string.

### Convert Python List of Strings to a string using join()
Let's say we have a list of month names.

```python
# List of month names
listOfmonths = ["Jan" , "Feb", "Mar", "April", "May", "Jun", "Jul"]
```

Now, we'll join all the strings in the above list, considering space as a separator.

```python
"""
Desc:
 Function to convert List of strings to a string with a separator
"""
def converttostr(input_seq, seperator):
   # Join all the strings in list
   final_str = seperator.join(input_seq)
   return final_str

# List of month names
listOfmonths = ["Jan" , "Feb", "Mar", "April", "May", "Jun", "Jul"]

# List of month names separated with a space
seperator = ' '
print("Scenario#1: ", converttostr(listOfmonths, seperator))

# List of month names separated with a comma
seperator = ', '
print("Scenario#2: ", converttostr(listOfmonths, seperator))
```

The output is as follows:


```python
Scenario#1:  Jan Feb Mar April May Jun Jul
Scenario#2:  Jan, Feb, Mar, April, May, Jun, Jul
```

### Convert a list of chars into a string

With the help of join() method, we can also convert a list of characters to a string. See the example given below:

```python
charList = ['p','y','t','h','o','n',' ',
            'p','r','o','g','r','a','m','m','i','n','g']

# Let's convert charList to a string.
finalString = ''.join(charList)

print(charList)
print(finalString)
```

The output is as follows:

```python
['p', 'y', 't', 'h', 'o', 'n', ' ',
 'p', 'r', 'o', 'g', 'r', 'a', 'm', 'm', 'i', 'n', 'g']
python programming
```

### Conversion of a mixed list to a string using join()

Let's assume that we have got a list of some numbers and strings. Since it is a combination of different objects, so we need to handle it a little differently.

```python
sourceList = ["I", "got", 60, "in", "Science", 70, "in", "English",
              ", and", 66, "in", "Maths"]
```

It is not possible to use the join() function on this list. We first have to convert each element to a string to form a new one, and then only we can call join() method.

```python
# Let's convert sourceList to a list of strings and
# then join its elements.
stringList = ' '.join([str(item) for item in sourceList ])
```

The final string would appear something like:

```python
I got 60 in Science, 70 in English, and 66 in Maths.
```

You can check out the full working code below:

```python
sourceList = ["I", "got", 60, "in", "Science,", 70, "in", "English,",
              "and", 66, "in", "Maths."]

# Let's convert sourceList to a list of strings and
# then join its elements.
stringList = ' '.join([str(item) for item in sourceList ])

print(stringList)
```

### Get a comma-separated string from a list of numbers
If you simply wish to convert a comma-separated string, then try the below shortcut:

```python
numList = [20, 213, 4587, 7869]
print(str(numList).strip('[]'))
```

The output is as follows:

```python
20, 213, 4587, 7869
```

Alternatively, we can use map() function to convert the items in the list to a string. After that, we can join them as below:

```python
print(', '.join(map(str, numList)))
```

The output:

```python
20, 213, 4587, 7869
```

We can even use the new line character (‘\n’) as a separator. See below:

```python
print('\n'.join(map(str, numList)))
```

The result is as follows:

```python
20
213
4587
7869
```
